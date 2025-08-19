# Assignment 3.13: Render States & 2D Sprites

## Requirements

- Add OpenGL-specific code to allow changing the depth buffer before clearing it
- Add render states to your run-time effects
- Add render states to your human-readable and binary effect files
- Remove the OpenGL-specific code that enabled culling and depth buffering
  - (This should now be handled in a platform-independent way by your effects)
- Add the ability to draw 2D sprites
  - For this assignment you must have at least one sprite that uses alpha transparency
  - If you can think of a way to make a sprite that will be good for your actual game please do! Make sure to mention in your write-up if anything must be done while playing to make it visible.
  - If you can't think of a good way yet then you can just draw an arbitrary sprite on screen somewhere. You may use one of the provided eae6320-themed images, all of which have alpha transparency.
- Your write-up should:
  - Show us at least two screenshots of your game rendering a 2D sprite!
    - The first one should show your game at its correct aspect ratio
    - The second one should show your game rendering at a wider aspect ratio than the first (in other words, leave the height of the screen alone but increase the width noticeably in the settings.ini file)
    - If you didn't implement the advanced sprite functionality explain why the sprite appears different in the two screenshots (why is it affected by the screen's aspect ratio?)
    - If you did implement the advanced sprite functionality explain what you did to make the sprite appear the same in both screenshots (how did you get it to not be affected by the screen's aspect ratio?)
- Show us an example of a human-readable effect file with all three render states set
  - Explain why you chose to represent them the way that you did
- Show us the binary file of the same effect (a screenshot of it in a hex editor is ok as long as it's readable)
  - Explain how the three different render states are represented. Make it clear to your readers how it works and how the binary we see corresponds to what we saw in the human readable file. (You may want to write out actual bits for the render states rather than bytes and circle or highlight parts in different colors to make this more clear.)
  - Explain why you chose to put the render states where you did in the binary file, and why other choices would have not been as good.

## Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up
- If you enable the Draw Both Triangle Sides render state in an effect, build assets, and run the game you should be able to see the difference (if there's a 2D plane you should be able to move the camera under it and see the bottom, and if there's a standard 3D object you should be able to move the camera inside of it to see the inside)
  - (Consider adding screenshots to your write-up comparing the difference of one-sided and two-sided rendering)
- If you disable the Depth Buffering render state in an effect, build assets, and run the game you should be able to see the difference (things should draw on top of other things incorrectly)
  - (Consider adding a screenshot to your write-up showing depth buffering disabled and explaining why things look bad)

## Details

### Adding Render States to Effects

- Add the provided Download providedcRenderState code and the corresponding OpenGL extension files
  - Note that I have done the standard substitutions for [D3DDEVICE] and [IMMEDIATECONTEXT] for the Direct3D-specific files. By now you should know how to replace these with your individual scheme.
  - (There is no reason to not also add the cSprite code at the same time)
- Add a cRenderState to your effect struct/class/whatever
  - You should add an actual cRenderState, and not just a pointer to one
- When you set/bind your effect you should call cRenderState::Bind() (in addition to setting/binding the shaders)
- You must initialize the cRenderState at the same time that your effect is being loaded
  - The render state bits will eventually come from your binary file. If you do this part of the assignment before you have added render states to your files (which is what I recommend) then you will have to figure out how to set the render state bits manually before you can call cRenderState::Initialize().
  - Look at cRenderState.h to get an idea of how the "bits" work. You can either use the helper functions or set the bits yourself. (If you use the helper functions you should look at their implementations to see how they are setting the bits to make sure that you understand.)
  - You should be able to create a good set of bits and pass them to cRenderStateInitialize()
  - The bits that you can set as default values (which should match what your engine is currently doing for all effects) are:
    - Alpha Transparency: DISABLED
    - Depth Buffering: ENABLED
    - Draw Both Triangle Sides: DISABLED

## Adding Render States to Files

- First, add the ability to add any of the three render states to your human-readable file
  - These must all be optional
    - Any combination of any of the three should be possible. If any one isn't specified it's ok, because you will set a default value for it in the binary file..
  - The human-readable file format must not know or care that these are represented as bits in the engine. All that should be apparent is whether each state is enabled or disabled.
  - What is the most natural way in Lua to represent a series of optional keys that can be enabled or disabled?
- Next, add the render states to your binary file
  - The way that these are represented has already been decided for you by the cRenderState code that I have given to you. How must they be stored in your binary file?
  - You will have to extract each of the three values from your Lua file, and then figure out how to set them in the binary format. Setting them as a binary value is similar to how setting default values at run-time is described in the Adding Render States to Effects section.
  - Don't try to write each render state out individually to a binary file! There should only be one write operation for all render states. Can you figure out how to do this?
  - You will have three pieces of data in your binary file now (the render states, the vertex shader path, and the fragment shader path). Where in the binary file should you add the render states? There is a correct answer for this.
- Finally, extract the render states at run-time
  - Just like there should only be one function call to write all render states to a binary file there should only be one extraction operation necessary to get all of the render states from the file. Once you do this how do you use them to initialize the cRenderState?

## Adding Sprites

### Basic Sprite Requirements

- Create a new vertex shader for sprites
  - This is simpler than your current standard mesh vertex shader: Instead of transforming the vertex into different spaces you treat the input vertex as already being in screen space.
    - (This is actually exactly what we did up until Assignment 06)
  - I have provided an example shader. This is the exact shader I use in my reference implementation, but if you haven't done the optional tasks of making your shaders platform-independent you will have to either adapt it for both platforms or just use it as an example to modify your existing standard vertex shader.
- Create a new effect for sprites
  - It should use your new sprite vertex shader and your standard fragment shader for meshes
  - It should have the following render states set:
    - Alpha Transparency: ENABLED
    - Depth Buffering: DISABLED
    - Draw Both Triangle Sides: ENABLED
- Create at least one material for your required sprite
  - Choose a texture with alpha transparency. You may use one of the provided Download providedones if you wish (see the screenshots below for examples of how they should appear when used as sprites).
  - Make sure to add your material to AssetsToBuild.lua. (Everything else should build automatically if you have completed the previous assignments successfully.)
- Add the provided cSprite class
  - You will have to replace [D3DDEVICE] and [IMMEDIATECONTEXT] in the Direct3D code
  - The OpenGL code defines the same vertex format that you should use for your meshes. If you added the texture coordinates to your struct the same way I did (likely) then the provided code should work. If, however, your sVertex struct layout is different than mine then you will have to update the code. This should be easy to do, but make sure to look at the provided code to make sure that it is assuming the same sVertex layout that you actually have.
- Add a way for your game to submit sprite draw calls
  - Up until now, every draw call we have made has been for a 3D object using a mesh and a material. Sprites are handled differently, however: They have different information associated with them, and they must draw after the 3D scene. (They are 2D and can't use depth buffering, which is why you disabled that render state in your sprite effect.)
  - You will need the following capabilities:
    - A list of every sprite draw call that has been submitted for the current frame
      - (Remember that a list is actually a bad choice for a collection like this. What is a better container?)
  - A way for the game to submit a new sprite draw call that will be added to that list
  - In your RenderFrame() function, after you have drawn all mesh draw calls you must iterate through each submitted sprite draw call and:
    - Bind its material
    - Call cSprite::Draw()
    - (Note that we don't use draw call constant data for sprites, and so you don't need to update or set/bind it. This is to keep things simple in our class, but not necessarily the "right" way.)
  - Make sure to clear the list of submitted sprite draw calls after drawing them all for a given frame!
  - This part will be easier if you have made your RenderFrame() function platform-independent
  - Based on the requirements above there are two pieces of data needed for each sprite draw call to be made:
    - A cSprite
    - A material
- The following information is needed to create a cSprite (make sure to look at cSprite.h):
  - Screen Position:
    - Left, Right, Top, Bottom
    - This is in screen-space, like we used before Assignment 06. The range of coordinates are [-1,1].
- Texture Coordinates:
  - Left, Right, Top, Bottom
  - These use the OpenGL convention. The bottom of a texture is 0 and the top is 1.
  - For this assignment you should always use the default:
    - Left = 0
    - Right = 1
    - Top = 1
    - Bottom = 0
- To fulfill the basic requirements of this assignment, then, your game must create the data needed to create a cSprite (the appropriate screen coordinates depending on where you want it positioned and how much of the screen you want it to take up), create an appropriate material (which must use the sprite effect in order to work correctly) and then submit that data it to be drawn every frame.
- You may choose to only submit the sprite draw call on frames when certain conditions are met (like if the sprite says "YOU WON!" or if it's some kind of danger sign when an enemy is close), but to satisfy the requirement you can also just submit it every frame so that it is always visible.

## Optional Advanced Sprites

- There are two problems with the basic requirements shown above:
  - Textures can be different aspect ratios, and so if you don't want the sprite to be distorted you need to match its width and height to the width and height of the texture it's using
  - The screen can be different aspect ratios, and so if you don't want the sprite to be distorted you need to account for the screen aspect ratio and adjust a sprite's screen coordinates accordingly
- In a general sense, the best way to solve both problems is to create a separate mechanism for dealing with sprites that someone can use besides having to directly submit the data that the graphics system wants
  - In other words, the graphics system doesn't care what an artist or programmer's intent is; it just wants to know where it should draw the sprite.
  - What is needed, then, is an intermediate layer, some kind of UI system, that can take as input what an artist or programmer actually wants and then translate that into the data that the Graphics system wants
  - For example, as a gameplay programmer I may want to say "I want to draw a sprite using [this] material, I want to draw it [this] big, and I want it to be [in the top left corner]. Some UI system would then figure out the aspect ratio of the material's texture, correct the requested size for both the texture's aspect ratio and the screen aspect ratio, and figure out the actual screen coordinates based on the screen's aspect ratio.
- We are obviously not going to make a full-featured UI system for our class, but you can take these principles and figure out ways to make your life easier when wanting to draw sprites. Particularly, think about how to solve the two main problems:
  - Texture Dimensions:
    - I have provided Download providedoptional code that adds the ability to query a texture for its width and height
      - (This code just adds functionality to the files given last week, but does use the [D3DDEVICE] and [IMMEDIATECONTEXT] substitutions. If you look at the differences in git it shouldn't be too hard to merge these new files with your existing ones.)
    - Given that information, can you think of a convenient way for a gameplay programmer to specify a general size and have an automatic way of figuring out what the width and height should be? (There are many ways to do this, and no "right" answer.)
  - Screen Dimensions:
    - Consider the situation where you have a square screen aspect ratio (let's say 512x512) and a square sprite. If that renders correctly, what will the sprite look like if you change the screen resolution to 1024x512 but use the same screen coordinates?
    - Can you figure out a way, given the screen resolution, to calculate screen coordinates for sprites that also maintain their correct proportions? (Again, there are many ways to do this and no "right" answer.)
- Let me emphasize that all of this is optional! For our class it is enough to just use numbers that work and not worry about being clever. These problems can be fun to solve, though, and could make your life easier depending on how many 2D UI elements you have in your game.

### Examples

- Here is a screenshot from my reference implementation using three different sprites:
- assignment14_sprites.png
- And here is another screenshot where the only thing I've changed is the screen resolution and the sprites still have their correct aspect ratio:
- assignment14_sprites_wide.png