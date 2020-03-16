An image of Dexcalibur is available on the [DockerHub](https://hub.docker.com/r/frenchyeti/dexcalibur)

The USB communication behavior from the device to the container differs between Linux and MacOS. So, each case are explained below.

## A. Linux host

If you expirement USB issue, follow this step-by-step guide :

### 1. Connect the device to your computer

If the device asks the communication mode, select "Transfert file". 
If ADB is installed on the host, run the following command.
```
adb devices   
``` 

### 2. Found the device ID and path

If you don't know the device ID do :
```
sudo dmesg | grep -i usb
```
Search the string */dev/ttyUSBx*. If you not found it, list all USB buses :
```
ls /dev/bus/usb/* 
```  
If the last thing you have connected to your computer is the device, search the higher ID. It is 010 in command output above.


### 3. Run the container and bind the good device

Start the container 
```
docker run -it -v <workspace_path>:/home/dexcalibur/workspace -p 8080:8000 --device=<device_path> frenchyeti/dexcalibur
```
 
In our case, the command will be  :
```
docker run -it -p 8080:8000 -v <your_workspace_path>:/home/dexcalibur/workspace --device=/dev/bus/usb/001/010 frenchyeti/dexcalibur
```

### 4. Kill host ADB server

On the host :
```
adb kill-server
```

### 5. Run ADB into the container

List connected device *from the container* :
```
adb devices
```

It is possible the device asks you to trust the computer fingerprint.

### 6. Start Dexcalibur

```
./dexcalibur --app=<target.app> --port=8000
```

## B. MacOs host 