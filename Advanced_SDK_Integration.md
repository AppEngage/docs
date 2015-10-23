### **Event Tracking APIs**

After AppEngage has been successfully initialized you can track an event by using following apis:
```java
AppEngage.get().analytics().track(String eventName)

AppEngage.get().analytics().track(String eventName, Map<String,? extends Object> attributes)
```
<br />

### **User APIs**
In addition to events, AppEngage allows you to attach attributes to logged in as well as
anonymous users. Attributes are set of properties that make up a user profile. This profile information can be used to segment users which allows for better targeting based on different user personas.

#### **User Identification**

Use following API to identify a user to AppEngage:

**When a user Logs-In (`Identifying User`)**
```java
AppEngage.get().user().loggedIn(String identifer)
```

> **IMP:** Please make sure to call above api as soon as a user logs into your application, or whenever earliest you are able to identify him/her.

> Once called, every session, user attribute and event will be attributes to this user.

> All attributes, events and session information accumulated before this API has been called is attributes to an anonymous user. Once this anonymous user is identified, all of this stored information is attributed to the latest identified user, using above API.

**When a user Logs-Out (`Forgetting User`)**

When a user logs out of your application then
```java
AppEngage.get().user().loggedOut(String identifer)
```

#### **Setting Custom User Attributes**
> Use these API's to attach custom attributes to the user

```java
AppEngage.get().user().setAttribute(String attributeName,String value)

AppEngage.get().user().setAttribute(String attributeName,Boolean value)

AppEngage.get().user().setAttribute(String attributeName,Long value)

AppEngage.get().user().setAttribute(String attributeName,Double value)

AppEngage.get().user().setAttribute(String attributeName,Integer value)

AppEngage.get().user().setAttribute(String attributeName,List<? extends Object> values)
```

> **IMP:** Following java data types are allowed for above API's: `String`, `Integer`, `Long`, `Double`, `Boolean`. The `List` itself can contain one of these data types.

```java
AppEngage.get().user().setAttribute(Map<String,? extends Object> attributes)
```
> The values in above `Map` should only be one of the above defined data types, or a `List`. If the value is of `List` type, it should not contain any other data type other than defined above.

<br />
#### **Setting Pre-Defined User Attributes**
```java
AppEngage.get().user().setEmail(String email)

AppEngage.get().user().setBirthDate(Integer year,Integer month,Integer day)	// Month is index 1 based: January = 1, ...

AppEngage.get().user().setPhoneNumber(String phoneNumber)

AppEngage.get().user().setGender(String gender)	// Can be `male`, `female` or `other`

AppEngage.get().user().setFirstName(String firstName)

AppEngage.get().user().setLastName(String lastName)

AppEngage.get().user().setCompany(String company)

AppEngage.get().user().setUserProfile(UserProfile userProfile) // Use this to set user profile in one go using `Java Builder Pattern`
```

#### **Deleting Custom User Attributes**
```java
AppEngage.get().user().deleteAttribute(String attributeKey)

AppEngage.get().user().deleteAttributes(List<String> attributeKeys)
```

#### **Deleting System User Attributes**
```java
AppEngage.get().user().deleteAttribute(UserAttributeKey attributeKey)
```
<br />
### **Other APIs**
#### **AppEngage Config**
> Returns AppEngage configuration. See [AppEngageConfig](#appengageconfig) for details
```java
AppEngageConfig AEConfig = AppEngage.get().getAppEngageConfig()
```

<br />
#### **Logging**
Set Log Level for AppEngage (Applicable only in development mode). See [Logger](#logger) for details
```java
AppEngage.get().setLogLevel(Logger.DEBUG)
```


<br />
#### **Event Reporting Strategy**
AppEngage stores every event in local database and sends them periodically to server in a background thread when a certain criteria
is met. The criteria is based on the number of events in local database and last synced time. 
> **Note:** We have made sure that the size of local database does
grow beyond certain limit in the case of extended connectivity lost

AppEngage allows you to change the event reporting behaviour during runtime also, so that you can make events sync faster to AppEngage if available conectivity is good.
```java
AppEngage.get().setEventReportingStrategy(ReportingStrategy.FORCE_SYNC)
```
> By default the reporting strategy is `ReportingStrategy.BUFFER`. See [ReportingStrategy](#reportingstrategy) for details

<br />
#### **Lower Level GCM APIs**
Receiving push notification is a straightforward way if GCM Registration and Message Receiving is left to AppEngage. 

However, if your application is already configured to handle GCM, or you intend to do it yourself, AppEngage allows you to do that quite easily too. 

You can use our lower level apis to achieve this and keep receiving AppEngage push notifications along with your existing stuff. Here's how you do it 

**1. Passing GCM Registration Id to AppEngage**
After successful registration with GCM you should pass your `GCM registration id` and `GCM project number`  to AppEngage using:
```java
AppEngage.get().setRegistrationID(String registrationID, String projectNumber)
```
**2. Passing GCM Messages to AppEngage**
All incoming messages from AppEngage will contain a key `“source”` with the value as `“appengage”` inside the _Android_ `bundle` object (_both without double-quotes_).

You **must** pass these messages to AppEngage using following api to keep receiving push notifications from AppEngage:
```java
AppEngage.get().onReceive(Intent intent)
```
> The Intent in the above api is the same intent as received in the GCM broadcast Receiver.

<br />
### **AppEngage  Callbacks**
> Implement one or both of these to listen to AppEngage lifecycle (GCM and App upgrade/install) and push notification lifecycle callbacks. See [LifeCycleCallbacks](#lifecyclecallbacks) and [PushNotificationCallbacks](#pushnotificationcallbacks) for details.
> 
> **IMP:** Make sure to `annotate` implementing class(es) with `AppEngageCallback`

1. LifeCycleCallbacks
2. PushNotificationCallbacks

## Class Reference
<a name="userattributeskey">
##### UserAttributesKey
</a>
> Use to pass pre-defined `User Attributes` to AppEngage on which advanced user segmentation is possible
```java
enum UserAttributesKey {
    FIRST_NAME,
    LAST_NAME,
    EMAIL,
    BIRTH_DATE,
    GENDER,
    PHONE,
    COMPANY;
}
```
<br />
<a name="userprofile">
##### UserProfile
</a>

> Use to set `user profile data` in one go

```java
class UserProfile {
    public static class Builder {
        public Builder setEmail(String email) {}

        public Builder setBirthDate(Integer year, Integer month, Integer day) {}

        public Builder setPhoneNumber(String phoneNumber) {}

        public Builder setGender(String gender) {}

        public Builder setFirstName(String firstName) {}

        public Builder setLastName(String lastName) {}

        public Builder setCompany(String company) {}

        public UserProfile build() {}
    }

    Map<String, Object> getUserData() {}
}
```


<br />
<a name="appengageconfig">
##### AppEngageConfig
</a>
> Use to get `AppEngage Configuration` information

```java
public class AppEngageConfig {

	boolean getLocationTrackingFlag()	//Returns whether location tracking is enabled or disabled
    
	boolean getAutoGCMRegistrationFlag()	// Returns true if GCM Registration is handled by AppEngage, else false.
    
	String getAppEngageKey()	// Return AppEngage issued licence code
    
	String getGcmProjectNumber()	// Returns project number used for GCM registration in case of automatic GCM registration
    
	String getAppEngageVersion()	// Returns AppEngage SDK version
    
	ReportingStrategy getEventReportingStrategy()	// Returns current event reporting strategy
}
```


<br />
<a name="logger">
##### Logger
</a>
> Use to set `AppEngage Logging level`

```java
class Logger {
    static final int SILENT = -2;
    static final int QUIET = -1;
    static final int NORMAL = 0;
    static final int DEBUG = 1;
    static final int VERBOSE = 2;

    static void setLogLevel(int _sLogLevel)

    static void v(String c, String s) // Verbose
    static void d(String c, String s) // Debug
    static void i(String c, String s) // Info
    static void w(String c, String s) // Warning
    static void w(String c, String s, Throwable tr) // Warning
    static void e(String c, String s) // Error
    static void e(String c, String s, Throwable tr) // Error
}
```



<br />
<a name="reportingstrategy">
##### ReportingStrategy
</a>

> Sets Reporting Strategy used by AppEngage to `sync event / user / other data` with servers

```java
enum ReportingStrategy {
    BUFFER,	// Data is first buffered in database, and then synced with server in a batch after a certain criteria has been met (DB-Size and/or Time-Since-Last-Sync)
    
    FORCE_SYNC	// Data is synced immediately with the server
}
```

<br />
<a name="lifecyclecallbacks">
##### LifeCycleCallbacks
</a>
> Listen to `GCM` and `App Install` / `App Upgrade` Callbacks
> 
> Make sure to `annotate` with `AppEngageCallback`

```java
public interface LifeCycleCallbacks {

    void onGCMRegistered(Context context, final String regID);

    void onGCMMessageReceived(Context context, final Intent intent);

    void onAppInstalled(Context context, final Intent intent);

    void onAppUpgraded(Context context, final int oldVersion, final int newVersion);
}

```

<br />
<a name="pushnotificationcallbacks">
##### PushNotificationCallbacks
</a>
> Listen to AppEngage `Push Notification Callbacks`
> 
> Make sure to `annotate` with `AppEngageCallback`

```java
public interface PushNotificationCallbacks {

    PushNotificationData onNotificationReceived(Context context, PushNotificationData pushNotificationData);

    void onNotificationShown(Context context, final PushNotificationData pushNotificationData);

    void onNotificationClicked(Context context, final PushNotificationData pushNotificationData);

    void onNotificationDismissed(Context context, final PushNotificationData pushNotificationData);

    void onNotificationActionClicked(Context context, final PushNotificationData pushNotificationData, final String buttonId);
}
```
