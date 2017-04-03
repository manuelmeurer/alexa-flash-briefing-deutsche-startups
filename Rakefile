require 'dotenv/load'

task :update_deals_feed do
  require 'nokogiri'
  require 'open-uri'
  require 'digest'
  require 'aws-sdk'

  GERMAN_MONTHS = %w(Januar Februar März April Mai Juni Juli August September Oktober November Dezember)
  DATE_REGEX    = /(?<day_of_month>\d+)\. (?<month>[A-Za-zä]+) (?<year>\d{4})/

  s3 = Aws::S3::Resource.new
  unless bucket_name = ENV['AWS_S3_BUCKET']
    fail 'Bucket name not configured.'
  end
  unless bucket = s3.bucket(bucket_name)
    fail "Bucket #{bucket_name} not found."
  end

  page = Nokogiri::HTML(open('https://www.deutsche-startups.de/ressort/deals/'))
  posts = page.css('.post').take(5)
  items = posts.map do |post|
    url        = post.at_css('a')[:href]
    title      = post.at_css('h3').content.sub(/\ADeal-Monitor/, '').strip
    text       = post.at_css('.text').content.gsub(/\s+\+{2,}\s+/, ' ').strip
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

  bucket.object('deals.json').put body:         items.to_json,
                                  acl:          'public-read',
                                  content_type: 'application/json'
end
