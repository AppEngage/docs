**NOTE:** 
> AppEngage needs `Google Play services` and `Android Support Library v4`. 

> `Android Support Library v13` can be also be used instead of v4 if your project already requires it.

#### Follow these steps to integrate AppEngage into your existing/new Android Studio Project
1. Click on `File` -> `New` -> `New Module`

2. Choose `“import .JAR/.AAR Package”` and click `“Next”`

3. Now locate the `AppEngage sdk .aar/.jar file` in the downloaded library in the `“File name”` field. Once located, the `“Subproject name”` should auto-populate. If not, just give it any name like “appengage” and click `“Finish”`. This is now imported as a module in your workspace. We will be referencing this in next step.

4. Now its time to refer to this `module` in your application project. Click `File` -> `Project Structure`. `“appengagesdk”` should be visible as a module under `“Modules”` section. Select your application module from `“Modules”` section to which you want to integrate `AppEngage SDK` with. Click on `“Dependencies”` tab on the top right side of the screen. Click on `“+”` button to add a new dependency to your application module __(at bottom left)__

5. From the `drop-down`, choose `“Module dependency”` and select `“:appengagesdk”` module from the pop-up window and click `“OK”`.

6. The `AppEngage SDK` setup is now complete. You can confirm it by looking at `“build.gradle”` of your app module. The `AppEngage SDK` should be listed as a dependency in `“dependencies”` section.
