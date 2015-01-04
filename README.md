# Bandon FEWS Small Data Scraper in Node.js
## Introduction
Cork County Council have a site called [Bandon FEWS](http://www.bandonfloodwarning.ie/) (Bandon Flood Early Warning System). When the Bandon river hits certain levels near Bandon town, it alerts registered users via SMS in case they need to take emergency measures. It's a very useful service. However the historical river level data is not available in any useful form and that's the point of this project.

In November 2011, I created a simple Python script which scrapes the site every 15 minutes and saves the river level to a Google Fusions Table "spreadsheet" [here](https://www.google.com/fusiontables/DataSource?docid=103YIcARoxuaWT7NfZ8mVBzY554sF_3ONYC1N3DE#rows:id=1). This now has (with a few interruptions) 3 years of data which anyone can query, re-use or slice-dice and mashup with weather info. Not that anyone has done this :-)

The script has been running on a small home Ubuntu fileserver all this time but stopped working mid-December 2014 either due to changes with the page structure of the Council site or as an unexpected byproduct of upgrading to Ubuntu 14.04. I have therefore re-written it in Node.js which has the big added advantage that there are free hosting possibilities for this code such as Red Hat's [OpenShift](https://www.openshift.com/).

The code started as Google's example G+ OAuth code but now has the bulk of the functionality completed. It is running reliably under Supervisor on an [ODROID-C1](http://www.hardkernel.com/main/products/prdt_info.php?g_code=G141578608433) single-board computer on my desk.

To use it for something else on Fusion Tables, the main thing you need to do is setup an App in the [Google API Console](https://console.developers.google.com/project?authuser=0), enable Fusion Tables API access and use those keys in the placeholders in the confile files.

## Files
* main.js - The main code
* bandonfews.conf - Supervisor config file. Copy to /etc/supervisor/conf.d/bandonfews.conf then `sudo supervisorctl reread` and `sudo supervisorctl reread`
* tokens-example.json - rename to tokens.json. Will be auto-generated by the code on first run in any case
* config-example.json - Rename to config.json and fill out the Google OAuth API key/secret, the URL of the FEWS site and the id of the Fusion Table
* index.html - Placeholder for upcoming landing page
* server.js - Placeholder for upcoming API server
* package.json - Required Node.js packages. `npm install` to install everything
* .gitignore - Stuff that shouldn't go under Git control


## OpenShift Environment Variables
If you are running on OpenShift, you need a way to set various keys etc without adding them to files in Git which could potentially leak accidentally if you also put a copy up on GitHub. So set the following (mirroring the equivalent ones in config.json and tokens.json):

```rhc set-env access_token="blah" token_type="Bearer" refresh_token="blah" expiry_date=1419970265050 client_id="blah" client_secret="blah" redirect_url="http://localhost" fews_url="http://www.bandonfloodwarning.ie/main.php" fusiontables_id="blah" -a bandonfews ```


## TO-DO
* The first time run is currently broken for the OAuth credentials. Easy fix coming.
* OpenShift is SIGINT'ing it for some unknown reason after a random time which is why it's running on the ODROID for the moment
* It should have a simple Express based API so others can avoid the pain of dealing with the Fusion Tables API for basic queries
* It should have a simple landing page showing the latest water level and maybe a graph
* It should add back in the functionality of the old Python code for posting to COSM
* ???

## Release Notes
* 04/01/2015 - First public usable version
