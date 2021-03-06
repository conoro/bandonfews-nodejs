# Bandon FEWS Small Data Scraper in Node.js

UPDATE JULY 2019: The Bandon FEWS system is being decommissioned so this code will no longer work. The OPW and Cork CoCo believe the extensive flood prevention measures they have implemented have rendered the service unnecessary. Based on the mess that is the Fish Run and Fish Kill Pond (aka the new weir), I have my doubts.

## Introduction
Cork County Council has a site called [Bandon FEWS](http://www.bandonfloodwarning.ie/) (Bandon Flood Early Warning System). When the Bandon river hits certain levels near Bandon town, it alerts registered users via SMS in case they need to take emergency measures. It's a very useful service. However the historical river level data is not available in any useful form and that's the point of this project.

In November 2011, I created a simple Python script which scrapes the site every 15 minutes and saves the river level to a Google Fusions Table "spreadsheet" [here](https://www.google.com/fusiontables/DataSource?docid=103YIcARoxuaWT7NfZ8mVBzY554sF_3ONYC1N3DE#rows:id=1). This now has (with a few interruptions) a lot of data which anyone can query, re-use or slice-dice and mashup with weather info. Not that anyone has done this :-)

I re-wrote it in Node.js in 2014.

To use it for something else on Fusion Tables, the main thing you need to do is setup an App in the [Google API Console](https://console.developers.google.com/project?authuser=0), enable Fusion Tables API access and use those keys in the placeholders in the config files.

## Files
* main.js - The main code
* bandonfews.conf - Supervisor config file. Copy to /etc/supervisor/conf.d/bandonfews.conf then `sudo supervisorctl reread` and `sudo supervisorctl reread`
* tokens-example.json - rename to tokens.json. Will be auto-generated by the code on first run in any case
* config-example.json - Rename to config.json and fill out the Google OAuth API key/secret, the URL of the FEWS site and the id of the Fusion Table
* index.html - Placeholder for upcoming landing page
* server.js - Placeholder for upcoming API server
* package.json - Required Node.js packages. `npm install` to install everything
* .gitignore - Stuff that shouldn't go under Git control

## TO-DO
* The first time run is currently broken for the OAuth credentials.
* Token refresh is not handled yet but has never been required.
* It should have a simple Express based API so others can avoid the pain of dealing with the Fusion Tables API for basic queries
* It should have a simple landing page showing the latest water level and maybe a graph

## Changelog
* 04/01/2015 - First public usable version and [blogpost](http://conoroneill.net/bandon-flood-warning-data-now-scraped-to-google-fusion-tables-using-nodejs)
* 05/01/2015 - Add error handling for parsing issues in case FEWS site changes and breaks the scraping
