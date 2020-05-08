Latest news can be found here.

## 8 may 2019 - Dexcalibur 0.7.0 : New release, Big update

Since several months i prepare the new Dexcalibur release (0.7). Today, i would share with you some improvements and new features. 

Latest release can be found here :
[Github](https://github.com/FrenchYeti/dexcalibur)
[NPM](https://www.npmjs.com/package/dexcalibur)

Each following improvement/feature:
- Easy install though NPM (no more configuration / dependencies to handle manually)
- Dexcalibur launch / new project
- Better control of frida-server and hooking session tart/stop frida server, unload hooks, re-spawn, clear hook logs, kill app
- Platform Manager: improvement of analysis by selecting target platform or using device as a target 
- Device management: device profiling, install/start frida server, and more ...  
- Smali VM (not detailled here)

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


### 2. Dexcalibur launch / new project / project switching without restart

Previous versions required several options such as : --app=<package> , --pull , --port=<webport> , ...
If you use Dexcalibur for the first time, the correct value for each option was difficult to find. 

Now, everythin have been autmated. Once you installed and create your workspace succesfully, just run command : `dexcalibur`
If you want work with big application, then increase heap size by adding options : `--max-heap <size in Mb>`  for exemple `--max-heap 8192`

By default, the web server listens on port 8000. Open your browser and visit `http://127.0.0.1:8000`. 

The new "home page" (below) appears. 

![home page](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/v0.7.0/dexc_v070_recents.png)

As you can see into the screenshot below, this page offers several actions related to project management and engine configuration:

*Project actions:*
 - Open a recent projects
 - Open an existing project into the workspace
 - Analyze an APK (create a new project)
 - Select an application into a device connected to the computer.
 - Import a project 
 
 *Engine actions using Dexcalibur marketplace:*
 - Install additional platform image (to perform more accurate analysis)
 - Install real device profile 
 - Install plugins (inspectors)
  
  
You can start a new project by selecting an application into a connected device, or analyzing a side-loaded APK.

 
#### 2.A List and select an application to analyze

Once you have enrolled your device, you can select an application installed and start to analyze it. If you hope to analyze vendor-specific app or system app, be aware some APK not contain bytecode (it will be detailled into a future post).
 
![home page](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/v0.7.0/dexc_v070_select.png)

It creates a new project and you will be able to share it with other peoples.
  
#### 2.B Open an APK
 

![home page](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/v0.7.0/dexc_v070_new.png)

You can analyze local and remote APKs by using one of three options availables:
* From a remote URL : the APK is downloaded
* By uploading your APK ( small APK ), allow you to run Dexcalibur on dedicated computer
* By entering absolute path of your local APK

The name of you project can be anything, but it must be unique. The package name is detected automatically.

Next, you should select the target platform to help dexcalibur to detect methods from Android framework during static analysis. You have several choice : 
* Choosing a specifc platform version
* lets Dexcalibur choose the platform according to Android manifest
* use the platform of the device where hooking is done

Two firsts choices allow you to use Dexcalibur only for static analysis purpose if you have not default device.


### 3. Better control of frida-server and hooking session


### 4. Platform Manager

Why Dexcalibur was not able to hook automatically `InMemoryDexClassLoader` ? Because Dexcalibur was based by default on Android API 24 (which not contains this method).

Now you can install/select the Android API to use during analysis.

Use the `Platform Manager` to install additional Android API (called platforms). The list of platforms available is retrieved from Dexcalibur-registry. 
![home page](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/v0.7.0/dexc_v070_pm.png)

Into Dexcalibur typology a `platform` is a collection of binaries/classes/symbols/... provided by a specific platform. There are commonly two source for such information:
- Android/misc SDK
- Framework/boot.oat from real device 

When Dexcalibur scans an application it starts by a short static analysis of the target platform in order to index Android API classes, internal classes, and if possible constructor specfic classes. Platforms are used during this step.

### 5. Device Management

Now, for several reasons explained later, the device running frida-server is a master piece of Dexcalibur logic. 

- Download/install/start/stop a compatible version of frida-server/frida-gadget is boring : let's Dexcalibur do it. 
- If you should understand an application into a specific context, you should be able to detect calls of undocumented Android methods, so lets Dexcalibur select better platform : Android API from the SDK (android.jar), or Android framework extracted from the device.

Before to start to use Dexcalibur, the device manager allows you to *enroll* a device. Device enrollment performs:
- Device profiling: gather properties, permissions, metadata, ...
- Download and Install compatible frida-server into the device
- Download the Android API binary (android.jar) from Android SDK as a DEX file in order to perform analysis of the platform.

Once your device has been enrolled, it is ready for hooking !

![Device Manager](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/v0.7.0/dexc_v070_dm.png)



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



  
