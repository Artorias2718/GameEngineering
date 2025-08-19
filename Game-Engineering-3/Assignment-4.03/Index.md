# Assignment 4.03: Debug Menu

## Requirements

- Add a "Debug Menu" to provide run-time data tweaking.
  - Since we are on PC, the activation toggle will be ~ (tilde).  The key right under escape on most keyboards. 
  - Once activated the Debug Menu should take the input focus away from the main game.
  - Navigate the Debug Menu via the arrow keys... up/down to select items, left/right to modify the current selection.
- *Optional:* In addition, you can also use the mouse if you prefer... this is YOUR tool.
- Provide the following functionality via your Debug Menu
  - Frame rate monitor (via 'Text' widget)
  - Enable/Disable a debug sphere (via 'Checkbox' widget)
  - Adjust Debug Sphere radius (via 'Slider' widget)
  - Reset Debug Sphere radius (via 'Button' widget)
- The Debug Menu must completely vanish from your code in a release build.
- Example code: DebugMenuExample.zipDownload DebugMenuExample.zip
- [Drawing a text with a font in DirectX:](http://www.drunkenhyena.com/cgi-bin/view_cpp_article.pl?chapter=3;article=17)