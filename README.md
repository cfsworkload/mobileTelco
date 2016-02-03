# Data Plan Manager

IBM Ready App for Telecommunications demonstrates a new genre of mobile service provider where plans are controlled by the end user and not limited to a few choices. These dynamic service providers are starting to emerge all around the world. The app empowers the customer to control their mobile voice, text, and data plan while empowering the service provider to provide the right offers at the right time. Combining this with MobileFirst Platform Foundation for IBM Containers allows providers to maintain the backend solution with Bluemix services.

#### Prerequisite Software
- **Android Studio** - To install the app on an Android phone or run it in an Android emulator, install Android Studio from http://developer.android.com/sdk/index.html.
- **Java 1.7+** - Java is required to create MobileFirst server files. The version of Java on your machine must also be compatible with the version installed on the IBM container for the MobileFirst server. More about this in a later section. 
- **Docker and IBM Containers Extension(ice)** - Docker and ice are required to create your MobileFirst server container as defined by a Dockerfile. See the [installation instructions](https://www.ng.bluemix.net/docs/containers/container_cli_ice_ov.html) in Option 2, to install Docker and ice for your operating system.
- **[Eclipse](https://eclipse.org/downloads/) Luna v4.4.2 or higher** - Eclipse is needed to run a java application to upload hotspot data to the Cloudant Geo database. When installing Eclipse, select Eclipse IDE for Java EE Developers.

#### Other Notes
- The IBM MobileFirst Platform Foundation on IBM Containers does not currently run a Windows OS but a virtual machine with Linux can be used to run the scripts.

## Sign up and log into Bluemix and DevOps
Sign up for Bluemix at https://console.ng.bluemix.net and DevOps Services at https://hub.jazz.net. When you sign up, you'll create an IBM ID, create an alias, and register with Bluemix.


## Get a public IP in your Bluemix space
This solution requires a free public IP address. In order to determine if a public IP is available, you need to find the number of used and your max quota of IP addresses allowed for your space.

To find this information:

1. Log into Bluemix at https://console.ng.bluemix.net.
2. Select **DASHBOARD**.
3. Select the space you where you would like your Telecommunications app to run.
4. In the Containers tile, information about your IP addresses is listed.
5. Check that the **Public IPs Requested** field has at least one available IP address.

If you have an IP address available, you can request a new IP or use an existing available IP to start building your Telecommunications app. If all of your IP addresses have been used, you will need to release one. In either case, to manage your public IP addresses, install the CF IC plugin, which can be found at the website below.

https://www.ng.bluemix.net/docs/containers/container_cli_ov.html#container_cli_cfic_installs

Once installed:

1. Log into your Bluemix account and space.

  `cf ic login`  
2. List your current external IP addresses.

  `cf ic ip list`
3. If the list of external IP addresses contains an unused address, you can use that one for this solution.
4. If you are not at your limit of IP addresses and want a new one, request an IP address.

  `cf ic ip request`
5. If you need to make an IP address available, release an IP address currently in use.

  `cf ic ip release <public IP>`  

## Download IBM MobileFirst Platform Foundation for IBM Containers
The IBM MobileFirst Platform Foundation for IBM Containers zip archive contains source code and scripts needed to build and deploy an MFPF server on IBM containers. It is first built locally, and then pushed to IBM Containers on Bluemix. Using this project allows any MobileFirst runtime environment to be included in the IBM container. The zip file includes separate installation packages for the MobileFirst Operations Console and the MobileFirst Analytics Console. For the purposes of this solution, you only need the Operations Console to get the mobile app up and running.

For more information about the IBM MobileFirst Foundation Platfom on IBM Containers, see [https://developer.ibm.com/mobilefirstplatform/documentation/getting-started-7-1/bluemix/run-foundation-on-bluemix](https://developer.ibm.com/mobilefirstplatform/documentation/getting-started-7-1/bluemix/run-foundation-on-bluemix).

1. [Review license and download zip file.](http://www14.software.ibm.com/cgi-bin/weblap/lap.pl?popup=Y&li_formnum=L-BVID-9XEQG7&accepted_url=http://public.dhe.ibm.com/ibmdl/export/pub/software/products/en/MobileFirstPlatform/mfpfcontainers/ibm-mfpf-container-7.1.0.0-eval.zip)
2. Once you have agreed to the license and downloaded the zip, extract the contents of the zip.

### Updating Dockerfiles for Java version
In the ibm-mfpf-container-7.1.0.0-eval/mfpf-server and ibm-mfpf-container-7.1.0.0-eval/mfpf-analytics folders, there are Dockerfiles that are used to create the containers. These Dockerfiles refer to a version of Java that is included in the dependencies folder. Since you build .war, .adapter, and .wlapp files on your local machine, to avoid Java version incompatibilities, you may need to match Java versions for your local machine with the version used to create the container.

One option is to update the Dockerfiles to install a different version of Java. 
For example, to install Oracle Java 8 you can comment out these lines in the Dockerfile:
	
	ADD dependencies/ibm-java-jre-7.1-3.10-linux-x86_64.tgz /opt/ibm/    
	ENV JAVA_HOME /opt/ibm/ibm-java-x86_64-71
	
And add in:

	RUN \
	  echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
	  add-apt-repository -y ppa:webupd8team/java && \
	  apt-get update && \
	  apt-get install -y oracle-java8-installer && \
	  rm -rf /var/lib/apt/lists/* && \
	  rm -rf /var/cache/oracle-jdk8-installer

	ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

## Clone IBM ReadyApp for Telecommunications GitHub repository
This repository contains the updated solution for the IBM ReadyApp for Telecommunications to work with Bluemix containers as well as the source code for the app itself. Clone this with the following Git command.
	
  `git clone https://github.com/cfsworkload/mobileTelco.git`


## Set up Cloudant GEO database
A Cloudant Geospatial database is required to query for wifi hotspots around a given user. Cloudant Geospatial allows a user to query the cloudant database using the standard GeoJSON formatted documents. For more information on developing queries and setting up the database visit [CloudantGeo](https://cloudant.com/product/cloudant-features/geospatial/). Cloudant on Bluemix does not currently support geospatial indexing and query. Until then, a Cloudant database must be created on Cloudant.com.

### Create Cloudant Geospatial database
Create a Cloudant account and database to be accessed and populated with hotspot data.

1. Go to Cloudant.com to create an account.
2. Click **Create Database**, enter a name for the database and click **Create** to create your geospatial capable database.

### Install IBM MobileFirst Platform Studio 7.1.0 Eclipse Plugin
IBM MobileFirst Platform Studio is an Eclipse plug-in that helps you quickly build, run, and manage mobile web, hybrid, and native apps.

1. Select **Help** and then click **Eclipse Marketplace** to open the Eclipse Marketplace window.
2. In the Find field, enter "MobileFirst" and click **Go** to search for the MobileFirst package.
3. Find IBM MobleFirst Platform Studio 7.1.0 and click the **Install** button.
4. Click **Confirm** to proceed with the default selected items.
5. Select **I accept the terms of the license agreements** and click **Finish**.
6. Restart Eclipse to complete the installation of IBM MobileFirst Platform Studio 7.1.0.
 
### Import the project into Eclipse
In Eclipse, import the TelcoReadyAppMFP folder into the Eclipse workspace to access CloudantService.java for the next section.

1. First, go back to your Eclipse IDE and change your view to the Package Explorer. This can be accessed by going to **Window** > **Show View** > **Other** > **General** > **Project Explorer**. This will open up the Project Explorer in the left panel of your Eclipse workspace. 
2. Next, right-click on the Package Explorer and choose **Import**. 
3. In the resulting dialog, find and expand the General folder and select **Existing Projects into Workspace**. 
4. Now, select **Next** and navigate to location where you have saved the Telco source code. 
5. Finally, choose the TelcoReadyAppMFP folder in the resulting dialog and click **Finish** to complete the import.

### Populate database with data
1. Go to server/java/resources/app.properties and change the values to reflect your account and password and database name.
2. Navigate to the CloudantService.java class in Eclipse under server/java/com/ibm/mil/cloudant and right click the class and click **Run as Java Application**. This will run main() method in CloudantService which uploads hotspots.json under server/java/resources/db/ to the Cloudant database specified in app.properties

The database is now set up to query using the CloudantGeoAdapter under the MobileFirst Platform.

## Install MobileFirst Platform CLI
The IBM MobileFirstFirst Platform Command Line Interface tool is used to easily create, manage, push, and register MobileFirst client and server artifacts. Specifically, you will use the CLI to create the server artifacts in the next section.

1. Download the CLI package for the required release version:
   [MobileFirst Platform Foundation CLI v7.1 (20151214)](http://public.dhe.ibm.com/ibmdl/export/pub/software/products/en/MobileFirstPlatform/mobilefirst_cli_installer_7.1.0.zip)
2. Unzip the downloaded file. The CLI is packaged as a single compressed file, which contains installation executable files for each platform:
        install_linux.bin
        install_mac.app
        install_windows.exe
3. Select and run the installer that is appropriate for your platform. A GUI appears and guides you through the installation of Command Line Interface. Follow the instructions to complete your installation.
4. On completion of the installation, log out from the OS, and then log back in. This action ensures that the appropriate commands are on your system path.

When the MobileFirst CLI is installed, you should be able to run mfp commands if the MobileFirst CLI directory is in the PATH environment variable. i.e. PATH=$PATH:/Applications/IBM/MobileFirst-CLI

#### Create a local mfpf server
To run some mfp commands a local server must first exist. If you do not already have a local server, follow these steps.

1. Run the following command to add a server:

	`mfp server add`
2. Select the local server option to create the local server.
3. Check you list of servers to make sure the server was created.

	`mfp server info`
4. Start your local mfp server so additional mfp commands, such as mfp push, can be run.

	`mfp start`

## Create the MobileFirst server files
To have a MobileFirst project run on a MobileFirst server, a runtime environment must be included on the server. This is done by adding a .war file for the MobileFirst project to the server.  Files created in this section are created in the TelcoReadyAppMFP/bin folder.

### Create Adapters
Adapters are server-side Java or Javascript code used to transfer and retrieve information from back-end systems to client applications and cloud services. This solution uses three adapters.

-  **TelcoUserAdapter**- Located in TelcoReadyAppMFP/adapters/TelcoUserAdapter, the TelcoUserAdapter controls all of the data that a user would request including offers, profile, etc. Some of these methods are protected by OAuth Security and can only be accessed by authenticating with the AuthenticationAdapter.
-  **CloudantGeoAdapter**- Located in TelcoReadyAppMFP/adapters/CloudantGeoAdapter, this adapter contains a method to query wifi hotspots around a given user's location.
-  **AuthenticationAdapter**- Located in TelcoReadyAppMFP/adapters/AuthenticationAdapter, this adapter is a javascript adapter with basic authentication for a user. This AuthenticationAdapter protects the realm challenged during the OAuth handshake.

To easily create these files, in Eclipse, for each folder in TelcoReadyAppMFP/adapters, right click on the folder and select **Run As** > **Deploy Mobile First Adapter**. This will create the adapter files in the TelcoReadyAppMFP/bin folder.

### Create .war and .wlapp files
To create these files, in your bash terminal, go to your MobileFirst project folder. (i.e. IBM-Ready-App-for-Telecommunications/TelcoReadyAppMFP) From that directory run the MobileFirst CLI command:
 
  `mfp push`
  
If you had a MobileFirst server running locally, it would create the necessary files(.war, .wlapp) and push them to the server. An error may appear if you do not have a local mfp server running, but the files are still created. All files must be compiled with the same or lower version of Java than what the server was built.

## Update Telco Android app properties files 

Enter your own keys in these files to set up access to services for the app:

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
5. Click **Show App Key** to get the MQA key and add it to the secrets.xml file.

analyticsKey: This is the Google analytics key.

1. Log in to Google analytics at http://www.google.com/analytics/ and create an application.
2. Find the Tracking ID by clicking **Admin** then **Property Settings**.
3. Replace the placeholder text in the secrets.xml by this Tracking ID.


**wlclient.properties**: This properties file contains information about the app ID and version as well as the MobileFirst server host and port. For the purposes of this solution, only the wlServerHost property needs to be updated.

- wlServerHost: The IP address of your MobileFirst server container. Use the available IP address that you acquired earlier.
- wlServerPort: The port of the MobileFirst server. The default value is 9080 as it is defined in the MobileFirst server configuration. For purposes of this walkthrough, use the default value.
- wlAppId: Your application id. This can be found in the application-descriptor.xml file located in the following directory: /TelcoReadyAppMFP/apps/TelcoReadyAppAndroid/ (Ex: myApp)
- wlAppVersion: Your application version. This can also be found in the application-descriptor.xml file located in the following directory: /TelcoReadyAppMFP/apps/TelcoReadyAppAndroid/ (i.e. 1.0)


## Update MobileFirst server configuration files

In the ibm-mfpf-container-7.1.0.0-eval/mfpf-server/scripts/args folder are a set of configuration files which contain the properties that are required to run the scripts without user interactive input.

### Prerequisite setup
Before you run the update the properties files and scripts, there are a few steps to make sure these scripts can run properly.

1. Make sure you are logged into IBM Container Cloud Service.

	`cf ic login`
2. Make sure that the namespace for container registry is set. The namespace is a unique name to identify your private repository on the Bluemix registry. This will be used in the properties files. The namespace is assigned once for an organization and cannot be changed.
Choose a namespace according to following rules:

	- It can contain only lowercase letters, numbers, or underscores (_).
	- It can be 4 – 30 characters. If you plan to manage containers from the command line, you might prefer to have a short namespace that can be typed quickly.
	- It must be unique in the Bluemix registry.

	To set a namespace, run the command:

	`cf ic namespace set <new_name>`

	To get the namespace that you have set, run the command:

	`cf ic namespace get`
	
### Properties Files
Fill in the property values in the following files:

**initenv.properties** - This file defines the properties needed for the initenv.sh script which initializes the Bluemix environment.

- BLUEMIX_API_URL - Bluemix API endpoint. The default is https://api.ng.bluemix.net. Uncomment this line if changes need to be made to this value.
- BLUEMIX_REGISTRY - The IBM Containers registry domain. The default is registry.ng.bluemix.net. Uncomment this line if changes need to be made to this value.
- BLUEMIX_CCS_HOST - The IBM Container Cloud Service Host. The default is https://containers-api.ng.bluemix.net/v3/containers. Uncomment this line if changes need to be made to this value.
- BLUEMIX_USER - Your Bluemix username (email).
- BLUEMIX_PASSWORD - Your Bluemix password.
- BLUEMIX_ORG - Your Bluemix organization name.
- BLUEMIX_SPACE - Your Bluemix space. The default space you create when creating a Bluemix account is usually **dev**.

**prepareserverdbs.properties** - This defines the properties needed to run the prepareserverdbs.sh which configures the management and runtime databases for the MobileFirst Platform projects.

- DB_TYPE - The Bluemix Database service type. Set this to **cloudantNoSQLDB** since this set up requires a Cloudant database.
- DB_SRV_NAME - Your Bluemix DB service instance name.
- DB_SRV_PLAN - This is the Bluemix database service plan type. Set this to **Shared** since this set up uses a Cloudant database.
- APP_NAME - Your Bluemix DB application name. Note: Choose a unique name.
- RUNTIME_NAME - The MobileFirst project runtime name. Required for configuring runtime databases only, as explained in the prepareserverdbs.sh step. The first use of this file by the prepareserverdbs.sh script requires the property to be commented out to the admin database. After that, it is uncommented out for the runtime database(s). The name of the runtime should match the name of the .war file created by the MobileFirst CLI. e.g. TelcoReadyAppMFP. 

**prepareserver.properties** - This defines the properties needed to run the perpareserver.sh script which creates the MobileFirst Platform Foundation server image and pushes it to the IBM Bluemix container registry.

- SERVER_IMAGE_TAG - A tag for the image. Should be of the form: registry-url/namespace/your-tag. Same as in startserver.properties. e.g. registry.ng.bluemix.net/mynamespace/mfpserver71:latest
- PROJECT_LOC - A path to the root directory of your MobileFirst project. Multiple project locations can be delimited by a comma. For this solution, uncomment out this property and enter the full directory location of IBM-Ready-App-for-Telecommunications/TelcoReadyAppMFP.

**startserver.properties** - This defines the properties to run the IBM MobileFirst Platform Foundation server image on an IBM Bluemix container service.

- SERVER_IMAGE_TAG - A tag for the image. Should be of the form: registry-url/namespace/your-tag. Same as in prepareserver.properties. e.g. registry.ng.bluemix.net/mynamespace/mfpserver71:latest
- SERVER_CONTAINER_NAME - A name for your Bluemix Container. Give this property a unique name.
- SERVER_IP - An IP address that the Bluemix Container should be bound to. Use the IP address that you acquired earlier.

## Run the scripts to build and deploy
The scripts found in the mfpf-server/scripts directory use the properties set in the previous section to build and deploy the MobileFirst server with the Telecommunications runtime environment. These scripts must run in the bash shell or they may not run as expected.

### installcontainercli.sh – Adding Container Extension to the MobileFirst CLI
In order to use the Container Extension you must first add it to the MobileFirst CLI. Before running this script, make sure the Docker daemon is running, JAVA_HOME attribute is set, and the MobileFirst CLI path is set.
	
Run:

  `sudo ./installcontainercli.sh`

Successful completion of this script should result in a 'Done' message.

### initenv.sh – Logging in to Bluemix
Run the initenv.sh script in order to create an environment for building and running IBM MobileFirst Platform Foundation on the IBM Containers:

  `./initenv.sh args/initenv.properties`

### prepareserverdbs.sh – Prepare the MobileFirst Server database
The prepareserverdbs.sh script is used to configure your MobileFirst project database. You will need to run it separately, once for the admin database and once for every MobileFirst project runtime database. Successful completion of this script should result in a "Successfully Completed Cloudant NoSQL DB Service Binding" message.

1. For the admin database make sure to comment out the RUNTIME_NAME argument in the prepareserverdbs.properties file and run:

  `./prepareserverdbs.sh args/prepareserverdbs.properties`
    
2. For each MobileFirst project runtime database – first uncomment the project RUNTIME_NAME argument, change it value to match the specific project war file and run:
	
  `./prepareserverdbs.sh args/prepareserverdbs.properties`

Note: If you get the error "Application not configured correctly", try to run the script again with the same properties.

### prepareserver.sh – Prepare a Mobilefirst Platform Foundation Server image
Uncomment the PROJECT_LOC argument and run the prepareserver.sh script in order to build a MobileFirst Platform Foundation Server image, deploy your project runtime and push it to to your Bluemix repository:

  `./prepareserver.sh args/prepareserver.properties`

To view all available images in your Bluemix repository run:

  `cf ic images`

The list contains the image name, date of creation and ID.

### startserver.sh – Running the image on an IBM Container
The startserver.sh script is used to run the Mobilefirst Server image on an IBM Container. It also binds your image to the public IP you configured in the SERVER_IP property.

  `./startserver.sh args/startserver.properties`

Once the container has started, follow the steps below to add the app and adapters.
   
1. Launch the MobileFirst Console by loading the following URL: http://<server_ip>:9080/worklightconsole. This may take a few minutes. The default login/password combination is admin/admin.
2. Upload the .wlapp and .adapter files by clicking **Add new app or adapter** and selecting the files from your TelcoReadyAppMFP/bin directory.
3. Update the application’s worklight.plist (for iOS) and/or wlclient.properties (for Android, Windows Universal, Windows Phone) with the protocol, host and port values of the IBM Container.
4. You can now run your application to verify that it successfully connects to the MobileFirst Server, running on IBM Containers.


## Install the App on Android Devices

Compile and Run the android project on an Android emulator or Android device to see the app in action.
1. Start Android Studio.
2. Click the Quick Start option, **Open an existing Android Studio project**.
3. Navigate to the cloned repository folder, **IBM-Ready-App-for-Telecommunications**, and select the **TelcoReadyAppAndroid** folder. Then click **Choose**.
4. Once the new project is done loading into Android Studio, click the **Run** at the top to compile and run the app on an Android device connected to your computer. 

Note: If you are new to Android development, you may need to go into your device settings and enable USB debugging for your computer to recognize the device. Also, when using Android Studio, if you use the emulator instead of an Android device, no other Virtual Machines can be running, including the Docker daemon.

Once you have the app running on your device, you can see the different possibilities the app has for managing data plans and using geospatial data to provide user-specific services. You can customize the TelcoUserAdapterResource.java file for your specific app requirements to handle user data changes. The adapter would then need to be recreated and pushed to the MobileFirst runtime environment.
