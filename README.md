# Mobile Telecommunications

The IBM Ready App for Telecommunications mobile app is an Android app which accesses a Cloudant GEO database for stored location points using a MobileFirst server and installed adapters. The app also includes additional functionality for user interaction with Google Maps, Google Analytics, and Twitter.

IBM Ready App for Telecommunications demonstrates a new genre of mobile service provider where plans are controlled by the end user and not limited to a few choices. These dynamic service providers are starting to emerge all around the world. The app empowers the customer to control their mobile voice, text, and data plan while empowering the service provider to provide the right offers at the right time.


### Prerequisite Software
- Android Studio - To install the app on an Android phone or run it in an Android emulator, install Android Studio from http://developer.android.com/sdk/index.html.
- Oracle Java 1.8 - To avoid version issues when creating adapters, the MobileFirst container is built with Oracle Java 8 and must be a higher version of Java that is used to build the adapters. Other versions of Java such as OpenJDK may also cause version issues.
- Docker
- Python
- Eclipse Luna v4.4.2 or higher - Eclipse is needed to run a java application to upload hotspot data to the Cloudant Geo database.


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

## Download solution files
The files required for this solution are here on this GitHub repository and a 

### Clone IBM ReadyApp for Telecommunications GitHub repository
This repository contains the updated solution for the IBM ReadyApp for Telecommunications to work with Bluemix containers. Clone this with the following command.
	
	git clone https://github.com/cfsworkload/mobileTelco.git

### Download IBM MobileFirst Platform Foundation server for containers
The 

## Set up Google Analytics API

## Set up Google Maps API

## Set up Twitter Credentials

## Set up Cloudant GEO database
A Cloudant Geospatial database is required to query for wifi hotspots around a given user. Cloudant Geospatial allows a user to query the cloudant database using the standard GeoJSON formatted documents.For more information on developing queries and setting up the database visit [CloudantGeo](https://cloudant.com/product/cloudant-features/geospatial/). Cloudant on Bluemix does not currently support geospatial indexing and query. Until then, a Cloudant database must be created on Cloudant.com.

1. Go to Cloudant.com to create an account and create a database.
2. Go to server/java/resources/app.properties and change the values to reflect your account and password and database name.


## Install MobileFirst Platform Foundation CLI
The IBM MobileFirstFirst Platform Command Line Interface...

1. Download the CLI package for the required release version:
   [MobileFirst Platform Foundation CLI v7.1 (20151214)](http://public.dhe.ibm.com/ibmdl/export/pub/software/products/en/MobileFirstPlatform/mobilefirst_cli_installer_7.1.0.zip)
2. Unzip the downloaded file. The CLI is packaged as a single compressed file, which contains installation executable files for each platform:
        install_linux.bin
        install_mac.app
        install_windows.exe
3. Select and run the installer that is appropriate for your platform. A GUI appears and guides you through the installation of Command Line Interface. Follow the instructions to complete your installation.
4. On completion of the installation, log out from the OS, and then log back in. This action ensures that the appropriate commands are on your system path.

## Install IBM MobileFirst Platform Studio
IBM MobileFirst Platform Studio is an Eclipse plug-in that helps you quickly build, run, and

## Set up Telco properties files

Enter your own keys in these files:
File Name	Location
secrets.xml	/TelcoReadyAppAndroid/app/source/main/res/values/
app.properties	/TelcoReadyAppMFP/server/java/resources/
wlclient.properties	/TelcoReadyAppAndroid/app/source/main/assets/

secrets.xml:

googeMapsApiKey: Go here to get a google maps api key, click on Get A Key and follow the instructions from google. Once you have the key, replace the placeholder text inside the secrets.xml file with the actual api key.

mqaKey:

    Log in to the bluemix console.
    Create a Mobile Quality Assurance(MQA) service.
    Inside the MQA service dashboard, click New MQA App to create a new app.
    Click Add Platform to the right of the application and choose a platform.
    Click Show App Key to get the MQA key and add it to the secrets.xml file

encodedTwitterKey:

    Log in to the twitter fabric console.
    Create a twitter application in the console or use an existing one.
    Get the application key for your application form the twitter fabric console.
    Encode the twitter key.
    Paste the encoded version of the application key in the secrets.xml file.

encodedTwitterSecret:

    Get the application secret for your application form the twitter fabric console.
    Encode the application secret.
    Paste the encoded version of the application secret in the secrets.xml file.

analyticsKey:

    Log in to google analytics and create an application.
    Find the Tracking ID under Admin > Property Settings
    Replace the placeholder text in the secrets.xml by this Tracking ID.

wlclient.properties:

wlServerHost: The host name of your worklight server. (ex: localhost)

wlServerPort: The port of the worklight server. Default will be used if this value is left blank. (Ex: 8080)

wlAppId: Your application id. This can be found in the application-descriptor.xml file located in the following directory: /TelcoReadyAppMFP/apps/TelcoReadyAppAndroid/ (Ex: myApp)

wlAppVersion: Your application version. This can also be found in the application-descriptor.xml file located in the following directory: /TelcoReadyAppMFP/apps/TelcoReadyAppAndroid/ (Ex: 1.0)

## Deploy to IBM MobileFirst Platform Server Container

1. Review the license and download the [ibm-mfpf-container-7.1.0.0-eval.zip](http://www14.software.ibm.com/cgi-bin/weblap/lap.pl?popup=Y&li_formnum=L-BVID-9XEQG7&accepted_url=http://public.dhe.ibm.com/ibmdl/export/pub/software/products/en/MobileFirstPlatform/mfpfcontainers/ibm-mfpf-container-7.1.0.0-eval.zip).
2. 

### Update MobileFirst server configuration files

In the args folder are a set of configuration files which contain the arguments that are required to run the scripts. Fill in the arguments’ values in the following files:

initenv.properties - This file defines the properties needed for the initenv.sh script which initializes the Bluemix environment.

    BLUEMIX_API_URL – Bluemix API endpoint. The default is https://api.ng.bluemix.net.
    BLUEMIX_REGISTRY – The IBM Containers registry domain. The default is registry.ng.bluemix.net.
    BLUEMIX_CCS_HOST – The IBM Container Cloud Service Host. The default is https://containers-api.ng.bluemix.net/v3/containers.
    BLUEMIX_USER – Your Bluemix username (email).
    BLUEMIX_PASSWORD – Your Bluemix password.
    BLUEMIX_ORG – Your Bluemix organization name.
    BLUEMIX_SPACE – Your Bluemix space (as explained previously).

prepareserverdbs.properties - This defines the properties needed to run the prepareserverdbs.sh which configures the management and runtime databases for the MobileFirst Platform projects.

    DB_SRV_NAME – Your Bluemix DB service instance name.
    APP_NAME – Your Bluemix DB application name. Note: Choose a unique name.
    RUNTIME_NAME – The MobileFirst project runtime name. Required for configuring runtime databases only, as explained in the next step. The name of the runtime should match the name of the .war file created by the MobileFirst CLI. e.g. TelcoReadyAppMFP

prepareserver.properties - This defines the properties needed to run the perpareserver.sh script which creates the MobileFirst Platform Foundation server image and pushes it to the IBM Bluemix container registry.

    SERVER_IMAGE_TAG – A tag for the image. Should be of the form: registry-url/namespace/your-tag. Same as in startserver.properties.
    PROJECT_LOC – A path to the root directory of your MobileFirst project. Multiple project locations can be delimited by a comma. For this solution, enter the full directory location of IBM-Ready-App-for-Telecommunications/TelcoReadyAppMFP.

startserver.properties - This defines the properties to run the IBM MobileFirst Platform Foundation server image on an IBM Bluemix container service.

    SERVER_IMAGE_TAG – A tag for the image. Should be of the form: registry-url/namespace/your-tag. Same as in prepareserver.properties.
    SERVER_CONTAINER_NAME – A name for your Bluemix Container.
    SERVER_IP – An IP address that the Bluemix Container should be bound to. Use the IP address that you acquired earlier.

### Run the 

## Install App on Android Devices

Compile and Run the android project
1. Start Android Studio.
2. Click the Quick Start option, “Open an existing Android Studio project”.
3. Navigate to the cloned repository folder, IBM-Ready-App-for-Telecommunications, and select the TelcoReadyAppAndroid folder. Then click the Choose button.
4. Once the new project is done loading into Android Studio, you can click the Run button at the top to compile and run the app on an Android device connected to your computer. Note: If you are new to Android development, you may need to go into your device settings and enable USB debugging for your computer to recognize the device.

