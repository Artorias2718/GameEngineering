# Assignment 1.12 - Interview Questions

## Primary Requirements

- Pick three items from the interview question list below that you do not know the answer to.
- Implement a solution and unit test to validate your solution without looking up the answer on the internet.
- You may implement all three of them in one solution but put each solution in its own .cpp file.
- Use your best coding skills. Think about the solid coding skills we've discussed in class over the last few weeks.
- If the problem requires any data structures (linked list node, binary tree node, etc.) you must implement it yourself.
- None of these should require the use of the CRT or STL.
- Keep in mind that you may be given one question similar to these in an interview and given about 30 minutes to provide a solution.

## Secondary Requirements

- Ensure your test app exits without memory leaks.
- Follow the requirements / suggestions we talked about in class:
  - Think about the helpful rules on creating classes and inheritance we discussed in class.
  - Use header guardbands in all your header files.
  - Be const correct.
  - Pass by reference and return by value (where appropriate).
  - Use initializer lists in your constructors.
  - Properly inline smaller methods and operators (say less than 10 lines).
  - Try to write code readable by others.
- Submit all files for building the application. This include:
  - Any necessary source files (.cpp, .h, etc.)
  - Visual Studio Solution and Project files (.sln, optional .suo, .vcproj, optional .vcprojfilters)
- Your application must compile without warnings.
- There should be no unnecessary files (.obj, .pdb, .exe, etc.) in your applications folder in your SVN trunk directory or tags directory.
- Must be submitted to your tags folder in a directory labeled "Assignment1.12"

### Interview Questions:

- Detect if a linked list is circular.
- Reverse a linked list in place.
- Delete all the nodes in a linked list given a pointer to a function that returns true if the link data should be removed.
- **HARD:** You are given a linked list where the link nodes contain not only a next pointer but an additional pointer to another random node in the list. Write a function that creates a deep copy of this list.
- Reverse a string in place.
- Remove side-by-side duplicate characters in a string ("AAA BBB" becomes "A B") in place.
- Remove a given character from a string in place ( "AABBAABBC" - 'A' becomes "BBBBC").
- Split a sentence, given as const char *, into the individual words. Return an array of char *'s containing the words.
- **HARD:** Reverse the words in a string in place.
- Find the maximum depth of a binary tree.
- Implement the CRT strcmp(): int strcmp( const char * i_lhs, const char * i_rsh).
- Implement the CRT itoa(): const char * itoa( int i_int );
- Given an array of ints move all the 0's to the end while maintaining the order  
    of the non 0's in the list. Do this in place.