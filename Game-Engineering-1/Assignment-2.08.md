# Assignment 2.08 - Vector4 / Matrix classes

## Primary Requirements

- Add a Vector4 class, similar to your Vector3 class to your engine.
- Add a 4x4 Matrix class to your engine.
  - It should support creating rotation, scaling and translation transforms
  - It should support matrix by matrix multiply.
  - It should support transpose and inversion.
  - It should support multiplying of Vector4 class, whether treated as row or column matrices.
- Write a unit test to test your Matrix and Vector4 classes.

## Secondary Requirements

- Ensure your game exits without memory or DirectX Object leaks.
- Follow the requirements / suggestions we talked about in class:
  - Think about the helpful rules on creating classes and inheritance we discussed in class.
  - Use header guardbands in all your header files.
  - Be const correct.
  - Pass by reference and return by value (where appropriate).
  - Use initializer lists in your constructors.
  - Properly inline smaller methods and operators (say less than 10 lines).Try to write code readable by others.
- Submit all files for building the application. This include:
  - Any necessary source files (.cpp, .h, etc.)
  - Visual Studio Solution and Project files (.sln, optional .suo, .vcproj, optional .vcprojfilters)
- Your application must compile without warnings.
- There should be no unnecessary files (.obj, .pdb, .exe, etc.) in your applications folder in your SVN trunk directory or tags directory.
- Must be submitted to your tags folder in a directory labeled "Assignment2.08"