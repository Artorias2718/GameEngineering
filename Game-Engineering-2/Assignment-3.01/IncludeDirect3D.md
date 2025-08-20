---
---

# Include Direct3D

- Direct3D is the graphics component of DirectX, functionality that Microsoft provides to give game programmers low level hardware access with a consistent interface (although Microsoft has changed this so that it is part of the core Windows API rather than separate for games). In our class we will use version 11 of Direct3D. (There is a newer version, Direct3D 12, but at the moment it is not being marketed as a replacement for 11 but rather as an alternative.)
- If you have Windows 8 or newer you no longer need to download the DirectX SDK in order to use Direct3D 11, but in our class I am going to still require it in case anyone is still using Windows 7 so that we can all be consistent. You will have to download the SDK from here:
  - https://www.microsoft.com/en-us/download/details.aspx?id=6812Links to an external site.

## Header Files

- Header files (those ending in .h) are used by the compiler to know whether your program has syntax errors (for example, whether the functions you are calling actually exist). When you install the DirectX SDK it has a number of header files that your code will need to #include, and since different people will install the SDK in different places we need a way to allow one way of writing #include statements in code that will work across different installations. In the example EntryPoint.cpp there is the following #include statement:

```cpp
#include <d3d11.h>
```

- The angle brackets tell the compiler that it should search for this in one of its known location for files. (In comparison, use quotes when #including a file using a relative path.) We need to tell Visual Studio where this location is so that it will be able to find the header file:
  - Right-click a project and choose "Properties"
  - In the tree view to the left select Configuration Properties->C/C++->General
  - There is a field called Additional Include Directories
  - To make all of the DirectX SDK header files available to be #included, add the following:
    - $(DXSDK_DIR)Include\
  - (When you install the DirectX SDK it will add the "DXSDK_DIR" environment variable to your machine)

## Libraries

- Access to header files are all that the compiler needs, but the "linker" will need the actual code that those header files promise exists. The Static Library example has more information. In order to make the DirectX libraries available to your solution the following steps are necessary:
  - Tell Visual Studio where the libraries are located
    - Right-click a project and choose "Properties"
    - In the tree view to the left select Configuration Properties->Linker->General
    - There is a field called Additional Library Directories
    - To make all of the DirectX SDK library files available to be linked, add the following:
      - $(DXSDK_DIR)Lib\x64\
    - (You would use \x86\ for 32-bit programs, but remember that in our class we are arbitrarily making Direct3D 64-bit and OpenGL 32-bit. In this example project I've actually set it up to support either 32 or 64 bit Direct3D so that you can see how it would be done, but in all of your code you should only support Direct3D for x64.)
- Tell Visual Studio which libraries to link
  - Just like we add #include statements in code for header files, we need to choose the specific libraries that each project needs
  - Right-click a project and choose "Properties"
  - In the tree view to the left select Configuration Properties->Linker->Input
  - There is a field called Additional Dependencies
  - Which libraries are necessary will depend on what functionality your programs use. In our class we will use d3d11.lib for core functionality and sometimes d3dx11.lib for "extra" functionality.
    - There is a debug version of the extended library, called d3dx11d.lib. Notice that the debug configurations have the trailing "d" but the release configurations don't.
  - The DirectX SDK documentation will tell you which header files and which libraries are required for specific functionality
- Help Visual Studio know that you want to build the DirectX SDK version rather than the built-in version
  - I think that this part is only required if you are using Windows 8 or later, but since I am now using Windows 10 you should all make sure to do it so that I don't get errors when I try to run your programs
  - Right-click a project and choose "Properties"
  - In the tree view to the left select Configuration Properties->General
  - There is a field called Platform Toolset
  - Change the default from v140 to v140_xp
  - Visual Studio will now not try to use the new built-in Windows Direct3D stuff. Note that you only need to do this for projects that include Direct3D. You will need to do it for your Graphics project, but you shouldn't do it for your other projects if you don't need to.
  - Additionally, _USING_V110_SDK71_ needs to be #defined. I have done this in the Direct3D.props file, so you should never need to worry about this in our class.

## Creating a Debug Device

- In a debug configuration a debug device is asked for by including the enumeration D3D11_CREATE_DEVICE_DEBUG, which will give additional output that can help track down problems. My understanding is that installing Visual Studio 2015 should give you everything you need for this to work, but if you get output messages saying that it is not working:
  - If you have Windows 10, go to the Settings panel, under System, Apps & features, Manage optional Features, Add a feature, and then look for “Graphics Tools”Links to an external site.
  - If you have Windows 8, download and install the Windows SDKLinks to an external site.