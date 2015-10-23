#### 1. Add following meta-data under manifest tag of your `AndroidManifest.xml`:
```xml
<meta-data
  android:name="com.appengage.sdk.android.project_number"
  android:value="$YOUR_GCM_PROJECT_NUMBER" />
<meta-data
  android:name="com.appengage.sdk.android.key"
  android:value="YOUR_APPENGAGE_KEY" />
```
> **NOTE:** Make sure to prefix `“YOUR_GCM_PROJECT_NUMBER”` with a `‘$’` sign.


#### 2. Add following `services` under `application` tag of your `AndroidManifest.xml`:
```xml
// required for event processing
<service android:name="com.appengage.sdk.android.ExecutorService" /> 

// required for event logging
<service android:name="com.appengage.sdk.android.EventLogService" />
```


#### 3. Add following `permissions` under manifest tag:
```xml
// required by AppEngage
<uses-permission android:name="android.permission.INTERNET"/>
        
// optional,but highly recommended
<uses-permission android:name="android.permission.WAKE_LOCK" />  
```


#### 4. GCM SETUP:
> Before set-up, make sure to include `Google Play Services` in your project.
(Refer: https://developers.google.com/android/guides/setup for more details)

##### 4.1. If GCM Registration for push messaging should be handled by AppEngage SDK, add following to your manifest:
  1. **meta-data**
  
  ```xml
  <meta-data
    android:name="com.appengage.sdk.android.auto_gcm_registration"
    android:value="true" /> 
  ```
  2. **permissions**
  
  ```xml
  <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
  <permission
    android:name="YOUR.PACKAGE.NAME.permission.C2D_MESSAGE"
    android:protectionLevel="signature" />
  <uses-permission android:name="YOUR.PACKAGE.NAME.permission.C2D_MESSAGE" />
  ```
  3. **receiver**
  
  ```xml
  <receiver android:name="com.appengage.sdk.android.AppEngageReceiver"
    android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
      <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.appengage.sdk.android.intent.ACTION" />
        <category android:name="YOUR.PACKAGE.NAME" />
      </intent-filter>
  </receiver>
  ```
  > **NOTE:** Replace `YOUR.PACKAGE.NAME` with your package name
  
  
##### 4.2. Else, if GCM Registration is already being handled by your app, or you plan to handle it yourself, add following to your manifest:
  1. **meta-data**
  
  ```xml
  <meta-data
    android:name="com.appengage.sdk.android.auto_gcm_registration"
    android:value="false" /> 
  ```

  2. **receiver**
  
  ```xml
  <receiver
    android:name="com.appengage.sdk.android.AppEngageReceiver"
    <intent-filter>
      <action android:name="com.appengage.sdk.android.intent.ACTION" />
      <category android:name="YOUR.PACKAGE.NAME" />
    </intent-filter>
  </receiver>
  ```


#### 5. LOCATION TRACKING SET-UP:
Add following meta-data under manifest tag of your `AndroidManifest.xml`:

```xml
  // If AppEngage should enable location tracking, set it to `true`, otherwise set it to `false`. `Default: TRUE`
  <meta-data
    android:name="com.appengage.sdk.android.location_tracking"
    android:value="true" />
```
> If  above meta-data is set to true then add following permission under manifest tag of your
  `AndroidManifest.xml`:

```xml
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```


#### 6. For AppEngage `Attribution Tracking`, set-up an `INSTALL-REFERRER`:
```xml
<receiver
  android:name="com.appengage.sdk.android.InstallTracker"
  android:exported="true">
  <intent-filter>
    <action android:name="com.android.vending.INSTALL_REFERRER" />
  </intent-filter>
</receiver>
```

