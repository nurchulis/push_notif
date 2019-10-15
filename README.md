# push_notif
---------------------------
Ini adalah sebuah aplikasi react native untuk menampilkan 
push Notfikiasi dengan react-native-firebase
Adapun yang harus disiapkan adalah:
- react-native-firebase": "^5.2.1
- react-native": "0.57.5


##Download Dulu file google-service.json
di https://firebase.google.com/?hl=id
Kemudian paste di android/app/


###Setting Android/src/main/AndroidManifest.xml

```html
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.push_notif">

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>

    <application
      android:name=".MainApplication"
      android:label="@string/app_name"
      android:icon="@mipmap/ic_launcher"
      android:allowBackup="false"
      android:theme="@style/AppTheme">
      <activity
        android:name=".MainActivity"
        android:label="@string/app_name"
        android:configChanges="keyboard|keyboardHidden|orientation|screenSize"
        android:windowSoftInputMode="adjustResize">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
      </activity>
      <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />
   //TAMBAH KAN INI   
      <service android:name="io.invertase.firebase.messaging.RNFirebaseMessagingService">
        <intent-filter>
          <action android:name="com.google.firebase.MESSAGING_EVENT" />
        </intent-filter>
      </service>
      <service android:name="io.invertase.firebase.messaging.RNFirebaseInstanceIdService">
        <intent-filter>
          <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
        </intent-filter>
      </service>
   //SAMPAI SINI
    </application>

</manifest>
```


###Setting Android/src/main/java/com/push_notif/MainApplication.java

```javascript
package com.push_notif;

import android.app.Application;

import com.facebook.react.ReactApplication;
import io.invertase.firebase.RNFirebasePackage; //Tambah kan ini
import io.invertase.firebase.messaging.RNFirebaseMessagingPackage;    //Tambah kan ini
import io.invertase.firebase.notifications.RNFirebaseNotificationsPackage;    //Tambah kan ini
import com.facebook.react.ReactNativeHost;
import com.facebook.react.ReactPackage;
import com.facebook.react.shell.MainReactPackage;
import com.facebook.soloader.SoLoader;

import java.util.Arrays;
import java.util.List;

public class MainApplication extends Application implements ReactApplication {

  private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
    @Override
    public boolean getUseDeveloperSupport() {
      return BuildConfig.DEBUG;
    }

    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
          new MainReactPackage(),
            new RNFirebasePackage(), //Tambah kan ini
              new RNFirebaseMessagingPackage(), //Tambah kan ini
                new RNFirebaseNotificationsPackage() //Tambah kan inis
      );
    }

    @Override
    protected String getJSMainModuleName() {
      return "index";
    }
  };

  @Override
  public ReactNativeHost getReactNativeHost() {
    return mReactNativeHost;
  }

  @Override
  public void onCreate() {
    super.onCreate();
    SoLoader.init(this, /* native exopackage */ false);
  }
}


```


###Setting Android/app/build.gradle
```javascript
dependencies {
    compile project(':react-native-firebase')
    implementation fileTree(dir: "libs", include: ["*.jar"])
    implementation "com.android.support:appcompat-v7:${rootProject.ext.supportLibVersion}"
    implementation "com.facebook.react:react-native:+"  // From node_modules

    implementation "com.google.android.gms:play-services-base:16.0.1"
    implementation "com.google.firebase:firebase-core:16.0.6"
    implementation "com.google.firebase:firebase-messaging:17.3.4"
}

// Run this once to be able to run the application with BUCK
// puts all compile dependencies into folder libs for BUCK to use
task copyDownloadableDepsToLibs(type: Copy) {
    from configurations.compile
    into 'libs'
}


apply plugin: 'com.google.gms.google-services'
```


###Setting Android/build.gradle
```javascript
buildscript {
    ext {
        buildToolsVersion = "27.0.3"
        minSdkVersion = 16
        compileSdkVersion = 27
        targetSdkVersion = 26
        supportLibVersion = "27.1.1"
    }
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.1.4'
        classpath 'com.google.gms:google-services:4.0.1' //Tambahkan ini
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        mavenLocal()
        google()
        jcenter()
        maven {
            // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
            url "$rootDir/../node_modules/react-native/android"
        }
    }
}


task wrapper(type: Wrapper) {
    gradleVersion = '4.4'
    distributionUrl = distributionUrl.replace("bin", "all")
}
```


###Setting Android/settings.gradle
```
rootProject.name = 'push_notif'
include ':react-native-firebase'
project(':react-native-firebase').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-firebase/android')

include ':app'
```
