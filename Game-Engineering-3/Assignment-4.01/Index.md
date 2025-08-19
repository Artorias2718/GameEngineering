# Assignment 4.01: Load Render Data

## Requirements

- Using your Maya tools and code from Game Engineering II, load ctf_v14.mb Download ctf_v14.mband export the renderable data within in it into your engine's data format.
  - All textures must also be loaded and applied correctly.
    - Textures can be found in the "ctf_textures" subdirectory.
    - The color maps come in both DDS and PNG format. You may ignore the normal map and specular map if you wish.
  - The file includes collision data that should be ignored (for now).
  - You may modify this file to suit your engine better if you wish. (combining meshes perhaps.)
- Provide a "Fly Cam" to navigate around this loaded level.
- [Sample code to extract materials and textures (among other things) from Maya](https://nccastaff.bournemouth.ac.uk/jmacey/RobTheBloke/www/research/maya/mfnmesh.htm)