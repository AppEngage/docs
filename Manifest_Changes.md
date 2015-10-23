#### 1. Add following meta-data under manifest tag of your AndroidManifest.xml:
```xml
<meta-data
  android:name="com.appengage.sdk.android.project_number"
  android:value="$YOUR_GCM_PROJECT_NUMBER" />
<meta-data
  android:name="com.appengage.sdk.android.key"
  android:value="YOUR_APPENGAGE_KEY" />
```
> **NOTE:** Make sure to prefix “YOUR_GCM_PROJECT_NUMBER” with a ‘$’ sign.

#### 2. Add following services under application tag of your AndroidManifest.xml:
```xml
// required for event processing
<service android:name="com.appengage.sdk.android.analytics.ExecutorService" /> 

// required for event logging
<service android:name="com.appengage.sdk.android.analytics.EventLogService" />
```

#### 3. Add following permissions under manifest tag:
```xml
// required by AppEngage
<uses-permission android:name="android.permission.INTERNET"/>
        
// optional,but highly recommended
<uses-permission android:name="android.permission.WAKE_LOCK" />  
```

#### 4. GCM SETUP:
> Before set-up, make sure to include Google Play Services in your project.
(Refer: https://developers.google.com/android/guides/setup for more details)
1. If GCM Registration for push messaging should be handled by AppEngage SDK, add following to your manifest:
  1. **meta-data**

#### 5. LOCATION TRACKING SET-UP:
#### 6. For AppEngage Attribution tracking, set-up an INSTALL-REFERRER:

