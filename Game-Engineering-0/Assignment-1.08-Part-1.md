---
---

# Assignment 1.08 - Part 1 - Block Based Heap Allocator

## Primary Requirements

- Start writing your block based memory allocator by implementing construction / initialization and a working allocation function.
- You do not need to implement your own new() / delete() yet. Instead include and satisfy a unit test function similar to the one at the bottom of this text.

## Secondary Requirements

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
- Must be submitted to your tags folder in a directory labeled "Assignment1.08"

UnitTest:

```cpp

  void Alloc_UnitTest()  
  {  
    // allocate some memory for this heap  
    const size_t sizeHeap = 1024 * 1024;  
    void * pHeapMemory = malloc( sizeHeap );
    assert( pHeapMemory );
  
    // create a HeapManager instance  
    HeapManager * pHeap = HeapManager::Create( pHeapMemory, sizeHeap );  
    assert( pHeap );
  
    unsigned int totalAllocations = 0;  
    long sizeAllocations = 0;
  
    void * pMemory = 0;
  
    do  
    {  
      size_t sizeAlloc = static_cast<size_t>( rand() );
  
      pMemory = pHeap->alloc( sizeAlloc );
  
      if( pMemory )  
      {  
        sizeAllocations += sizeAlloc;  
        totalAllocations++;  
      }  
    }
    while( pMemory != NULL );
  
    DEBUG_PRINT( "Made %d allocations totaling %l from heap of %d",  
    totalAllocations, sizeAllocations, sizeHeap );  
  }

```