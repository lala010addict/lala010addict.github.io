---
layout: post
section-type: post
title: Build a Simple Custom Slack Bot using @slack/client
category: TUTORIALS
tags: [ 'tutorials' ]
---

Recently, we officially switched from Hipchat to Slack at work. I wanted to make a simple slack bot that can help me to choose whom to pick on for PR reviews. I made a randomizer that takes in a list of names and outputs two names.

Set up a bot user
	1. Go to[https://my.slack.com/services/new/bot][https://my.slack.com/services/new/bot]
	2. Generate an API token.

Set up the node server
	1. Make a new folder and cd into the folder
	2. `npm init`
	3. Install slack/client by doing `npm install @slack/client --save`
	4. Create a new file named app.js

Make the slack bot
<pre><code>
var RtmClient = require('@slack/client').RtmClient;

var _ = require('lodash')

var token = 'YOUR SLACK BOT API KEY' || '';

var rtm = new RtmClient(token, { logLevel: 'debug' });
rtm.start();


var CLIENT_EVENTS = require('@slack/client').CLIENT_EVENTS;

rtm.on(CLIENT_EVENTS.RTM.AUTHENTICATED, function(rtmStartData) {
  console.log(`Logged in as ${rtmStartData.self.name} of team ${rtmStartData.team.name}, but not yet connected to a channel`);
});


var RTM_EVENTS = require('@slack/client').RTM_EVENTS;

// Listens to all `message` events from the team
rtm.on(RTM_EVENTS.MESSAGE, function(message) {
  console.log(message, 'message')



  var str = message.text;
  // <@U22QZJNKA> is the my bot id. I am checking to see if someone @ my bot
  var n = str.includes("<@U22QZJNKA>")

  var array = str.split(', ');

  var splicedArray = array.splice(1)

  var lucky = _.sampleSize(splicedArray, 2)
  var msg = ':laser_cat: meow meow :thinking_face: ??? ' + lucky[0] + ' & ' + lucky[1] + ' today is your lucky day RUFFF RUFFFF :doge:!! :partyparrot: :beers::dealwithitparrot::hypnotoad: '

  var params = {
    text: msg,
    channel_id: 'G20TSFYAX',

  }
  if (n) {
  	// This will send the message to the channel identified by id 'G20TSFYAX'
    rtm.sendMessage(params.text, params.channel_id, function messageSent() {
    	// optionally, you can supply a callback to execute once the message has been sent
    });
  }

});
</code></pre>

Run the bot
	`nodemon app.js` (if you have nodemon installed) or `node app.js`
	The bot will work as long as you have it running it locally or deployed somewhere.

Results
	taaadaaaa
	![](/blogimgs/slackbot/slackbot.png)


