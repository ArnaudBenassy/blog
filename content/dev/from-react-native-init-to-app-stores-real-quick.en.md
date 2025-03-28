---
type:           "post"
title:          "From react-native init to app stores real quick"
date:           "2017-11-09"
publishdate:    "2017-11-09"
draft:          false
slug:           "from-react-native-init-to-app-stores-real-quick"
description:    "From react-native init to stores real quick"
summary:        3

thumbnail:      "/images/posts/thumbnails/from-react-native-init-to-app-stores-real-quick.jpg"
header_img:     "/images/posts/headers/from-react-native-init-to-app-stores-real-quick.jpg"
tags:           ["react", "react native", "mobile"]
categories:     ["dev"]

author_username: "tjarrand"

---

Hi, I'm Thomas and I'm gonna show you how to build and release a React Native app to iOS and Android stores on macOS.

## Our goals

What we aim to accomplish:

* Automated build process that generates signed and __ready-for-stores__ release builds.
* Handling different environments (development, staging, production) with different domain-related variables (e.g. URL of the back-end API).

So let's get to it.

## Prerequisite

###  iOS

You'll need a [Apple ID](https://appleid.apple.com/#!&page=signin) and [Xcode is installed on you mac](https://facebook.github.io/react-native/docs/getting-started.html#xcode).

Also ensure that you agreed to Apple's latest terms and conditions (just launch Xcode and click _Agree_).

### Android

<span class="side-note">🔧</span> Get Android Studio with [tools required by React Native](https://facebook.github.io/react-native/docs/getting-started.html#1-install-android-studio)

<span class="side-note">🔧</span> Launch Android Studio and install [Android SDK and other required libraries](https://facebook.github.io/react-native/docs/getting-started.html#2-install-the-android-sdk) _React Native requires Android 6.0 and SDK 23.0.1_

<span class="side-note">🔧</span> Configure your [Android Home environment variable](https://facebook.github.io/react-native/docs/getting-started.html#3-configure-the-android-home-environment-variable) by adding the following lines in your bash profile file:

{{< highlight bash >}}
export ANDROID_HOME=${HOME}/Library/Android/sdk
export PATH=${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools
{{< /highlight >}}

## Create your app

We won't be using [the default react-native way](https://facebook.github.io/react-native/docs/getting-started.html) that relies on [Expo](https://expo.io) (a container to run your React-Native code in).

While [Expo](https://expo.io) might be fine for experimenting and quickly bootstraping an App, it's not compatible with setting up a _real release process_ as stated in our goals.

So instead, let's go to the [Building Projects with Native Code](https://facebook.github.io/react-native/docs/getting-started.html#the-react-native-cli) section of the documentation:

<span class="side-note">🔧</span> Install the React Native cli tool:

{{< highlight bash >}}
npm install -g react-native-cli
{{< /highlight >}}

<span class="side-note">🔧</span> Create your app and be careful to __choose a name that contains no dots, spaces or other special characters__.

{{< highlight bash >}}
react-native init AcmeApp
cd AcmeApp
{{< /highlight >}}

## Environments and variables

To build a release of the app, we need to give it a few pieces of information:

- A __unique identifier__: `com.acme.app`
- A __version number__: `1.0.0`
- A __build number__: `1`
- Any __domain-releted variables__ we need.

#### Unique identifier

Note the format of the unique identifier: a reverse domain name, starting with `com`, then the vendor name and then the app name.

I strongly recommend that you follow this format and avoid any special character.

This identifier must not be already used on Google Play Store or Apple's App Store.

Once published, an app can't change its identifier, so choose a name that can live through the whole life of the app.
Don't worry though, it's only a technical identifier, it will not be displayed to the public anywhere and you'll have a _Display name_ for that and this one can change anytime you need.

#### Version Number

The version number must follow the semver format `major.minor.patch`.

#### Build Number

The build number is just an integer as simple as `1`.

You'll increment build number _every time you publish a release_.
You can't upload a release that has a build number inferior or equal to any build you ever uploaded for this app.

#### Domain related variables

We might also need some __domain-releated variables__ like the address of the API providing the data for the app.

Such variables will likely have different values in different environments.

For example, we want to call `http://api.app.acme.test` in development mode in the simulator, and `https://api.app.acme.com` in a production release for the stores.

### Enable vendor name in Android

Before anything else, we need to configure Android to support the unique identifier we chose for the app.

The init script generated a unique identifier for Android based on the name you gave it (e.g. `com.acmeapp` for `AcmeApp`).

<span class="side-note light">💡</span> _You can see what name was generated by looking for the `applicationId` key in `android/app/build.gradle`._

If you need to change that unique identifier, do it now as described below:

<span class="side-note">🔧</span> In the `/android` folder, replace all occurrences of `com.acmeapp` by `com.acme.app`

<span class="side-note">🔧</span> Then change the directory structure with the following commands:

{{< highlight bash >}}
mkdir android/app/src/main/java/com/acme
mv android/app/src/main/java/com/acmeapp android/app/src/main/java/com/acme/app
{{< /highlight >}}
<span class="side-note light">💡</span> _You need a folder level for each dot in the app identifier._

### React Native Config

For setting and accessing the variables described above wherever we need it, I recommend using [react-native-config](https://github.com/luggit/react-native-config)

<span class="side-note">🔧</span> Install it with:

{{< highlight bash >}}
yarn add react-native-config
react-native link react-native-config
{{< /highlight >}}

<span class="side-note">🔧</span> Then create an Env file for each environment in the root folder of the project, with the following suggested variables:

__.env.development__

{{< highlight ini >}}
APP_ID=com.acme.app
APP_ENV=development
APP_VERSION=0.1.0
APP_BUILD=1
API_HOST=http://api.app.acme.local
{{< /highlight >}}
__.env.staging__

{{< highlight ini >}}
APP_ID=com.acme.app
APP_ENV=staging
APP_VERSION=0.1.0
APP_BUILD=1
API_HOST=http://api.app.acme.staging
{{< /highlight >}}
__.env.production__

{{< highlight ini >}}
APP_ID=com.acme.app
APP_ENV=production
APP_VERSION=0.1.0
APP_BUILD=1
API_HOST=http://api.app.acme.com
{{< /highlight >}}

## Configure the release

### Generate an Android Signing Key

Android requires that you sign APK before releasing them on the Play Store.
To do so you will need to generate a personal signing key and to configure your Gradle build to use that key.

Edit your local gradle config file `~/.gradle/gradle.properties`:

<span class="side-note">🔧</span> Fill it with the following lines:

{{< highlight ini >}}
MY_RELEASE_STORE_FILE=my_release.keystore
MY_RELEASE_KEY_ALIAS=my_release
MY_RELEASE_STORE_PASSWORD={Generate a 32 characters password}
MY_RELEASE_KEY_PASSWORD={Generate another 32 characters password}
{{< /highlight >}}

<span class="side-note light">💡</span> _You'll need to [generate 2 passwords](https://passwordsgenerator.net/)_

<span class="side-note">🔧</span> Now generate the keystore key file:

{{< highlight bash >}}
keytool -genkey -v -keystore ~/.gradle/my_release.keystore -alias my_release -keyalg RSA -keysize 2048 -validity 10000
{{< /highlight >}}

<span class="side-note">🔧</span> Follow the instructions as below:

{{< highlight bash >}}
Entrez le mot de passe du fichier de clés: {MY_RELEASE_STORE_PASSWORD}
Ressaisissez le nouveau mot de passe: {MY_RELEASE_STORE_PASSWORD}
Quels sont vos nom et prénom ? Jane Doe
Quel est le nom de votre unité organisationnelle ? Lyon
Quel est le nom de votre entreprise ? élao
Quel est le nom de votre ville de résidence ? Lyon
Quel est le nom de votre état ou province ? Rhones-Alpes
Quel est le code pays à deux lettres pour cette unité ? FR
Est-ce CN=Jane Doe, OU=Lyon, O=élao, L=Lyon, ST=Rhones-Alpes, C=FR ? oui
Entrez le mot de passe de la clé pour <my_release> {MY_RELEASE_KEY_PASSWORD}
Ressaisissez le nouveau mot de passe : {MY_RELEASE_KEY_PASSWORD}
{{< /highlight >}}

<span class="side-note">🔧</span> Finally, link the keystore to the project with a symbolic link:

{{< highlight bash >}}
ln -s ~/.gradle/my_release.keystore android/app/my_release.keystore
{{< /highlight >}}

### Configure the gradle build

Edit the `android/app/build.gradle` file and apply the following changes:

<span class="side-note">🔧</span> Insert the following line on _line 2:_

{{< highlight groovy >}}
apply from: project(':react-native-config').projectDir.getPath() + "/dotenv.gradle"
{{< /highlight >}}

<span class="side-note">🔧</span> In the config block, replace the following lines:

{{< highlight diff >}}
defaultConfig {
-    versionCode 1
+    versionCode project.env.get("APP_BUILD") as Integer
-    versionName "1.0"
+    versionName project.env.get("APP_VERSION")
  // ...
}
{{< /highlight >}}

<span class="side-note">🔧</span> Between the `splits` and `buildTypes` blocks (should be line 118) add the following block :

{{< highlight groovy >}}
signingConfigs {
    release {
        storeFile file(MY_RELEASE_STORE_FILE)
        storePassword MY_RELEASE_STORE_PASSWORD
        keyAlias MY_RELEASE_KEY_ALIAS
        keyPassword MY_RELEASE_KEY_PASSWORD
    }
}
{{< /highlight >}}

<span class="side-note">🔧</span> Then add the `signingConfig` property in the `buildTypes > release` section (should be line 131)

{{< highlight groovy >}}
buildTypes {
    release {
        // ...
        signingConfig signingConfigs.release
    }
}
{{< /highlight >}}

### Configure the iOS build

Open your iOS project `iOS/AcmeApp.xcodeproj` with Xcode and select the root item __AcmeApp__ in the file browser on the left.

#### In the _General_ tab

<span class="side-note">🔧</span> In the _Identity_ section input the following values:

- __Bundle Identifier__: Your app unique identifier `com.acme.app` (instead of _org.reactjs.native.example.AcmeApp_)
- __Version__: `__RN_CONFIG_APP_VERSION`
- __Build__: `__RN_CONFIG_APP_BUILD`

<span class="side-note">🔧</span> In the _Signing_ section, select a __Team__

![](/images/posts/2017/from-react-native-init-to-app-stores-real-quick/xcode_config.png)

#### In the _Build Settings_ tab

<span class="side-note">🔧</span> In the _All_ section:

* Search for "preprocess"
* Set __Preprocess Info.plist File__ to `Yes`
* Set __Info.plist Preprocessor Prefix File__ to `${BUILD_DIR}/GeneratedInfoPlistDotEnv.h`
* Set __Info.plist Other Preprocessor Flags__ to `-traditional`

![](/images/posts/2017/from-react-native-init-to-app-stores-real-quick/xcode_build_settings.png)

<span class="side-note light">💡</span> _If you don't see those settings, verify that "All" is selected at the top (instead of "Basic")._

### Change the public app name

To change the name that will be displayed to the public on the stores:

#### On iOS

<span class="side-note">🔧</span> Open your project with XCode and set a __Display Name__ in the _General_ > _Identify_ section.

#### On Android

<span class="side-note">🔧</span> Set the `displayName` property in the the `app.json` file at the root of the project.

## Test our setup

Here's a simple homepage that displays the app version, build number, environment and platform.
You can open your `./App.js` file and replace its content with the following code:

{{< highlight javascript >}}
import React, { Component } from 'react';
import { Platform, Text, View, StyleSheet } from 'react-native';
import Config from 'react-native-config';

export default class App extends Component {
  static styles = StyleSheet.create({
    container: {
      flex: 1,
      justifyContent: 'center',
      alignItems: 'center',
      backgroundColor: '#ffffff',
    },
    infos: {
      textAlign: 'center',
      color: '#333333',
    },
  });

  render() {
    const { styles } = App;
    const { APP_ENV, APP_VERSION, APP_BUILD } = Config;
    const { OS } = Platform;

    return (
      <View style={styles.container}>
        <Text style={styles.infos}>
          AcmeApp v{APP_VERSION} build {APP_BUILD}
        </Text>
        <Text style={styles.infos}>
          [{APP_ENV}] on {OS}
        </Text>
      </View>
    );
  }
}
{{< /highlight >}}

### Quick launch commands

Start the react-native packager manually : `react-native start`

Now a simple `react-native run-ios` should start your app in iOS simulator.

- `react-native run-ios --simulator 'iPhone SE'`
  Run the app in the simulator on a specific device.
- `react-native run-ios --device`
  Run the app on a real iPhone (that must be connected to the mac by USB).
- `react-native run-ios --device --configuration Release`
  Run the app on a real iPhone as a release (fast and no debug, just like it will be in the store).
- `react-native run-android `
  Run the app on any simulated or real android device found.
- `react-native run-android --variant=release `
  Run the app on any simulated or real android device found, as a release.

What's more, we can now run the app in a _certain_ environment by specifying the `ENVFILE` that React Native must use:

| Develoment                               | Staging                                  | Production                               |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| `ENVFILE=.env.development react-native run-ios` | `ENVFILE=.env.staging react-native run-ios ` | `ENVFILE=.env.production react-native run-ios ` |
| ![](/images/posts/2017/from-react-native-init-to-app-stores-real-quick/AcmeApp_dev.png) | ![](/images/posts/2017/from-react-native-init-to-app-stores-real-quick/AcmeApp_staging.png) | ![](/images/posts/2017/from-react-native-init-to-app-stores-real-quick/AcmeApp_production.png) |

## Store configuration

### Generate store icons

Apple's App Store and the Google Play Store both require that you provide icons for your app in various formats.

Fortunately, there are a few online-services that will generate those icons for you from a high-res source icon.

I personally use https://makeappicon.com

- Upload a high-res square icon (ideally `1536x1536 `).
- Enter an email (you don't have to subscribe to the newsletter).
- You will receive the icons by email.

#### Setup icons for iOS

<span class="side-note">🔧</span> Move the content of `iOS` in the following path in your project folder: `iOS/AcmeApp/Images.xcassets/`  (if asked, replace existing content with the new icons).

#### Setup icons for Android

<span class="side-note">🔧</span> Move the content of `android` in the following path in your projet folder: `android/app/src/main/res`  (if asked, replace existing content with the new icons).

### Setting up a launch screen

#### Generation

For splash screen, I use: http://apetools.webprofusion.com/tools/imagegorilla

- Upload a high-res square splash screen (ideally `2048x2048`).
- Press `Kapow`.
- Download the zip file.

<span class="side-note light">🔧</span> Then place the generated launch screen in the same folder as the store icons described above.

#### Configuration

##### iOS

Open your project in XCode:

- In the _General_ tab, go to the _App Icons and Launch Images_ section.
- On the _Launch Images Source_ line, click the _User Asset Catalog_ button, then click _migrate_.
- Once done, click the little grey arrow at the end of the _App Icons Source_ line.

<span class="side-note">🔧</span> Click on the newly created _LaunchImage_ catalog, and fill every required launch screen with the corresponding format from the generated image folder.

<span class="side-note light">💡</span> _If you're having trouble figuring out which format corresponds to which case, the alert tab on the left will tell you exactly what size is expected for each image._

![](/images/posts/2017/from-react-native-init-to-app-stores-real-quick/notice.png)

<span class="side-note">🔧</span> Finally, go back in the _General_ tab, in the _App Icons and Launch Images_ section, do the following:

* As _Launch Images Source_ select __LaunchImage__.
* Empty the __Launch Screen File__.

![](/images/posts/2017/from-react-native-init-to-app-stores-real-quick/launch_screen_setup.png)

<span class="side-note light">💡</span> _You can now delete the `LaunchScreen.xib` in the `AcmeApp` folder._

##### Android

<span class="side-note">🔧</span> Create a `android/app/src/main/res/drawable/splash_screen.xml` file:

{{< highlight xml >}}
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item>
        <bitmap android:gravity="center" android:src="@drawable/screen"/>
    </item>
</layer-list>
{{< /highlight >}}

<span class="side-note">🔧</span> Create a `android/app/src/main/res/values/colors.xml` file:

{{< highlight xml >}}
<resources>
    <color name="primary">#ffffff</color>
</resources>
{{< /highlight >}}

<span class="side-note light">💡</span> _Here `#ffffff` is there to set a white background to the app, feel free to replace it with the color of your choice._

<span class="side-note">🔧</span> Then edit `android/app/src/main/res/values/styles.xml` to add the following node in the _style_ tag:

{{< highlight diff >}}
<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
    <!-- Customize your theme here. -->
+    <item name="android:windowBackground">@drawable/splash_screen</item>
</style>
{{< /highlight >}}

## Build store release

### iOS release

The following command will generate your iOS release:

{{< highlight bash >}}
ENVFILE=.env.production xcodebuild \
    -workspace ./iOS/AcmeApp.xcodeproj/project.xcworkspace \
    -scheme AcmeApp \
    -sdk iphoneos \
    -configuration AppStoreDistribution archive \
    -archivePath ./iOS/release/AcmeApp.xcarchive
{{< /highlight >}}

The archive will be available at `./iOS/release/AcmeApp.xcarchive` and you can open it in Xcode to build an IPA for development purpose or upload it to the App Store.

### Android release

The following command will generate your Android release:

{{< highlight bash >}}
. ./.env.production && cd android && ./gradlew assembleRelease
{{< /highlight >}}

The APK will be available at `./android/app/build/outputs/apk/app-release.apk` and you can [upload it directly to the Play Store](https://play.google.com/apps/publish).

## Troubles?

Here's a working example of all we discussed above : https://github.com/Elao/AcmeApp

<span class="side-note light">💡</span> _Note the handy [Makefile](https://github.com/Elao/AcmeApp/blob/master/Makefile) that hides all complex build and release commands behind simple make tasks like:  `make run android` or `make release-ios`_

Also don't hesitate to reach out for help in the comment or on social medias.
