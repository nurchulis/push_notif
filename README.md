# push_notif
---------------------------
Ini adalah sebuah aplikasi react native untuk menampilkan 
push Notfikiasi dengan react-native-firebase
Adapun yang harus disiapkan adalah:
- react-native-firebase": "^5.2.1
- react-native": "0.57.5



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
