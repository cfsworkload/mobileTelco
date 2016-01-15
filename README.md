# Mobile Telecommunications

The IBM Ready App for Telecommunications mobile app is an Android app which accesses a Cloudant GEO database for stored location points using a MobileFirst server and installed adapters. The app also includes additional functionality for user interaction with Google Maps, Google Analytics, and Twitter.

IBM Ready App for Telecommunications demonstrates a new genre of mobile service provider where plans are controlled by the end user and not limited to a few choices. These dynamic service providers are starting to emerge all around the world. The app empowers the customer to control their mobile voice, text, and data plan while empowering the service provider to provide the right offers at the right time.
((HELP: The second sentence is straight from the Ready App people))

#### Prerequisite Software
- **Android Studio** - To install the app on an Android phone or run it in an Android emulator, install Android Studio from http://developer.android.com/sdk/index.html.
- **Oracle Java 1.8** - To avoid versioning issues when creating adapters, the MobileFirst container is built with Oracle Java 8 and must be a higher version of Java that is used to build the adapters. Other versions of Java such as OpenJDK may also cause version incompatibility issues.
- **Docker version 1.6 or higher** - Docker is required to create your MobileFirst server container as defined by a Dockerfile. See the [installation instructions](https://docs.docker.com/machine/install-machine/) to install Docker for your operating system.
- **Eclipse Luna v4.4.2 or higher** - Eclipse is needed to run a java application to upload hotspot data to the Cloudant Geo database.


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
The files required for this solution are here on this GitHub repository and a zip file of the IBM MobileFirst Platform Foundation for containers

### Download IBM MobileFirst Platform Foundation server for containers
The IBM MobileFirst Platform Foundation project contains source code and scripts needed to build and deploy an MFPF server on IBM containers. Using this project allows any MobileFirst runtime environment to be included in the IBM container. The zip file includes separate installation packages for the MobileFirst Operations Console and the MobileFirst Analytics Console. For the purposes of this solution, you only need the Operations Console to get the mobile app up and running.

[Download](https://ibm.box.com/shared/static/1x4getedme1ulgm2fnf8g4y7ghdqfipv.zip) the zip file from IBM Box and unzip it.

### Clone IBM ReadyApp for Telecommunications GitHub repository
This repository contains the updated solution for the IBM ReadyApp for Telecommunications to work with Bluemix containers as well as the source code for the app itself. Clone this with the following git command.
	
  `git clone https://github.com/cfsworkload/mobileTelco.git`


## Set up Cloudant GEO database
A Cloudant Geospatial database is required to query for wifi hotspots around a given user. Cloudant Geospatial allows a user to query the cloudant database using the standard GeoJSON formatted documents. For more information on developing queries and setting up the database visit [CloudantGeo](https://cloudant.com/product/cloudant-features/geospatial/). Cloudant on Bluemix does not currently support geospatial indexing and query. Until then, a Cloudant database must be created on Cloudant.com.

### Create Cloudant Geospatial database
Create a Cloudant account and database to be accessed and populated with hotspot data.

1. Go to Cloudant.com to create an account.
2. Click **Create Database**, enter a name for the database and click **Create** to create your geospatial capable database.

### Import project into Eclipse
In Eclipse, import the TelcoReadyAppMFP folder into the Eclipse workspace to access CloudantService.java for the next section.

1. First, go back to your Eclipse IDE and change your view to the Package Explorer. This can be accessed via Window > Show View > Other > Project Explorer. This will open up the Project Explorer in the left panel of your Eclipse workspace. 
2. Next, right-click on the Package Explorer and choose **Import**. 
3. In the resulting dialog, find and expand the General folder and select Existing Projects into Workspace. 
4. Now, select **Next** and navigate to location where you have saved the Telco source code. 
5. Finally, choose the TelcoReadyAppMFP folder in the resulting dialog and choose **Finish** to complete the import.

### Populate database with data
1. Go to server/java/resources/app.properties and change the values to reflect your account and password and database name.
2. Navigate to the CloudantService.java class in Eclipse under server/java/com/ibm/mil/cloudant and right click the class and click **Run as Java Application**. This will run main() method in CloudantService which uploads hotspots.json under server/java/resources/db/ to the Cloudant database specified in app.properties

The database is now set up to query using the CloudantGeoAdapter under the MobileFirst Platform.

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


## Set Telco Android app properties file 

Enter your own keys in these files:

	File Name				 Location
	secrets.xml			   /TelcoReadyAppAndroid/app/src/main/res/values/
	wlclient.properties	   /TelcoReadyAppAndroid/app/src/main/assets/

**secrets.xml:**

googeMapsApiKey: This API key is used to allow the app to be location-aware integrate Google maps.
1. Go to https://developers.google.com/maps/android/ to get a Google maps api key.
2. Click on **Get A Key** and follow the instructions from Google. 
3. Once you have the key, replace the placeholder text inside the secrets.xml file with the actual api key.

mqaKey: (Optional) This is the key for the Mobile Quality Assurance Bluemix service. As it is a paid service, it has been commented out of the this properties file as well as the MainActivity.java file.

1. [Log in](http://console.ng.bluemix.net/) to the Bluemix console.
2. Create a Mobile Quality Assurance(MQA) service.
3. Inside the MQA service dashboard, click New MQA App to create a new app.
4. Click **Add Platform** to the right of the application and choose a platform.
5. Click **Show App Key** to get the MQA key and add it to the secrets.xml file

encodedTwitterKey: This encoded key and the next encoded value enables the app to send Twitter messages.

1. [Log in](https://get.fabric.io/) to the twitter fabric console.
2. Create a Twitter application in the console or use an existing one.
3. Get the application key for your application form the Twitter fabric console.
4. Encode the Twitter key using a [base 64 encoder](https://www.base64encode.org/).
5. Paste the encoded version of the application key in the secrets.xml file.

encodedTwitterSecret: This encoded value is also required to enable the app to send Twitter messages.

1. Get the application secret for your application form the twitter fabric console.
2. Encode the application secret using a [base 64 encoder](https://www.base64encode.org/).
3. Paste the encoded version of the application secret in the secrets.xml file.

analyticsKey: This is the Google analytics key.

1. Log in to Google analytics at http://www.google.com/analytics/ and create an application.
2. Find the Tracking ID under Admin > Property Settings
3. Replace the placeholder text in the secrets.xml by this Tracking ID.


**wlclient.properties:**

- wlServerHost: The IP address of your MobileFirst server container.

- wlServerPort: The port of the MobileFirst server. The default value is 9080 as it is defined in the MobileFirst server configuration.

- wlAppId: Your application id. This can be found in the application-descriptor.xml file located in the following directory: /TelcoReadyAppMFP/apps/TelcoReadyAppAndroid/ (Ex: myApp)

- wlAppVersion: Your application version. This can also be found in the application-descriptor.xml file located in the following directory: /TelcoReadyAppMFP/apps/TelcoReadyAppAndroid/ (Ex: 1.0)

## Configure the IBM MobileFirst Platform Server Container

1. Review the license and download the [ibm-mfpf-container-7.1.0.0-eval.zip](http://www14.software.ibm.com/cgi-bin/weblap/lap.pl?popup=Y&li_formnum=L-BVID-9XEQG7&accepted_url=http://public.dhe.ibm.com/ibmdl/export/pub/software/products/en/MobileFirstPlatform/mfpfcontainers/ibm-mfpf-container-7.1.0.0-eval.zip).
2. 

### Update MobileFirst server configuration files

In the args folder are a set of configuration files which contain the properties that are required to run the scripts. Fill in the arguments’ values in the following files:

**initenv.properties** - This file defines the properties needed for the initenv.sh script which initializes the Bluemix environment.

- BLUEMIX_API_URL – Bluemix API endpoint. The default is https://api.ng.bluemix.net.
- BLUEMIX_REGISTRY – The IBM Containers registry domain. The default is registry.ng.bluemix.net.
- BLUEMIX_CCS_HOST – The IBM Container Cloud Service Host. The default is https://containers-api.ng.bluemix.net/v3/containers.
- BLUEMIX_USER – Your Bluemix username (email).
- BLUEMIX_PASSWORD – Your Bluemix password.
- BLUEMIX_ORG – Your Bluemix organization name.
- BLUEMIX_SPACE – Your Bluemix space (as explained previously).

**prepareserverdbs.properties** - This defines the properties needed to run the prepareserverdbs.sh which configures the management and runtime databases for the MobileFirst Platform projects.

- DB_SRV_NAME – Your Bluemix DB service instance name.
- APP_NAME – Your Bluemix DB application name. Note: Choose a unique name.
- RUNTIME_NAME – The MobileFirst project runtime name. Required for configuring runtime databases only, as explained in the next step. The name of the runtime should match the name of the .war file created by the MobileFirst CLI. e.g. TelcoReadyAppMFP

**prepareserver.properties** - This defines the properties needed to run the perpareserver.sh script which creates the MobileFirst Platform Foundation server image and pushes it to the IBM Bluemix container registry.

- SERVER_IMAGE_TAG – A tag for the image. Should be of the form: registry-url/namespace/your-tag. Same as in startserver.properties.
- PROJECT_LOC – A path to the root directory of your MobileFirst project. Multiple project locations can be delimited by a comma. For this solution, enter the full directory location of IBM-Ready-App-for-Telecommunications/TelcoReadyAppMFP.

**startserver.properties** - This defines the properties to run the IBM MobileFirst Platform Foundation server image on an IBM Bluemix container service.

- SERVER_IMAGE_TAG – A tag for the image. Should be of the form: registry-url/namespace/your-tag. Same as in prepareserver.properties.
- SERVER_CONTAINER_NAME – A name for your Bluemix Container.
- SERVER_IP – An IP address that the Bluemix Container should be bound to. Use the IP address that you acquired earlier.

## Run the scripts to build and deploy
The scripts found in the mfpf-server/scripts directory use the properties set in the previous section to build and deploy 
### installcontainercli.sh – Adding Container Extension to the MobileFirst CLI
In order to use the Container Extension you must first add it to the MobileFirst CLI.
Run:

  `sudo ./installcontainercli.sh`

### initenv.sh – Logging in to Bluemix
Run the initenv.sh script in order to create an environment for building and running IBM MobileFirst Platform Foundation on the IBM Containers:

  `./initenv.sh args/initenv.properties`

### prepareserverdbs.sh – Prepare the MobileFirst Server database
The prepareserverdbs.sh script is used to configure your MobileFirst project database. You will need to run it separately, once for the admin database and once for every MobileFirst project runtime database.

1. For the admin database make sure to comment out the RUNTIME_NAME argument in the prepareserverdbs.properties file and run:

  `./prepareserverdbs.sh args/prepareserverdbs.properties`
    
2. For each MobileFirst project runtime database – first uncomment the project RUNTIME_NAME argument, change it value to match the specific project war file and run:
	
  `./prepareserverdbs.sh args/prepareserverdbs.properties`

Note: If you are getting an error: “Application not configured correctly” – try to run the script (with the same properties) again.

### prepareserver.sh – Prepare a Mobilefirst Platform Foundation Server image
Uncomment the PROJECT_LOC argument and run the prepareserver.sh script in order to build a MobileFirst Platform Foundation Server image, deploy your project runtime and push it to to your Bluemix repository:

  `./prepareserver.sh args/prepareserver.properties`

To view all available images in your Bluemix repository run:

  `cf ic images`

The list contains the image name, date of creation and ID.

### startserver.sh – Running the image on an IBM Container
The startserver.sh script is used to run the Mobilefirst Server image on an IBM Container. It also Binds your image to the public IP you configured in the SERVER_IP property.

  `./startserver.sh args/startserver.properties`

Once the container has     
1. Launch the MobileFirst Console by loading the following URL: http://<server_ip>:9080/worklightconsole (it may take a few moments).
2. Upload the .wlapp and .adapter files.
3. Update the application’s worklight.plist (for iOS) and/or wlclient.properties (for Android, Windows Universal, Windows Phone) with the protocol, host and port values of the IBM Container.
4. You can now run your application to verify that it successfully connects to the MobileFirst Server, running on IBM Containers.




./initenv.sh args/initenv.properties  						(log into blue mix and the right orgs)
		Make sure docker daemon is running
		Make sure JAVA_HOME is set  export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home"
		make sure MF CLI path is set   export PATH=$PATH:/Applications/IBM/MobileFirst-CLI
3. Run the prepareserverdbs.sh script first with the RUNTIME_NAME variable in the prepareserverdbs.properties file commented out. This will install 
	./prepareserverdbs.sh args/prepareserverdbs.properties       (Once without RUNTIME_NAME)
			??? Update app.properties and build war file with ‘mfp push’
			Get 
./prepareserverdbs.sh args/prepareserverdbs.properties      ( and once each for the RUNTIME_NAME)
./prepareserver.sh args/prepareserver.properties			(run the docker file)
6. Run the image on IBM Container by running the starterver.sh script. This script rus the Mobilefirst Server image on IBM Container and binds the container to the public IP set in the SERVER_IP property.
	./startserver.sh args/startserver.properties


## Install App on Android Devices

Compile and Run the android project
1. Start Android Studio.
2. Click the Quick Start option, “Open an existing Android Studio project”.
3. Navigate to the cloned repository folder, IBM-Ready-App-for-Telecommunications, and select the TelcoReadyAppAndroid folder. Then click the Choose button.
4. Once the new project is done loading into Android Studio, you can click the Run button at the top to compile and run the app on an Android device connected to your computer. Note: If you are new to Android development, you may need to go into your device settings and enable USB debugging for your computer to recognize the device.

