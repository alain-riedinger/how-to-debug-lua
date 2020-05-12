# How to debug lua scripts

Practical explanations on how to debug lua scripts, whether standalone or embedded, on Windows

The *magic* is done with following tools:
- The **mobdebug** lua module that implements a local / remote lua debugger: [pkulchenko/MobDebug: Remote debugger for Lua.](https://github.com/pkulchenko/MobDebug)
All you need is the script "mobdebug.lua"
- The **luasocket** module that is needed by the above debugging module: [diegonehab/luasocket: Network support for the Lua language](https://github.com/diegonehab/luasocket)
I compiled a version of this module for **Windows** and **lua 5.3**, as it was hard to get a decent and working set of binaries for this combination.
Current releases work for **lua 5.3.5**: [alain-riedinger/luasocket: A luasocket module that works with lua 5.3](https://github.com/alain-riedinger/luasocket)
- The **ZeroBraneStudio** lua IDE that hosts the debugger editor part itself: [ZeroBrane Studio - Lua IDE/editor/debugger for Windows, Mac OSX, and Linux](https://studio.zerobrane.com/)

## The ZeroBraneStudio editor

Quite simple:
- download the **Windows 32bit** zip file found here [Windows 32bit (zip archive)](https://download.zerobrane.com/ZeroBraneStudioEduPack-1.90-win32.zip)
- unzip it in a folder on your local Windows machine

Your lua debugger is ready to... debug lua scripts

## Debugging standalone lua scripts

Standalone lua scripts means for me some lua scripts that are run by the lua interpreter with the following way:
```
lua53.exe my-script.lua
```

To be able to debug your *my-script.lua* lua script, you need to:
- create a directory called *lua*
- add the lua binaries for the lua interpreter  
lua53.dll  
lua53.exe  
- add a subdirectory *modules* with the files needed for the *luasocket* module, compiled in the same model as the lua interpreter:  
mime\  
socket\  
ltn12.lua  
mime.lua  
socket.lua  
- for convenience, I add the lua scripts in the same directory than the lua binaries
my-script.lua
- copy the *mobdebug.lua* script under the same directory than the lua script to debug

In the lua script to debug *my-script.lua*, you need to add the following lines at the beginning of the script
```lua
-- Add to the LUA path the local sub-directory with libraries
package.path = package.path .. ';./?.lua;./modules/?.lua;./modules/?/?.lua'
package.cpath = package.cpath .. ';./modules/?.dll;./modules/?/?.dll'
require("mobdebug").start() -- same as start("localhost", 8172)
```

Start *ZeroBraneStudio* and open the lua script to debug
Launch the *debugger server* by doing: "Project", "Start Debugger Server"
You can also set some breakpoints in the lua script

Run the standalone lua script
```
lua53.exe my-script.lua
```

The debugger automatically starts the debugging session: you can now navigate in the script with all the debugging features !

## Debugging lua scripts in SciTE editor

The **SciTE** editor ([Scintilla and SciTE](https://www.scintilla.org/SciTE.html)) is a great editor, with a powerful lua interface that lets you develop lua scripts to enhance and adapt the editor to you very needs and way of work.
Those scripts can be complex, specially when it comes to lua coded lexers. This is when the need for a debugger comes clearly under the spot lights !
SciTE is compiled as a **x64** binary

I have my personal organization of the *SciTE* files, so the lua scripts are under à *lua* sub-directory:  
wscite\lua

- copy the *mobdebug.lua* script under the same directory than the lua script to debug
- copy the files of the *luasocket* module, compiled in **x64**, under the sub-directory:  
wscite\lua\modules  
mime\  
socket\  
ltn12.lua  
mime.lua  
socket.lua  

In the lua script to debug *my-script.lua*, you need to add the following lines at the beginning of the script
```lua
-- Add to the LUA path the local sub-directory with libraries
package.path = package.path .. ';.\\lua\\modules\\?.lua'
package.cpath = package.cpath .. ';.\\lua\\modules\\?.dll'
require("mobdebug").start() -- same as start("localhost", 8172)
```

If the lua script to debug is a function, linked to a command via SciTE *.properties* file facility, you must also include this code at the very beginning of the top lua function (the one that is configured in the .properties file)
```lua
require("mobdebug").start() -- same as start("localhost", 8172)
```

Start *ZeroBraneStudio* and open the lua script to debug
Launch the *debugger server* by doing: "Project", "Start Debugger Server"
You can also set some breakpoints in the lua script

Run SciTE and do what is needed to trigger the lua script, most common cases are:
- open a file of the type of the lua coded lexer
- launch the command that uses the lua script

The debugger automatically starts the debugging session: you can now navigate in the script with all the debugging features !

## Debugging lua scripts in TextAdept editor

The **TextAdept** editor ([Textadept](https://foicica.com/textadept/)) is a very powerful Notepad replacement. It is lightweight, it is fast and it is mainly lua based code. The need for a lua debugging facility is immediate.
Those scripts can be complex, specially when it comes to lua coded lexers. This is when the need for a debugger comes clearly under the spot lights !
SciTE is compiled as a **Win32** binary

TextAdept has its own strict organization of the files: all the non-core files are under:  
**%USERPROFILE%\.textadept**

- copy the *mobdebug.lua* script under the directory  
%USERPROFILE%\.textadept\modules
- copy the files of the *luasocket* module, compiled in **Win32**, under the sub-directory:  
%USERPROFILE%\.textadept\modules  
mime\  
socket\  
ltn12.lua  
mime.lua  
socket.lua  

In the lua script to debug *my-script.lua*, you need to add the following lines at the beginning of the script
```lua
-- Add to the LUA path the local sub-directory with libraries
package.path = package.path .. ';.\\modules\\?.lua'
package.cpath = package.cpath .. ';.\\modules\\?.dll'
require("mobdebug").start() -- same as start("localhost", 8172)
```

Start *ZeroBraneStudio* and open the lua script to debug
Launch the *debugger server* by doing: "Project", "Start Debugger Server"
You can also set some breakpoints in the lua script

Run TextAdept and do what is needed to trigger the lua script, most common cases are:
- open a file of the type of the lua coded lexer
- launch the command that uses the lua script

The debugger automatically starts the debugging session: you can now navigate in the script with all the debugging features !
