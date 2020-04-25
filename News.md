Latest news can be found here.

## [EDITING] 18 April 2019 - Future release, Big update

Since several month i prepare the future Dexcalibur release (0.7). Today, i would share with you some improvements and new features. 

Each following improvement/feature are detailled into a dedicated section below.
- Easy install though NPM (no more configuration / dependencies to handle manually)
- Rethinking all workflows
- Platform Manager: improvement of analysis by selecting target platform or using device as a target 
- Project management: abilities to switch betwen project from a single instance
- Device management: device profiling, install/start frida server, and more ...  
- Smali VM

### 1. Easy NPM Install / installer

Several users encountered issues caused by misconfiguration, missing dependencies or error related to dependencies version (mismatch of Frida or ADB major version, ...).


Before v0.7, in order to install Dexcalibur, you had to fill a file manually with several absolute path and you had to have at least:
- nodejs >= 12
- java >= 8
- apktool
- adb / android platform tools
- baksmali (packaged with dexcalibur)

Today, Dexcalibur requires only :
- nodejs >= 12
- java >= 8

Now, Dexcalibur can be installed by doing:
```
$ npm install -g dexcalibur
$ dexcalibur
```

At first run, Dexcalibur starts "Install mode":

*Step 1 :*
![step1](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/dxc_installer-step1.png)

*Step 2 :*
![step2](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/dxc_installer-step2.png)


### 2. Dexcalibur launch

Previous versions required several options such as : --app=<package> , --pull , --port=<webport> , ...
If you use Dexcalibur for the first time, the correct value of option 

When Dexcalibur is installed by using NPM, it can be launch with this simple command. No more options are required !
```
$ dexcalibur
```

By default, the web server listens on port 8000. Open your browser and visit `http://127.0.0.1:8000`. 

The new "home page" (below) appears. 


![home page](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/splashscreen.png)

As you can see into the screenshot below, this page offers several actions related to project management and engine configuration:

*Project actions:*
 - Open a recent projects
 - Open an existing project into the workspace
 - Analyze an APK (create a new project)
 - Select an application into a device connected to the computer.
 - Import a project 
 
 *Engine actions using Dexcalibur marketplace:*
 - Install additional platform image (to perform more accurate analysisp
 - Install device profile 
 - Install plugins (inspectors)
  
  
  
List and select an application to analyze
 
![home page](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/splash_select_app.png)

### 3. Platform Manager

Why Dexcalibur was not able to hook automatically `InMemoryDexClassLoader` ? Because Dexcalibur was based by default on Android API 24 (which not contains this method).

Now you can install/select the Android API to use during analysis.

Use the `Platform Manager` to install additional Android API (called platforms). The list of platforms available is retrieved from Dexcalibur-registry. 
![home page](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/dxc_platform_manager.png)

Into Dexcalibur typology a `platform` is a collection of binaries/classes/symbols/... provided by a specific platform. There are commonly two source for such information:
- Android/misc SDK
- Framework/boot.oat from real device 

When Dexcalibur scans an application it starts by a short static analysis of the target platform in order to index Android API classes, internal classes, and if possible constructor specfic classes. 

### 4. Device Management

Now, for several reasons explained later, the device running frida-server is a master piece of Dexcalibur logic. 

- Download/install/start a compatible version of frida-server/frida-gadget is boring : let's Dexcalibur do it. 
- If you should understand an application into a specific context, you should be able to detect calls of undocumented Android methods, so lets Dexcalibur select better platform : Android API from the SDK (android.jar), or Android API from the device (boot.oat)  

Before to start to use Dexcalibur, the device manager allows you to *enroll* a device. Device enrollment performs:
- Device profiling: gather properties, permissions, metadata, ...
- Install compatible frida-server into the device
- Download the Android API binary (android.jar) from Android SDK as a DEX file in order to perform analysis of the platform.

![Device Manager](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/devmanager_splash_list.png)



## 02 December 2019 - New feature : auto save

Did you never be afraid to lost your work when Dexcalibur crashes/exits ? Actually, a backup mechanism already exists, however you need to go into Setting -> Save menu, so it can be boring...   

New feature "Auto-save" allows you to automatically backup (into a file) Dexcalibur changes such as hook status, hook code, and aliases, when these are modified. At next Dexcalibur start, backuped data will be automatically restored.

![Auto-save status](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/autosave_status_on.png)

You can turn ON/OFF  auto-save by clicking on corresponding switch at right of navbar.

![Auto-save status](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/autosave_status_off.png)

Auto-save backup data into a file when :
* you **change hook code and click "save"** or "save & reply" from hook editor
* you **enable/disable a hook** from its on/off button
* you **rename a class/method/field**

 Link:
[https://github.com/FrenchYeti/dexcalibur/releases/tag/V0.6.1](https://github.com/FrenchYeti/dexcalibur/releases/tag/V0.6.1)



## 06 November 2019 - Release v0.6 + Major update

Latest version comes with lot of new features and changes.

New features:
- Hook editor helpers: the hook editor embeds a navigation bar of hook snippets for Java and native hooks.
- Polymorphic hook: static value into hook code can filled/updater automatically with data from previous application+hook execution. Allowing to do evolutive black-list.
- [Partial] Dynamic hook loading: when a method is hooked, the condition to load/unload hook can be defined. Now, you can configure from UI a hook to deploy only if a condition is verified at runtime. 
- All hooks can be enabled/disabled by a single click.
- Execution can be launch from hooking dashboard, so hook messages from previous execution are flushed before to display hook logs page.

Fix:
- Device Manager has been partially rewritten to be more stable. Default device where hooks should be deployed can be selected.
- Save/Open feature has been patched and UI redesigned.
- "Delete hook" works again.

Changes:
- Migration to Bootstrap 4
- UI theme
- Remote errors are now partially rendered client-side 
- UI is more compact, so more data can be displayed into a screen.
- Navigation bar has been rewritten to offer fastest access to features/inspectors
- 


## 07 July 2019 - New feature : "just-in-time decompilation" is available :)

One of the main features of Dexcalibur have been implemented : "just-in-time decompilation". Dexcalibur hooks DexClassLoader constructor in order to detect dynamic loading of additional Dex file, potentially deciphered at runtime. Then, when new bytecode chunk is detected, the chunk is sent back to the server, it is decompiled and the internal database (graph) is updated with discovered elements such as packages, classes, methods, fields, strings, byte array, and so.

The picture below shows the smali code of a function defined into a Dex file deciphered from a JNI library and loaded dynamically. Here, it is a crackme application and the static string is the flag. 


![JITD](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/JITD_v1_flag.png)

Freshly discovered classes can be explored and its methods hooked :)
![JITD](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/JITD_v1_class.png)
 

 

## 26 June 2019 - UI Improvements : fields setter/getter

The views and queries involving cross references have been improved. The information is more accessible for API user : **Field** have now *getters* and *setters* properties.


The UI have been improved in order to display getters and setters. You can get xref for a field from the Class  view.


![Xrefs of a field](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/gsetter_field.png)  


![Explore fields from a class](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/field_xref.png)  


When you search where a given symbol is referenced, if this symbol is a field, you can see if the instruction referencing this field sets or gets the value. 

![Results of searching call or use of a symbol](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/find_call_setget_field.png)  

## 25 June 2019 - Dexcalibur dockerfile improvements

I patch a long term issue : frida cli was not installed -_-"
The PATH env variable is now more complete, so you don't need to know the absolute path of ADB or Frida tools.

Theorically, the docker images contain the same major version of frida and node-frida, so there is probably lesser issue than a fresh install of Dexcalibur with an existing frida cli/server setup.
    

## 23 June 2019 - New inspector : Issue Inspector

Actually, this inspector catch the call to the SecurityException constructor and tags resulting logs as "Error". It is usefull in order to catch permission issue without use "adb logcat".



  
