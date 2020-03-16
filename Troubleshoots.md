This page attempts to provide some solution to commonly issues.

# A - Dexcalibur fails to start (bad interpreter)

If you get this error message *./dexcalibur: bad interpreter: /usr/bin/node: no such file or directory*, ensure *nodeJS* is installed and the path is good.

It is often caused by the interpreter path in the head of the **./dexcalibur** script:
```
#!/usr/bin/node --max-old-space-size=4096
```

Ensure the path to your *node* program is the **/usr/bin/node**. If it differs, modify the first line of the ./dexcalibur file. 

Tip: if you don't know the absolute path of the NodeJS interpreter, you can run dexcalibur by this way :
```
node --max-old-space-size=8192 ./dexcalibur <your params>
```
The parameter *--max-old-space-size* is optional but often required with large APKs.


# B - I cannot install frida-node when i run *npm install*

If the error message is something like this :
```
...
```

Then, it can be caused by two issues :

## B.1 You have an incompatible frida-node module version in your cache.

## B.2 You have an incompatibility between your local NodeJS version and the frida-node dependency