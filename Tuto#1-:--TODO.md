
This tutorials is aimed at introduction to Dexcalibur. Future tutorials will covers specific features/uses cases.
During this tutorials we will discover how study an android application - WhatsApp in our case - using Dexcalibur.

## 1. Setup

I assume you have already installed and configured Dexcalibur following installation guide, you have already Frida installed on your desktop, and a comptible Frida server running on your device.

You are free to install WhatsApp or another application in order to follow next steps.



## 2. Start Dexcalibur and create a new project

To download, start and analyze the <target> application, type :
```
./dexcalibur  --app=com.whatsapp --port=8000 --pull
```

The aim of each parameters :
* **--app** is mandatory and specify the application package name to analyze.
* **--pull** is optional, if set Dexcalibur downloads the application package from the connected Device.
* **--port** is optional, the value is webserver port where GUI is available (here http://127.0.0.1:8000 ).

---

**Tips:**

*To be sure to conserve your setup, I greatly encourage you to move your dexcalibur configuration file out of Dexcalibur folder and to start Dexcalibur with --config params.*
```
./dexcalibur --app=com.whatsapp --config=$PWD/../config.js --port=8000 --pull
```

---

If you encounter shell errors when ./dexcalibur starts, then check shebang (```#!``` symbol) at begin of ./dexcalibur file. The node binary path can be false.

Now, Dexcalibur should print some logs into your terminal looking like (some data have been omit):
```
[INFO]  Given configuration file loaded
...
[INFO] [Inspector::injectContext][FrontController] saver registered !
[INFO] 

███████╗ ███████╗██╗  ██╗ ██████╗ █████╗ ██╗     ██╗██████╗ ██╗   ██╗██████╗
██╔═══██╗██╔════╝╚██╗██╔╝██╔════╝██╔══██╗██║     ██║██╔══██╗██║   ██║██╔══██╗
██║   ██║█████╗   ╚███╔╝ ██║     ███████║██║     ██║██████╔╝██║   ██║██████╔╝
██║   ██║██╔══╝   ██╔██╗ ██║     ██╔══██║██║     ██║██╔══██╗██║   ██║██╔══██╗
███████╔╝███████╗██╔╝ ██╗╚██████╗██║  ██║███████╗██║██████╔╝╚██████╔╝██║  ██║
╚══════╝ ╚══════╝╚═╝  ╚═╝ ╚═════╝╚═╝  ╚═╝╚══════╝╚═╝╚═════╝  ╚═════╝ ╚═╝  ╚═╝
 v0.6                                                          by @FrenchYeti 
╔════════════════════════════════════════════════════════════════════════════╗
║ How to use ?                                                               ║
║ > const Dexcalibur = require('./src/Project.js')                           ║
║ > var project = new Dexcalibur('com.example.test')                         ║
║ > project.useAPI('android:7.0.0').fullscan()                               ║
║ > project.find.method('name:loadLibrary')                                  ║
║                                                                            ║
║ Read *.help() ! =)                                                         ║
╚════════════════════════════════════════════════════════════════════════════╝

[*] Working directory : /Users/salade/Tools/dxc/workspace/com.whatsapp
[INFO] Unrecognized key (single token) : usb:336658432X
USB usb:336658432X [ 'usb', '336658432X' ]
╔═══════════════════════════[ Android devices ]═══════════════════════════════╗
║ 02581073e1c3398f/usb/336658432X                                             ║
╚═════════════════════════════════════════════════════════════════════════════╝

[*] Device selected : 02581073e1c3398f
[*] Package found
[*] Package downloaded to /Users/salade/Tools/dxc/workspace/com.whatsapp/com.whatsapp.apk
S: WARNING: Could not write to (/Users/salade/Library/apktool/framework), using /var/folders/76/12f7yq_s09v0skvs08ytc7000000gn/T/ instead...
S: Please be aware this is a volatile directory and frameworks could go missing, please utilize --frame-path if the default storage directory is unavailable
[*] APK decompiled in /Users/salade/Tools/dxc/workspace/com.whatsapp/dex
[INFO] Scanning platform android:7.0.0
[*] Smali analyzing done.
---------------------------------------
[*] 3683 classes analyzed. 

[*] Start object mapping ...
------------------------------------------
[INFO] DB size : 3683
[INFO] 200/3683 Classes mapped (android.app.usage.UsageStatsManager)
...
[INFO] 3683/3683 Classes mapped (org.xmlpull.v1.sax2.Driver)
[*] 35556 methods indexed
[*] 16380 fields indexed
[*] 130193 instructions indexed
[*] 33921 method calls mapped
[*] 2713 field calls mapped
[INFO] Scanning default path : /Users/salade/Tools/dxc/workspace/com.whatsapp/dex
[*] Smali analyzing done.
---------------------------------------
[*] 12791 classes analyzed. 

[*] Start object mapping ...
------------------------------------------
[INFO] DB size : 16483
[INFO] 200/12791 Classes mapped (X.03h)
[INFO] 400/12791 Classes mapped (X.07B)
...
[INFO] 12791/12791 Classes mapped (pl.droidsonroids.gif.GifInfoHandle)
[*] 86754 methods indexed
[*] 63144 fields indexed
[*] 1454803 instructions indexed
[*] 362046 method calls mapped
[*] 245664 field calls mapped
[*] 6678 files analyzed
[*] 0 files analyzed
[INFO] [INSPECTOR][TASK] Trying to restore previous data of DynLoaderInspector ... 
[PROBE][HOOK SET] Add :  android.os.Parcelable$ClassLoaderCreator.createFromParcel(<android.os.Parcel><java.lang.ClassLoader>)<java.lang.Object>
...
[INTERCEPT][HOOK SET] Add :  java.lang.Runtime.loadLibrary(<java.lang.String>)<void>
Server started on : 8000
[INFO] [AppTopo][activity] Internal dependencies mapped for : org.npci.commonlibrary.GetCredential
[INFO] [AppTopo][activity] Internal dependencies mapped for : com.whatsapp.payments.ui.IndiaUpiPaymentsTosActivity
...
[INFO] [AppTopo][receiver] Internal dependencies mapped for : com.google.firebase.iid.FirebaseInstanceIdReceiver
```  

Now, Dexcalibur has successfully downloaded, dissembled Dex and analyzed statically (partially) the application. You can now open your favorite browser and set the URL : 
```
http://127.0.0.1:8000
``` 

## 3. Discover application content

Application information, such as manifest content / permissions / activities / receivers / services / providers, are available through "APK" tab.

TODO_IMG_NAV_BAR

From this interface, you can explore raw manifest, permissions, activities and more. 
  


 