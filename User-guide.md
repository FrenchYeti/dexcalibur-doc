# 1. Start 

Dexcalibur is mainly used as a web application, but can be integrated into a NodeJS script or used directly from the NodeJS terminal (with *quickstart.js*). This user guide is focus on the web app.

## 1.a Analyze an application installed on a device

**Actually the device should be rooted in order to perform hook, but a way to hook the application with non-rooted device will be available ASAP**

You should know the package name of the targeted application in order to start. If you don't know it, use the package manager. 

Start Dexcalibur, the **--pull** parameter order to dexcalibur to download the application APK file from the device  :
```
./dexcalibur --app=<my.example> --port=8080 --pull
``` 
  
When the message *Server started on : 8080* appears, open your browser and goto to *http://127.0.0.1:8080*.

## 1.b Analyze an application from an APK

You want discover the application and you have only the APK file ? use `--apk` parameter.

```
./dexcalibur --app=my.app --apk=$PWD/../../myapp.apk --port=8000
```
 
# 2. Analyze

The aim of Dexcalibur is to offer a way to perform several kind of analyze and correlate data.
It starts by analyzing statically an APK file and dex files, instrumenting the application and analyzing data gathered by the hooks. 
 
Dexcalibur starts by disassembling the *.dex* files  contained into the APK file. It implies if it is a multidex  application and some dex files are encrypted, then Dexcalibur doesn't be aware of some classes. 

Before the first run, the internal database contains only element discovered statically. So, some xref or classes can exists into the application but not be found by Dexcalibur. After each run, if new elements are discovered, then the database is updated with these elements. 
  
## 2.a The "Bytecode explorer" tab

**This view is under development and could be remove**

This tab is the home page of Dexcalibur, its aim is to draw a quick view of the application. Give elements which allow an analyst to identify quickly the obfuscation properties, search class, and so.  

## 2.b The "Application object finder" tab

This view offers a search engine allowing you to request any kind of entity (class, method, field, static string, call, byte array, ...) by any type of property or constraints like : 

* Search a method by the type of the arguments 
* Search callers where the called function return a value of the expected type
* Search a static strings by its value
* Search a static byte array by its size or tags
* Search a class by its inherited class or ancestors
* Search methods invoked dynamically
* Search class loaded from an external Dex file
* ...

Once you have found an element, you can explore its properties / parent / relations.

For example, if the element is a method, you can see its properties : alias, properties, smali bytecode, xref from/to, xref from graph, and add a probe.

**Important : any pattern set after ":" symbol into a request is a RegExp pattern, so if you expect an exact match please add "^" and "$" symbols**

### 2.b.1 Pre-built request

Buttons under Search input are prebuilt requests. Click on the button, enter your RegExp patterns and click ok. It will craft a request. By clicking "Search", you perform your search over the application graph and Dexcalibur internal database.

### 2.b.2 Write custom request

Available fields and opérations, callable into a request, are documented in the dedicated page : Finder API.

Fields are subject to change and can be customized by plugins, so documentation cannot be exhaustive.

### 2.b.3 Rename symbol

Symbols - such as method name, class name, field name and so - can be aliased through the "rename" button. Its purpose is to improve understanding by broadcasting the alias into other views/features like xref.

Aliases can be saved and share with other users. If several users work remotely with the same instance, they share automatically all aliases.


## 2.c The "Hooks" tab

The view `/pages/probe.html` summarizes all hooks existing for this application. It contains:

* Dashboard
* Logs
* Hook set

### 2.c.1  The Hook Dashboard 

By default, the hook dashboard shows all hooks in the app. The hooks are taken from the files in folder `inspectors/`.

You can turn a hook ON/OFF by clicking on the button ON/OFF. If the hook is turned to OFF (button is red and display "OFF"), the hook will not be deployed in the next run. The hook code is not lost and you can turn it ON more later.

Hook can be move by clicking on the red trash button. Be aware, if it's a built-in hook it can breaks feature like dynamic discovering. It is generally a better choice to turn OFF the hook. 

## 2.c.2 The "Hook logs"

When an application is spawned, page `/pages/probelog.html` is shown. It contains the list of all collected events for the activated hooks. 

## 2.c.3 "Hook sets"

The hook set tab contain all the scanners that are present in folder `src/scanner/`, for example:

* Deobfuscater.js
* KeystoreScanner.js
* RootDetection.js

## 2.e The "Security scanner" tab

## 2.f The "Inspectors" tab

## 2.g The "Device manager" tab


# 3. Generate and customize hooks

# 4. Scanners


# 5. Inspectors

# 6. Conformity scan

## 6.A SSL Pinning  (work in progress)

`src/Security.js` declares three heuristics related to SSL pinning. Such heuristics use Dexcalibur's static analyzer to identify if certificate verifying pattern are detected over the code. 

Results are better if the application has been executed several times with instrumentation before to perform the scan.

* custom_keystore_based: The application seems use a custom keystore where the custom certificate is stored
* harcoded_certificate_signature: SSL Pining uses a comparaison with an hardcoded signature
* okhttp3_pinner: SSL Pining by the okHttpClient and a custom keystore

# 7. API

`msg`: `http://localhost:8000/api/probe/msg` returns a JSON file with all the event 

```json
         {
            "action" : "Update",
            "after" : false,
            "before" : true,
            "data" : {
               "name" : "android.app.servertransaction.StopActivityItem"
            },
            "hook" : "Zjg3YmRjOTA3ZTVjNzdhNDIxNGM2Yzg5YTM5OGQ4N2Y=",
            "isIntercept" : false,
            "match" : true,
            "msg" : "Class.forName()",
            "tags" : [
               {
                  "style" : "purple",
                  "text" : "dynamic"
               }
            ]
         }
```


[Home](./Home.md)
