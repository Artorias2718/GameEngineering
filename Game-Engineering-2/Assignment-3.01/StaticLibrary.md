---
---

# Static Library Example

- In addition to executable files we will also deal with "libraries" in this class. There are both "dynamic" and "static" libraries Dynamic libraries can be loaded at run-time, which means:
  - They can be changed without changing the executable file
  - As long as they fulfill an interface that the executable expects they can be used as "plug-ins" so that anyone can create code that an existing program will use
- In our class our experience with dynamic libraries will be limited to Maya plug-ins (Maya will load libraries that we create and execute our code). All other libraries that we create will be static.

## Static Libraries

- Static libraries are separate files that get embedded into the executable at build time. Functionally there is no real reason to use them the way we will as opposed to just putting the source files in a single project. The reason we will use them is for organization: It allows us to create separate projects to handle separate responsibilities. This means:
  - We will learn to make good design and architecture decisions by decoupling functionality where possible (as well as being forced to think about proper dependencies between different functionality)
  - We will only have to recompile functionality that we have changed
  - When we get error messages they will relate to the specific library functionality we have changed (or a dependent library functionality that we have broken by making a change)
- All of the above arguments could also be made in favor of using dynamic libraries instead and many games do exactly that.

## Creation

- Right-click the solution in Solution Explorer and choose Add->New Project...
- Choose "Win32 Console Application"
- In the Application Wizard window that appears select the "Application Settings" tab and change Application type: to Static library
- Now when you build this project it will create a LIB file instead of an EXE file. The rest of the required set-up is the same as the other projects in our class.
- The ProjectDefaults.props file sets the default output directory to $(BinDir) and the "Additional Library Directories" to $(BinDir) so that any time a static library is required as a dependency $(BinDir) is one of the places that will be looked for it. This means that if you set up your projects correctly with ProjectDefault.props you will never have to worry about Visual Studio looking in the wrong place (if this doesn't make sense read the rest of this page to see how the example solution does it).

## Using

- Look at EntryPoint.cpp in the Executable project. This consists of a standard main() function like you are used to, and the only thing that main() does is call a trivial example function. That function comes from the Static Library project (declared in ExampleLibrary.h and defined in ExampleLibrary.cpp). Notice that the C/C++ code doesn't care that this function comes from an external library! It is exactly the same as if ExampleLibrary.h and ExampleLibrary.cpp were part of the Executable project.
- A consequence of this is that if you don't set up a static library correctly you will not get compile errors (the solution will compile successfully) but instead you will get linker errors (compiling just guarantees that the syntax in calls to the static library are correct (as determined by the header file) and the linker will actually try to associate ("link") the calls to the static library with real compiled machine code and fail to find it). When this happens you will see "unresolved external symbol" errors (LNK2019Links to an external site.). When you see an "unresolved external symbol" error DON'T PANIC! Remember what it means: Your code is calling some externally-defined function correctly, but the linker can't find that externally-defined function and you need to help locate it.
- This is what you need to do to get Visual Studio to successfully use the static library that your project needs:
- Find the static library
  - Right-click the project that depends on a static library and choose "Properties"
    - (In the example solution this is the Executable project (because it depends on the StaticLibrary project). In general, though, remember that static libraries themselves can depend on other static libraries. Technically you only need to do this step for the executable itself and some sources you read online will actually recommend this as a best practice. I disagree, however, and in our class we will do this step for each project (whether executable or library) that uses code from another library.)
  - In the tree view to the left select Configuration Properties->Linker->Input
    - (If you're doing this for a library project instead of an executable project you will instead select Configuration Properties->Librarian->General)
  - Under Additional Dependencies add the name of the static library
    - In the example program this is StaticLibrary.lib
  - In general, you should also select Configuration Properties->Linker->General and add the location of the library to Additional Library Directories. When the static library is built as part of your own solution, however, the ProjectDefaults.props file will already add $(BinDir) and so you don't need to anything.
    - (If you're doing this for a library project instead of an executable project you will instead select Configuration Properties->Librarian->General to find the Additional Library Directories field.)
- Set the static library as a dependency
  - When a static library is built as part of your solution (as is the case in the example program) you must make sure that it has finished building before the linker actually tries to use it
    - In order to do this you must tell Visual Studio that the program that uses the static library depends on that static library
    - See the Solution Setup example under Project Build Order for how to do this
    - In the example program the Executable project depends on the StaticLibrary project
- When a static library comes from an external source (e.g.when using Direct3D or Maya) it will aready be built and this dependency step is unnecessary
- I would encourage you to try removing StaticLibrary.lb from the Additional Dependencies field in the example program and then rebuilding the solution to see errors that result (and see if you can read the errors and understand what they're telling you). Then add StaticLibrary.lib back and build again to see how it fixes the problem.