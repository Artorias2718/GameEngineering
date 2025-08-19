# Assignment 1.04 - Part 2 - Understanding caching and working with bits

## Objective

- The focus of this assignment is to test your understanding of CPU cache addresses and manipulating bitfields.

## Primary Requirements

- Write a application that simulates a set associative cache:
  1. Create a data structure to simulate the tags for this type of cache.
  2. Prompts the user for a simulated set associative cache line width (bytes per cache line), sets in the cache and lines per set.
  3. Validate that the cache line width and number of sets are a multiple of 2.
  4. Prompts the user to either display the cache status (what lines are in use and their tag) or simulate caching a random memory address.
  5. When simulating a random memory address:
        1. Randomly generates an address.
        2. Determines the cache set and tag that the address would map to.
        3. Displays the memory block that that address would be included in. (The memory block is the block of bytes that map to a cache line that include that address).
        4. Simulate the cache lookup: Checks to see if that addresses memory block is in a line of the cache set it maps to. If not write store its tag for that cache set. Display the outcome to the user.
  6. Allow the user to quit. If not go to step 4.
      - Play with my [sample app &#128193;](https://code.eaemgs.utah.edu/svn/eaemgs-C06/jbarnes/dropbox/EAE6300/CacheSimulator.exe) if this is confusing

## Secondary Requirements

- Submit all files for building the application. This include:
  - Any necessary source files (.cpp, .h, etc.)
  - Visual Studio Solution and Project files (.sln, optional .suo, .vcproj, optional .vcprojfilters)
- Your application must compile without warnings.
- There should be no unnecessary files (.obj, .pdb, .exe, etc.) in your applications folder in your SVN trunk directory or tags directory.
- Must be submitted to your tags folder in a directory labeled "Assignment1.04.02"