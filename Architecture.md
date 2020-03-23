# Architecture

This page give some information about Dexcalibur internals. 
Each main components are described below. 

## 1.Hook manager

The Hook Manager has several tasks including:
- to generate source code of hook when you click on "Probe" button
- to complete/customize hook templates provided by inspectors
- to maintain mapping between Method/Class object into internal DB and associated hooks
- to handle message sent by hooks, and to store it into HookSession object
- to invoke callback from inspector (which can trigger event)
- to make source code of the final Frida's script inclusing all enabled hooks, shared variables and dependancies
- to enable/disable hook


Picture below summarizes what it is included into HookManager object. 
[Hook manager architecture](https://raw.githubusercontent.com/FrenchYeti/dexcalibur-doc/master/pictures/v0_6_3_hook_manager.png)
