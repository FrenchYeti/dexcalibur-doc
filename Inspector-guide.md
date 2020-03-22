
An inspector is a pluggable piece of the Dexcalibur engine. It is designed to perform additionnal analysis, to listen events emitted by others components, to hook methods and to treat intercepted data. 

Optionally, an Inspector can extends the GUI by adding new view.

## 0. Why do i develop an inspector ? 

Dexcalibur can help you to extend Dexcalibur by:
* Add built-in hook using or not the Dexcalibur search engines
* Add listener of events which are emitted by other inspectors  
* Add custom post-treatment when a hook is trigged (for exemple ELF parsing)
* Extend GUI (for exemple, in order to dump keystore content intercepted or show http(s) communications) 
* Add a function called before or after each application start
* and so

A partial list of existing - listenable - events are listed here : TODO

## 1. Skeleton

Each inspector must be inside separate folder and the inspector filename must be named "inspector.js". 

```
dexcalibur/
    inspectors/
-------> MyInspector/
            inspector.js 
            service/ 
                main.js
            web/
                main.html
```
  
The main file is "inspector.js" where the Inspector is declared. The folders */service/* and */web/* are optional, and should be used only if you expect to add a GUI to your inspector.

If your inspector has a GUI, the user will be able to display it by clicking the "show" button, else there is not this button. The picture below shows the "Data classifier" inspector has not GUI, but the "Bytecode cleaner" has one. 

![List of inspector and "show" button](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/inspector_howto_show.png)
 

## 2. Declaring an inspector

All inspectors contained into the folder /inspectors/ are loaded when Dexcalibur start. 
The code block below is the minimal amont of source code required in order to declare a new inspector.

```
const HOOK = require("../../src/HookManager.js");
const API = require("../../src/Inspector.js");

var IssueInspector = new API.Inspector({
    hookSet: new HOOK.HookSet({
        id: "IssueObserver",
        name: "Issue observer",
        description: "Track and save security exception and device logs"
    })
});

module.exports = IssueInspector;
```

The first step is to create a new instance of Inspector class and its hookset. The field **id** is mandatory and it is the unique identifier of the inspector into Dexcalibur. The fields **name** and **description** are only used in order to give more information about the inspector to your users. And finally, the instance must be exported like any node module.

Now, the next step is to declare hook or event listeners.

## 3. Declaring a hook

Each inspector contains an hook set where all hooks are declared. Two type of hook are supported:

* probes: log only parameter and return values (generated)
* intercepters: do something which must be defined


When you know the signature of method(s) to hook, you can declare a hook for these signatures.
The code below shows how to declare a hook for each SecurityException constructors.  

If you expect to hook a single method, the field **method** should contains the method signature as a String.  
```
IssueInspector.hookSet.addIntercept({
    method: "java.lang.SecurityException.<init>()<void>",
```

Else, if you need to hook several methods but you expect to do samething with your hook, the value of the **method** field can be an array of signature, such as in the following picture.
```
IssueInspector.hookSet.addIntercept({
    method: [
        "java.lang.SecurityException.<init>()<void>",	
        "java.lang.SecurityException.<init>(<java.lang.String>)<void>",	
        "java.lang.SecurityException.<init>(<java.lang.String><java.lang.Throwable>)<void>",	
        "java.lang.SecurityException.<init>(<java.lang.Throwable>)<void>"
    ],
```
You can insert your hook, before, after or in place of, the original method. Declare the hook code into the corresponding field:

* interceptBefore : the code will be executed before to call the hooked method
* interceptAfter : the code will be executed after to call the hooked method
* interceptReplace : the hooked method will be replace by the given hook code

Finally, you can define a callback function into the field **onMatch**, this function will be called server-side (where dexcalibur runs) only if the message sent back to the server contains **match: true**. This behavior will be describe into another section. The example below contains a callback which emits a new event "hook.except.security.new" when a new SecurityException is created. This event can now be catch by any other inspector or it self.

```
IssueInspector.hookSet.addIntercept({
    method: [
        "java.lang.SecurityException.<init>()<void>",	
        "java.lang.SecurityException.<init>(<java.lang.String>)<void>",	
        "java.lang.SecurityException.<init>(<java.lang.String><java.lang.Throwable>)<void>",	
        "java.lang.SecurityException.<init>(<java.lang.Throwable>)<void>"
    ],
    onMatch: function(ctx,event){
        IssueInspector.emits("hook.except.security.new",event);
    },
    interceptBefore: ` 

            var msg="";    
            if(isInstanceOf(arg0,"java.lang.String"))
                msg = arg0;
            else
                msg = "<unknow>";

            send({ 
                id:"@@__HOOK_ID__@@", 
                match: true, 
                data: {
                    msg: msg
                },
                after: false, 
                msg: "SecurityException", 
                tags: [{
                    style:"black",
                    text: "error"
                }],
                action: "" 
            });
    `
});
```

## 4. Declaring an event listener

## 5. Examples of inspectors

### SSL Pinning

`src/Security.js` declares three hooks related to SSL pinning:

* custom_keystore_based: The application seems use a custom keystore where the custom certificate is stored
* harcoded_certificate_signature: SSL Pining uses a comparaison with an hardcoded signature
* okhttp3_pinner: SSL Pining by the okHttpClient and a custom keystore

