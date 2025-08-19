# Assignment 1.04 - Part 1 - Working with pointers

## Objective

- The focus of this assignment is to test your abilities to work with C pointers. It's very similar to questions asked during programming interviews.

## Outline

- Grab my samples from [here &#128193;:](git@eae-git.eng.utah.edu:cohort7/samples.git).
- Look at the shell code in MakeSentence\main.cpp.
- Create a Visual Studio Solution with a console application project.
- Implement the function MakeSentence().
  - It should create and return a sentence from the passed (as const char *'s) array of words .
  - It should:
    - Determine the space needed to hold the sentence.
    - Allocate  **ONLY** the amount of memory needed.
    - Build the sentence into the allocated memory from the words (**adding spaces between the words and a period at the end**).
    - Return the string.
- Modify the sample app to query the user for the words in the sentence, instead of using a fixed set of strings. End the input when the user enters just ENTER.

## Requirements

- You can use malloc() to allocate the needed memory. You can not use anything else from C runtime library or STL.
- malloc() is the only external function you can call.
- You can not use any built in string functions (std::string, strlen(), strdup, etc).
- You should allocate exactly how much memory the built string will require, no extra.
- Your program should release all the memory it allocates.
- Submit all files for building the application. This include:
  - Any necessary source files (.cpp, .h, etc.)
  - Visual Studio Solution and Project files (.sln, optional .suo, .vcproj, optional .vcprojfilters)
- Your application must compile without warnings.
- There should be no unnecessary files (.obj, .pdb, .exe, etc.) in your applications folder in your SVN trunk directory or tags directory.
- Must be submitted to your GitLab group project as a branch labeled "Assignment1.04.01"