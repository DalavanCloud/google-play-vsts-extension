# Visual Studio Team Services Extension for Google Play

This extension contains a set of deployment tasks which allow you to automate the release of app updates to the Google Play store from your CI environment. This can reduce the effort needed to keep your dev/alpha/beta/etc. deployments up-to-date, since you can simply push changes to the configured source control branches, and let your automated build take care of the rest.

## Quick Start

1. Go to your [Google Play developer console](https://play.google.com/apps/publish/) and select **Settings**

2. Click on **Api Access** and **Create a Service Account**.

3. Once your service account is created, you will be given a JSON file with your authentication information. We will either extract the needed information from this, or check this file in to VSTS.

4. Install the Google Play extension from the [VSTS Marketplace](https://marketplace.visualstudio.com/items/ms-vsclient.google-play)

5. Go to your Visual Studio Team Services or TFS project, click on the **Build** tab, and create a new build definition (the "+" icon) that is hooked up to your project's appropriate source repo

6. Click **Add build step...** and select the neccessary tasks to generate your release assets (e.g. **Gulp**, **Cordova Build**)

7. Click **Add build step...** and select **Google Play Release** from the **Deploy** category

8. Configure the deploy step with the JSON auth key created in step #1. and the built APK and release track.

9. Click the **Queue Build** button or push a change to your repo in order to run the newly defined build pipeline

## Configuring Your Google Play Publisher Credentials

In addition to specifying your publisher credentials file directly within a build task instance, you can also configure your credentials globally and refer to them within each build or release definition as needed. To do this, perform the following steps:

1. Setup a publishing manager (https://play.google.com/apps/publish/) and get the JSON key file from the [Google Developer API console](https://console.developers.google.com/apis)

2. Go into your Visual Studio Team Services or TFS project and click on the gear icon in the upper right corner

3. Click on the **Services** tab

4. Click on **New Service Endpoint** and select **Google Play**

5. Give the new endpoint a name and enter the credentials for the publishing manager you generated in step#1. The credentials you need can be found in the JSON file and are the Email and the private key.

6. Select this endpoint via the name you chose in #5 whenever you add either the **Google Play Release** or **Google Play Promote** tasks to a build or release definition

## Task Option Reference

In addition to the custom service endpoint, this extension also contributes the following three build and release tasks:

### Google Play Release

The **Google Play Release** task allows you to release an update to your app on Google Play, and includes the following options:

1. **JSON Key Path** (File path) or **Service Endpoint** - The access key to use to authenticate with Google Play. This can be acquired from the [Google Developer API console](https://console.developers.google.com/apis) and provided either directly to the task (via the `JSON Auth File` authentication method), or configured within a service endpoint that you reference from the task (via the `Service Endpoint` authentication method). Note that in order to use the JSON Auth File method, the JSON file you get from the developer console needs to be checked into your VSTS instance.

2. **APK Path** (File path, Required) - Path to the APK file you want to publish to the specified track.

3. **Track** (String, Required) - Publishing track to publish the APK to.

4. **User Fraction** (String, Required if visible) - The percentage of users to roll the specified APK out to. This option is only available when the Track option is set to Rollout.

5. **Release Notes** (File path) - Path to the file specifying the release notes for the APK you are publishing.

### Google Play Promote

The **Google Play Promote** task allows you to move a previously release APK to another track, and includes the following options:

1. **JSON Key Path** (File path) or **Service Endpoint** - The access key to use to authenticate with Google Play. This can be acquired from the [Google Developer API console](https://console.developers.google.com/apis) and provided either directly to the task (via the `JSON Auth File` authentication method), or configured within a service endpoint that you reference from the task (via the `Service Endpoint` authentication method). Note that in order to use the JSON Auth File method, the JSON file you get from the developer console needs to be checked into your VSTS instance.

2. **Package Name** (String, Required) - The unique package identifier (e.g. com.foo.myapp) that you wish to promote.

3. **Source Track** (Required) - The track you wish to promote from.

4. **Destination Track** (Required) - The track you wish to promote to.

5. **User Fraction** (String, Required if visible) - The percentage of users to roll the app out to. This option is only available when the Destination Track option is set to Rollout.

### Google Play Increase Rollout

The **Google Play Increase Rollout** task is a special task that allows you to update the user fraction for an app already on the Rollout track, and includes the following options:

1. **JSON Key Path** (File path) or **Service Endpoint** - The access key to use to authenticate with Google Play. This can be acquired from the [Google Developer API console](https://console.developers.google.com/apis) and provided either directly to the task (via the `JSON Auth File` authentication method), or configured within a service endpoint that you reference from the task (via the `Service Endpoint` authentication method). Note that in order to use the JSON Auth File method, the JSON file you get from the developer console needs to be checked into your VSTS instance.

2. **Package Name** (String, Required) - The unique package identifier (e.g. com.foo.myapp) that you wish to promote.

3. **User Fraction** (String, Required) - The new user fraction to rollout the app to.

##Installation

### Visual Studio Team Services / Visual Studio Online

1. Install the [Visual Studio Team Services Extension for Google Play](https://marketplace.visualstudio.com/items/ms-vsclient.google-play)

2. You will now find the **Google Player Release**, **Google Play Promote**, and **Google Play Increase Rollout** tasks underneath the **Deploy** category

### TFS 2015 Update 1 or Earlier

1. [Enable basic auth](http://go.microsoft.com/fwlink/?LinkID=699518) in your TFS instance

2. Install the tfx-cli and login

	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	npm install -g tfx-cli
	tfx login --authType basic 
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

3. Enter your collection URL (Ex: https://localhost:8080/tfs/DefaultCollection) and user name and password 

4. Download the [latest release](https://github.com/Microsoft/google-play-vsts-extension/releases) of the CodePush tasks locally and unzip it

5. Type the following from the root of the repo from Windows:

	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	upload
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

	Or from a Mac:

	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	sh upload.sh
	~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

## Contact Us
* [Report an issue](https://github.com/Microsoft/google-play-vsts-extension/issues)

Google Play and the Google Play logo are trademarks of Google Inc.