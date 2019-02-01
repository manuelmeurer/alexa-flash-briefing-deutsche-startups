# Alexa Flash Briefing Deutsche Startups

Read the [blog post](https://www.krautcomputing.com/blog/2017/04/23/simple-and-powerful-the-alexa-flash-briefing-skill-api/)!

This is the source code for the [Alexa Flash Briefing Skill for Deutsche Startups](https://www.amazon.de/Kraut-Computing-Deutsche-Startups/dp/B06Y5RZV24/).

It's basically a Rake task that takes the latest 5 posts from the [Deals](https://www.deutsche-startups.de/ressort/deals/) and [Startup Ticker](https://www.deutsche-startups.de/tag/startupticker/) sections of Deutsche-Startups.de, reformats them as JSON for the [Alexa Flash Briefing Skill API](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/understanding-the-flash-briefing-skill-api) and uploads the files to S3.

The Rake task is run every 10 minutes by [Heroku Scheduler](https://devcenter.heroku.com/articles/scheduler) so the latest content is always available.

I built this after reading that [the Alexa Flash Briefing Skill API is available in Germany](https://developer.amazon.com/blogs/post/39ea02b7-1f73-4ead-80ab-a313d9886b82/alexa-flash-briefing-skill-api-now-available-in-the-uk-and-germany) (since March 28, 2017) because I wanted to try it out. And indeed it was super simple to implement the feed, add the skill in the [Amazon Developer Services portal](https://developer.amazon.com/) and get it running on my Amazon Echo.

## Support

If you like this project, consider [buying me a coffee](https://www.buymeacoffee.com/279lcDtbF)! :)
