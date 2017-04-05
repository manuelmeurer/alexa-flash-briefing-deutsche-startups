# Alexa Flash Briefing Deutsche Startups

This is a Rake task that takes the latest 5 posts from the ["Deals" section of Deutsche-Startups.de](https://www.deutsche-startups.de/ressort/deals/), reformats them as JSON for the [Alexa Flash Briefing Skill API](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/understanding-the-flash-briefing-skill-api) and uploads the file to S3.

The Rake task can be run regularly (e.g. using the [Heroku Scheduler](https://devcenter.heroku.com/articles/scheduler)) so the latest content is always available.

I built this quickly after reading that [the Alexa Flash Briefing Skill API is available in Germany](https://developer.amazon.com/blogs/post/39ea02b7-1f73-4ead-80ab-a313d9886b82/alexa-flash-briefing-skill-api-now-available-in-the-uk-and-germany) (since March 28, 2017) and I wanted to try it out. And indeed it was super-simple to set this up, add the Skill and get it running on my Amazon Echo.

I am currently talking to the guys at Deutsche-Startups.de to see if I can publish the Skill so everybody can use it.
