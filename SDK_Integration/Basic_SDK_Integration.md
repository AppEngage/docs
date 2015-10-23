#### 1. Automatic
This approach is only supported in `Android 4.0`(API level 14, >92% coverage as of August 2015) and later. So, if you are developing your app to support minimal `14 API level`, we **recommend** to use this approach as it is easy to integrate and requires less boilerplate.

##### If you don't have a custom Application class, create one and specify the name in your AndroidManifest.xml

```xml
  <application
    android:name=".MyApplication"
    android:icon="@drawable/ic_launcher"
    android:label="@string/app_name">
```

Now, In your Application's `onCreate()` method, register `AppEngageActivityLifeCycleCallbacks`.

```java
  public class MyApplication extends Application {
    @Override
    public void onCreate() {
      super.onCreate();
      // Register AppEngageActivityLifeCycleCallabcks
      registerActivityLifecycleCallbacks(new AppEngageActivityLifeCycleCallbacks(this));
    }
  }
```

#### 2. Manual
Use this approach if you plan to target devices `below Android API level 14 also` ( >94% coverage as of August 2015)

##### 2.1 If you don't have a custom Application class, and can create one, create an new Application class, and specify the name in your AndroidManifest.xml

```xml
  <application
    android:name=".MyApplication"
    android:icon="@drawable/ic_launcher"
    android:label="@string/app_name">
```

Now, In your application's `onCreate()` method, integrate `AppEngage`:

```java
public class MyApplication extends Application {
  @Override
  public void onCreate() {
    super.onCreate();
    // Integrate Localytics
    AppEngage.engage(this);
  }}
```

##### 2.2 In case you don’t plan to create a new Application class, follow these steps:
From `onCreate()` of your `Launcher Activity` call `AppEngage.engage(APPLICATION_CONTEXT)`:

```java
@Override
public void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  AppEngage.engage(getApplicationContext());
}
```
> **Note:** `AppEngage.engage(APPLICATION_CONTEXT)` should be called only once, either from `Application onCreate()` or from `Launcher Activity onCreate()`

From `onResume()` of **each** Activity call `AppEngage.resume(ACTIVITY_CONTEXT)`:

```java
@Override
public void onResume(){
  super.onResume();
  AppEngage.get().analytics().resume(this);
}
```

From `onPause()` of **each** Activity call `AppEngage.pause(ACTIVITY_CONTEXT)`:

```java
public void onResume(){
  super.onResume();
  AppEngage.get().analytics().pause(this);
}
```

**NOTE:** Make sure to repeat above 2 steps(the `onResume()` and `onPause()`) for every other Activity in your application. This enables `user session tracking` and `funnel generation` for your app visitors.
