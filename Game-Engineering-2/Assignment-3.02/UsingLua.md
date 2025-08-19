# Using Lua

- In our class we will use Lua 5.2.3 (newer versions are available). Although the material in this example will give a quick introduction focused on what we will use for our class you are strongly encouraged to consult the official documentation throughout the class when you have a question. The reference manual can be found hereLinks to an external site..
- This example has a few non-standard features compared to the usual solution setup we use in this class:
  - I am copying the scripts that the example uses to $(BinDir), where the executables will be generated
  - To make things more convenient for you I have modified the .gitignore file so that the vcxproj.user settings are included. You must not do this in your own game solutions! For this example, though, all you should have to do is change the startup project in order to debug each of them.

## Solution Setup

- There is a new Lua project in External
- In our class we build Lua as a static library. This means that the entire Lua programming language can be embedded in any executable that we generate (our game, our asset build system, and any tools we create).
  - In general it's often a better idea to use Lua as a dynamic library (particularly on Windows if you want to allow your users to load arbitrary external Lua modules). For our class, however, we will keep it a simple static library.
- The source code for Lua is freely availableLinks to an external site.. I have made a few very minor changes, mostly dealing with the format of Lua's error messages so that they will show up in Visual Studio's "Error List" window and be double-clickable.
- I have put the source code in its own 5.2.3/ folder on disk, and in a "Source" folder in Solution Explorer. It's not obvious from within the IDE, but the actual vcxproj files use a special $(LuaVersion) environment variable in case we ever want to change to a different version of Lua.
- I have created a file called "Includes.h", which can be #included by any of our code that wants to use Lua. This makes it convenient for users (including you).
- I have also created a file called "License.txt", which contains the license that Lua asks be included with any code that uses it. This file will be copied to $(TargetLicenseDir), although its name will be changed to Lua.txt.
  - Right-click LIcense.txt (the file itself, not the project!), and choose Properties
  - Notice that in Configuration Properties->General I have changed Item Type to Custom Build Tool
  - Doing that (and clicking "Apply") makes a new section, Configuration Properties->Custom Build Tool appear
  - Take note of the Command Line, Description, and Outputs fields. These tell Visual Studio to always make sure that the file listed in Outputs will always be kept up-to-date with the file.
    - (This does the same thing that our asset build system will, but in a less-convenient and powerful way)
    - (This is also, incidentally, how this example copies the example Lua scripts to $(BinDir)
- Finally, you will notice a second project, "LuaExe". This builds a standalone Lua interpreter.
  - You can run this by double-clicking the generated EXE in Windows Explorer and then interactively type Lua to experiment
  - More useful in a real game build system, though, you can use this to run Lua scripts as part of the build process by passing in command line arguments
  - If you look at the project you will see that it only has a single C file. Everything else comes from the static library.
- The remainder of this webpage discusses each project and what you should learn from it. The information here is fairly brief; you really need to look at the code yourself, read the comments, and debug the programs and looks at the output in order to learn what you need to know. To be absolutely clear: My expectation is that you will step through each project line-by-line to gain an understanding of how things are working.
- In order to understand each project you should study them in the following order:

## StandaloneLuaScript

- This introduces how to use a Lua script inside of a C/C++ program. A Lua "script" is really just a file that has some executable code, but it is helpful to think of it as a "script" because it can be run using the standalone lua.exe interpreter.
  - standaloneScript.lua
    - This is the script that does all of the work in this example program
    - I have written extensive comments in the file, and it is highly recommended that you read through it all and execute the script to see the output.
      - You could run StandaloneLuaScript.exe and look at the output (make sure do run it from a command prompt so that the console window doesn't close)
      - You could run the StandaloneLuaScript from Visual Studio either with a breakpoint or by using DEBUG->Start Without Debugging so that the window will stay open
      - You could also run the standalone lua.exe interpreter and pass standaloneScript.lua as a command line argument
    - Please feel free to experiment with changing things in this file and running the script again to see what happens
  - EntryPoint.cpp
    - All this does is 1) create a new Lua state, 2) load and execute standaloneScript.lua, and 3) close the Lua state
    - The script should run without errors initially, but if an error does occur you will see code that talks about a "stack". This is a fundamental concept that you will need to understand when working with Lua from C/C++ code, and, unfortunately, it will probably be a bit confusing to you initially. I would encourage you to read this pageLinks to an external site. for an introduction, and it definitely wouldn't hurt to read some of the follow-up sections as well. (In fact, don't hesitate to refer to that entire online book as often as you wish. It was written by one of the creators of Lua and I think it is excellent. The free online edition covers an earlier version of Lua than we are using, but almost all of it is still relevant.)
    - A brief summary is that if there is an error either loading or executing the script then Lua will push a relevant error message to the stack. The example code will get this string and display it to stderr, and then pop the error message from the stack.

## LuaFunctionsFromC

- This example is an extension from the last, but the Lua script in this one no longer stands on its own. You could still execute it from the standalone lua.exe interpreter if you wanted to, but it wouldn't actually appear to do much. Whereas standaloneScript.lua did actual "tasks" (in the form of printing output), luaFunctionFromC.lua mostly defines functions, but doesn't actually call them. Once defined, however, these functions are part of the Lua state and can be called from C/C++ code, just like you could call a standard C/C++ function.
  - luaFunctionsFromC.lua
    - The very first thing that this file does is print() a statement
      - This will behave just like it did in standaloneScript.lua, and when luaFunctionFromC.lua is loaded and executed you will see this printed statement immediately
    - The rest of the file, however, just defines functions but doesn't call them. These functions are defined globally, and are available to the Lua state. After the file has been loaded and executed the output will show the single print statement discussed above, but otherwise it will appear as if nothing has happened. In order to see any results we will have to call the functions from C/C++ code.
    - Please take the time to read through the code and comments in this file and try to understand what each of the functions is doing. Feel free to change things, but be aware that the C/C++ code won't catch errors on most of these functions and so your program may unexpectedly terminate. (This is explained in the comments in EntryPoint.cpp, and once you understand it you are free to add lua_pcall() to the code if you wish.)
  - EntryPoint.cpp
    - This file starts exactly the same as the one in the previous example, but after loading and executing the luaFunctionsFromC.lua script it calls each of the example functions
    - Please take the time to read through each example and try to understand the comments about how things work. You are strongly encouraged to step through this in a debugger and watch the output.

## CFunctionsFromLua

- This example is where things get really exciting. The functions in this do almost exactly the same thing as those in LuaFunctionsFromC, but this time they will be written as standard C/C++ functions, and the Lua script will call them!
  - EntryPoint.cpp
    - For a C/C++ function to be callable from Lua it must be a lua_CFunction:
      - ```typedef int (*lua_CFunction)( lua_State *L );```
      - It must have a single input parameter, which is a Lua state
      - It must return the number of return values that it has pushed on the stack
    - You will see that each of the example functions defined in this file have the required function signature
    - After creating a new Lua state, the main() function will "register" each of these lua_CFunctions into the Lua state, where the string provided is the global name that the functions can be called by
    - After that, the main() function is simple: It loads and executes the CFunctionsFromLua.lua script and then closes the Lua state and exits.
    - Each lua_CFunction matches a corresponding Lua function from the previous example (in fact I tried to make them identical except for ExampleError() which had to be a bit different). Please take the time to read through each and understand the comments and how they work.
  - CFunctionsFromLua.lua
    - Just as EntryPoint.cpp in this example does the work of the previous Lua script, this Lua script does the work of the previous EntryPoint.cpp: All it does is call each of the functions that have been registered by EntryPoint.cpp
      - (Because of this, this Lua script can definitely not stand on its own. If you tried to run it in the standalone lua.exe interpreter there wouldn't be any syntax errors found (it is all valid Lua), but as soon as it tried to call the first function there would be a a run-time error because that function wouldn't have been registered and thus wouldn't exist.)
    - I believe that most of this code will be quite straightforward to follow, but please pay particular attention to ExampleStats(), which shows how to deal with multiple return values, and the two example error functions so that you will know how to deal with errors in Lua.
  - Note that from Lua's perspective a function is a function, regardless of where it comes from. This means that Lua functions can call C/C++ functions which could call other C/C++ functions which could call Lua functions which could call other Lua functions, which could then call C/C++ functions and so on.

## Tables

- A "table" is the only data structure that Lua has, but tables are extremely versatile. This project has several parts to it.

### tableExamples.lua

- This is a stand-alone Lua script with no associated C/C++ code; in order to see the output you should run it with the lua.exe interpreter in $(BinDir):
  - Build the Tables project
  - Open a command prompt in $(BinDir)
    - (You can do this in Windows Explorer by holding down shift, right-clicking in $(BinDir), and choosing "Open command window here")
  - Type the following:
    - lua tableExamples.lua
  - Compare the output with the source code in the script itself
- The script provides a very quick introduction to tables, but for our class you shouldn't need to know anything more advanced than the techniques shown. Nevertheless, I would encourage you to read the following sections on tables from the "Programming in Lua" book:
- [Section 2.5](http://www.lua.org/pil/2.5.html)
- [Section 3.6](http://www.lua.org/pil/3.6.html)
- [Section 3.6](http://www.lua.org/pil/11.1.html)
- They cover mostly the same information as my example script, but may be easier to follow.

### LoadTableFromFile

- Look at loadTableFromFile.lua
  - This gives an example of what every file in our class should look like that is used as an asset
    - In other words, not every Lua script file we use in our class will look like this, but every Lua file we use to represent an authored asset must
- In the C/C++ code three different methods are shown for loading the example Lua asset
  - All three are valid; please read and understand all three and then decide which one you like best

### ReadTopLevelTableValues

- Look at readTopLevelTableValues.lua
  - This gives simple examples of both an array and a dictionary (used in the same table)
- The C/C++ code demonstrates how to read each of these values
  - You will need to be able to do the same thing in order to read your authored asset files using Lua as a format
  - There are a lot of comments; Please step through the code line-by-line in a debugger while reading the comments to make sure that you understand how everything works.
  - This example tends to have a lot of error checking to show you how to do it. It is up to you how much you personally feel is the right amount.
    - Although I say that it is your choice, many of the assignments will require your build system to gracefully handle invalid input data (the TA will intentionally corrupt your asset files). In general my advice would be that more error checking is better than less.

### ReadNestedTableValues

- Look at readNestedTableValues.lua
  - This gives you a good example of what an asset file using Lua as a format can look like
  - Notice how human-readable it is (ignoring the comments, which were hand-authored by me won't be there in most assets). This is one of the big advantages of using Lua and why we do it in this class: It is very adaptable to any form you decide to personally use for your style, and since it has the robust Lua language powering it it is easy to make changes without having to write a custom parser yourself.
    - (To clarify: Lua is not necessarily the "correct" way to make human-readable assets. We use it in our class because it is easy to learn and we can then use it for many things. In a real game any format (including custom home-grown formats) are valid, as long as they follow the same principles that we will learn about.)
- The C/C++ code demonstrates how to look at each table and the values within it
  - The example code uses a technique of calling a new function whenever a new "level" of table is being processed. The advantages to doing this probably won't make sense until you try to worry about maintaining the Lua stack without doing so. If you find yourself having trouble figuring out how many values to pop in an actual assignment you may want to revisit this example.