# What_do_cars_sell_for
By scraping Craigslist and following up on the URLs, it is possible to determine the like price at which cars sell/don’t sell.

The system behavior is going to look something like this:

- Check craigslist for new listings; save all of them with a unique ID (URL)
- Check all saved listings to see if they've changed status (options are [removed by user, expired, flagged])
- repeat

After that process has run for a while, I'll have a dataset that I can use to compare a listing's time on the platform,end status, and any details about the listings

That information can probably be used to learn stuff about cars; or maybe even train a neural network to predict what price/timeframe a given listing would sell at/in

I need two main pieces of tech infrastructure to build this:
- a database that can store HTML files
- a webscraper that can access craigslist

In the past I've spent a ton of time and energy building data cleaning functions into the webscraper; taking pieces of information like model, price, etc out of the HTML before writing it to the database.

This practice made the pipeline brittle, the frequent breaks limited the size/usefulness of a dataset

Craigslist's own T&C cause listings to expire after 45 days no matter what, so I can label a URL as 'retired' after that timeframe.

I also can just compare the saved HTML to the scraped HTML to see if it's changed and save a new version if it has.  No need to determine conclusivity out of the gate.

|-webscraper-| -> |-database-|
|-program on ec2-| -> |-dynamoDB-|

I need to run the scraper on ec2 because Craigslist uses client-side rendering, so I need to host a web browser client to get all the information out of HTML

dynamoDB is a key-value store that'll be adequate for saving HTML pages with a unique key

In the future I can write an HTML parser to analyze this data; the mistake I've made in the past is writing a parser inline with the pipeline.  
- Note: don't do this.

-  
