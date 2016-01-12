# Mobile Telecommunications

The IBM Ready App for Telecommunications mobile app is an Android app which accesses a Cloudant GEO database for stored location points using a MobileFirst server and installed adapters. The app also includes additional functionality for user interaction with Google Maps, Google Analytics, and Twitter.

IBM Ready App for Telecommunications demonstrates a new genre of mobile service provider where plans are controlled by the end user and not limited to a few choices. These dynamic service providers are starting to emerge all around the world. The app empowers the customer to control their mobile voice, text, and data plan while empowering the service provider to provide the right offers at the right time.


### Recommended Software
- Android Studio - To install the app on an Android phone or run it in an Android emulator, install Android Studio from http://developer.android.com/sdk/index.html.
- OpenJDK Java 1.7 - To avoid version issues when creating ada
- Docker



## Sign up and log into Bluemix and DevOps
Sign up for Bluemix at https://console.ng.bluemix.net and DevOps Services at https://hub.jazz.net. When you sign up, you'll create an IBM ID, create an alias, and register with Bluemix.

## Make sure a public IP is available in your Bluemix space
This solution requires a free public IP. In order to determine if a public IP is available, you need to find the number of used and your max quota of IP addresses allowed for your space.

To find this information:

1. Log into Bluemix at https://console.ng.bluemix.net.
2. Select **DASHBOARD**.
3. Select the space you where you would like your Telecommunications app to run.
4. In the Containers tile, information about your IP addresses is listed.
5. Check that the **Public IPs Requested** field has at least one available IP address.

If you have an IP address available, you're ready to start building your Wish List app. If all of your IP addresses have been used, you will need to release one. In order to release a public IP, install the CF IC plugin, which can be found at the website below.

https://www.ng.bluemix.net/docs/containers/container_cli_ov.html#container_cli_cfic_installs

Once installed:

1. Log into your Bluemix account and space.

  `cf ic login`  
2. List your current external IP addresses.

  `cf ic ip list`
3. Release an IP address.

  `cf ic ip release <public IP>`

## Set up Google Analytics API

## Set up Google Maps API

## Set up Twitter Credentials

## Set Cloudant GEO server
A Cloudant Geospatial database is required to handle Geospatial queries...

## Install MobileFirst CLI locally


