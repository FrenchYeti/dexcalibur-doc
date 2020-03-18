## 1 Install requirements

Install this requirements before to continue.

Desktop requirements :
-   NodeJS > 12.0.0 (else UInt64 buffer contained into the targeted bytecode are not fully supported) 
-   [Frida](https://frida.re/) (any version, create an issue if you experiment a problem)
-   Java > 8
-   APKTool

Device requirements :
-  Download and install Frida-server ([reference documentation]](https://frida.re/docs/android/))

## 2 Pull/build the dependencies

First, you need to rebuild the dependencies for your version of node.
Execute the following command from the repository containing the INSTALL.md file.
```
npm install
``` 

**You experiment some difficulties to install frida node module ?**
> If NPM says there is not a module build for your node version, don't worry ! Check your node version with the command "*node --version*", if your major version is less than 12 you need to upgrade your node setup  or use NVM (node version manager), else install manually an older version of the frida module with "*npm install frida@12.1.1*" for example. 

## 3 Configure

Modify the following lines, into the **config.js** file, with the good values. 
```
    // Dexcalibur src location
    dexcaliburPath: "/home/example/dexcalibur/src",
    
    // workspace location : folder where analyzed APK and data are stored
    workspacePath: "/home/example/workspace/",
    
    // ADB location
    adbPath: "/home/example/Android/Sdk/platform-tools/adb",
    androidSdkPath: "/home/example/Android/Sdk/",

    // APKTool location
    apktPath: "/home/example/tools/apktool",
```

If Java binary is not in your $PATH, add the absolute path to your configuration file:
```
    javaBinPath: "/your/java/bin/path"
```


## 4 Run

Ensure your device is connected and detected. 

Start [frida-server](https://frida.re/docs/android/) on your device (you should adapt the command) :
```
adb shell su -c "/data/local/tmp/frida-server"
```

From the dexcalibur folder, run : 
```
./dexcalibur --app=com.app.test --port=8000 --pull
```
If the **--pull** parameters is set, Dexcalibur will pull the targeted APK from the device.

If you need to specify an API version, use the --api parameter with the desired version.

```
./dexcalibur --app=com.app.test --api=android:7.0.0 --port=8000 --pull
```


## 5 (OPTIONAL) Use a different Android API version (default = Android 7.0.0)

Dexcalibur source contains the decompiled Android API 22 Stubs (Android 7.0.0). If you need to hook or search a method relaying to a method or object from Android API version > 22, you will need to download or build the required APIs image.
     
### 5.A. Import a prebuilt Android API images

The Android APIs image for each version will be published as soon as possible. 

### 5.B. Build your own Android API images, from the SDK Android.

(TODO...)

* Download the desired Android API by using the Android SDK Manager.
* Convert the **android.jar** file containing into a .dex file by using the Android **dx** tool.
```
mkdir /tmp/dxc
cp <ANDROID_SDK_HOME>/platforms/android_<api_version>/android.jar /tmp/dxc/android.jar
unzip /tmp/dxc/android.jar /tmp/dxc/android_classes
java -jar <ANDROID_SDK_HOME>/build-tools/lib/dx.jar --dex --core-library --output /tmp/dxc/android.dex /tmp/dxc/android_classes
```
* Smalify the resulting .dex file, and move *.smali files into the Dexcalibur folder
```
baksmali d <DEXCALIBUR_HOME>/APIs/android_<api_version>/android.dex 
```


## 6. Troubleshooting

### 6.A Dexcalibur fails to start (bad interpreter)

If you get this error message *./dexcalibur: bad interpreter: /usr/bin/node: no such file or directory*, ensure *nodeJS* is installed and the path is good.

If you don't know the path of the NodeJS interpreter, you can run dexcalibur by this way :
```
node --max-old-space-size=8192 ./dexcalibur <your params>
```
The parameter *--max-old-space-size* is optional but often required with large APKs.
