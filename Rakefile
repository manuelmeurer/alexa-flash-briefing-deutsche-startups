require 'open-uri'
require 'digest'
require 'dotenv/load'
require 'bundler'

Bundler.require

if rollbar_access_token = ENV['ROLLBAR_ACCESS_TOKEN']
  Rollbar.configure do |config|
    config.access_token = rollbar_access_token
  end
end

GERMAN_MONTHS = %w(Januar Februar März April Mai Juni Juli August September Oktober November Dezember)
DATE_REGEX    = /(?<day_of_month>\d+)\. (?<month>[A-Za-zä]+) (?<year>\d{4})/
FEEDS = {
          deals:  'https://www.deutsche-startups.de/ressort/deals/',
          ticker: 'https://www.deutsche-startups.de/tag/startupticker/'
        }

task :update_feeds do
  s3 = Aws::S3::Resource.new
  unless bucket_name = ENV['AWS_S3_BUCKET']
    fail 'Bucket name not configured.'
  end
  unless bucket = s3.bucket(bucket_name)
    fail "Bucket #{bucket_name} not found."
  end

  FEEDS.each do |name, url|
    page = 5.tries(delay: 1) do
      Nokogiri::HTML(open(url))
    end
    posts = page.css('.post').take(5)
    items = posts.map do |post|
      url        = post.at_css('a')[:href]
      title      = post.at_css('h3 > text()').content.strip
      text       = post.at_css('.text').content.sub('+++', '').sub('#StartupTicker', '').strip
      date_match = post.at_css('.meta').content.match(DATE_REGEX)
      time       = Time.utc(date_match[:year].to_i,
                            GERMAN_MONTHS.index(date_match[:month]) + 1,
                            date_match[:day_of_month].to_i,
                            posts.index(post))
      {
        uid:            Digest::SHA1.hexdigest(url),
        updateDate:     time.iso8601.sub(/Z\z/, '.0Z'),
        titleText:      title,
        mainText:       text,
        redirectionUrl: url
      }
    end

    bucket.object("#{name}.json").put body:         items.to_json,
                                      acl:          'public-read',
                                      content_type: 'application/json'
  end
end
