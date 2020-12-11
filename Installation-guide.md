## Install requirements

Install these requirements before to continue.

-   NodeJS  12.x LTS (before that version UInt64 buffer contained into the targeted bytecode are not fully supported, after that version see  [Issue #27](https://github.com/FrenchYeti/dexcalibur/issues/27) )
-   [Frida](https://frida.re/) on the host (any version, create an issue if you experiment a problem) for example use `pip install frida-tools`.
-   Java > 8


## Install

### From NPM package

Run command:
```
$  npm install -g dexcalibur
```

And start Dexcalibur with:
```
$  dexcalibur
```
Visit [http://127.0.0.1:8000](http://127.0.0.1:8000) and follow instruction.

Your default port number 8000 is already in use ? Specify a custom port by using `--port=<number>` like `$  dexcalibur --port=9999`


Fill the form with the location of your workspace and default listening port, and submit it. 
The workspace will contain a folder for each application you reverse using Dexcalibur.  
![Install configuration](https://github.com/FrenchYeti/dexcalibur-doc/raw/master/pictures/dxc_installer-step1.png)

Dexcalibur will create the workspace if the folder not exists. A standalone version of android platform tools, APKtool, and more will be downloaded into this workspace.
![Running install](https://github.com/FrenchYeti/dexcalibur-doc/raw/master/pictures/dxc_installer-step2.png)


Once install is done, restart Dexcalibur by killing it and doing (again)
```
$ dexcalibur
``` 

### From sources

*TO DO*


## Run

- Ensure your device is connected and detected (`adb devices`, `adb shell`...).
- **Enroll** your device using the Web UI (Home > Device Manager). If everything goes well, Dexcalibur will automatically download and push a **Frida server** to your device.
- Create a project and start using Dexcalibur :)

In most situations, Dexcalibur will automatically start the Frida server on your device. However, there are some cases where Dexcalibur is unable to start it or get its status:

- if you set a custom name for frida-server binary
- if you move frida-server at a custom location
- if you use another way to inject frida

In those case, manually start [frida-server](https://frida.re/docs/android/) on your device (you should adapt the command) :
```
adb shell su -c "/data/local/tmp/frida-server"
```


## Troubleshooting

### Dexcalibur fails to start (bad interpreter)

If you get this error message *./dexcalibur: bad interpreter: /usr/bin/node: no such file or directory*, ensure *nodeJS* is installed and the path is good.

If you don't know the path of the NodeJS interpreter, you can run dexcalibur by this way :
```
node --max-old-space-size=8192 ./dexcalibur <your params>
```
The parameter *--max-old-space-size* is optional but often required with large APKs.
