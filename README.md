## Worst Hackathon Ever Demo

Description: Uses Google Cloud Platform to say "Hello World" via the most innefficient manner possible. This is purely for fun, not an official Google product, and should not be used as a best practice for saying "Hello World".

This program uses the following architecture to spin up 11 GCE instances, each with a startup script that tweets a character of "Hello_World", has a 12th GCE instance with a Twitter listener installed listening for specific Tweets with various hashtags, passes those tweets to the Google Cloud NLP API to abstract sentiment, streams the NLP API reponses to BigQuery, and then uses Data Studio and BigQuery to combine the letters in the correct order to display Hello World with some other cool stats.


<img src="Worst Hackathon Ever Architecture.png" width="900"/>

Hats off to Sara Robinson for building the Twitter -> NLP -> BigQuery integration. Check our her GitHub for this code and more here: https://github.com/sararob/ml-talk-demos

## Create and configure 11 GCE instances to tweet at startup
1. Generate [Twitter Streaming API](https://dev.twitter.com/streaming/overview) 

2. Follow this guide https://www.ostechnix.com/tweet-commandline-linux/ to configure your GCE Linux host to tweet for CLI.

      a. sudo apt-get install ruby-dev

      b. sudo apt-get install build-essential

      c. sudo gem install t

      d. sudo gem install twurl (ran into some issues with authorizing just t, so installed twurl)

      e. sudo twurl authorize --consumer-key <consumer key> --consumer-secret <consumer secret>

      f. sudo cp /root/.twurlrc /root/.trc

      g. sudo twurl accounts

      e. sudo t accounts

      f. sudo t update "Hello World #hackathon" (boom, tweet should have been sent! The reason you have to use sudo is because the GCP Startup scripts run as root)
  
3. For each GCE instace, configure a startup script from the Google Cloud Console and store the script in GCS

      i.e. example: sudo t update "E. Having an awesome time! Love hacking! Boom! Best thing ever! #worsthackathonever" (in this example the E will be the second character in "Hello_World" and you'll use the NLP API to generate a sentiment score based off the "Having an awesome time! Love hacking! Boom! Best thing ever! #worsthackathonever" and order the tweets according to positive sentiment.
  
  Here are all the scripts:
  
    sudo t update "H. Having so much fun! Love programming! AMAZING! Best thing ever! #worsthackathonever"
    sudo t update "E. Having an awesome time! Love hacking! Boom! Best thing ever! #worsthackathonever"
    sudo t update "L. Loving the Google Cloud APIS!!! Talk about the best thing ever! #worsthackathonever"
    sudo t update "L. SUPER EXCITED TO SAY Hello World!!! #worsthackathonever"
    sudo t update "O. Love saying Hello World! #worsthackathonever"
    sudo t update "_. Not too much of a fan of people who don't comment in their code. #worsthackathonever"
    sudo t update "W. My competitors are going down for the worst hackathon ever! Boom! #worsthackathonever"
    sudo t update "O. Not using the cloud is boring. #worsthackathonever"
    sudo t update "R. Going head to head in the worst hackathon! #worsthackathonever"
    sudo t update "L. I hate poorly written programs. Yuck! #worsthackathonever"
    sudo t update "D. Hacking for the worst hackathon ever! Talk about an inefficient way of saying hello world. Horrible! #worsthackathonever"


## Natural Language API BigQuery demo

1. `cd` into `natural-language/`
2. Generate [Twitter Streaming API](https://dev.twitter.com/streaming/overview) credentials and copy them to `local.json`
3. Create a Google Cloud project, generate a JSON keyfile, and add the filepath to `local.json`
4. Create a BigQuery dataset and table with the below schema, add them to `local.json`
<img src="table-schema.png" width="300"/>

5. Generate an API key and add it to `local.json`
6. Change line 37 to filter tweets on whichver terms you'd like
7. Install node modules: `npm install`
8. Run the script: `node twitter.js`


