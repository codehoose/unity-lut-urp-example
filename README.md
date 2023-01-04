# Unity LUT URP Example
URP is a very different beast from the original (??) Unity renderer. I've been asked a few times on the (channel)[https://youtube.com/c/sloankelly] to do a video on it. I found a (good video)[https://www.youtube.com/watch?v=X3YNpxJpI4k] that covers the subject and it contained a link to further resources. Finding articles and videos on this subject is NOT easy.

Go check out Game Cottage's video because it's an absolute mine of information on the subject. And Sam's (excellent article)[https://samdriver.xyz/article/scriptable-render] really helped too.

My video on (YouTube)[https://youtu.be/6xm6UCFGDPE].

## What is a LUT?
Short for Look Up Table, it takes a value from 0..1 and maps it to a colour in a small image file. The file is x-pixels across by 1 pixel and the 0..1 value maps to 0..x-1 pixels.

## Getting the Value for Lookup
I used the Gameboy palette as the LUT for this example. It goes from dark to light green and will give the scene a late 1980s feel to it in it's monochromatic green glory. The value is determined using the NTSC luminosity calculation. It takes the red (R), green (G) and blue (B) values, multiplies them with a value and adds them together:

```
Y = (0.2126 * R) + (0.7152 * G) + (0.0722 * B)
```

This gives us a number between 0 and 1 inclusive. That number can then be used to map to the gameboy palette using the LUT image. 

## Shader Graph
I used shader graph for the first time in this project. It's similar to other shader editors I've used the past. I wrote one myself a long time ago when I was at another company. I take the input texture (the image that will be rendered to the screen) and the LUT texture as inputs. I use a variety of nodes to strip out the RGB values to calculate the Luminosity. Once that's done, I use the UV sampler on the LUT image to find the appropriate colour by sampling the image at X (where X is the Luminosity) and 0.5 on the Y. I could probably have left this at 0 but /shrug/.

## Putting it Together
I then used the code from the (article)[https://samdriver.xyz/article/scriptable-render] and changed the URP-HighFidelity-Renderer by adding a reference to the new render feature. I made sure to change the settings for the new feature to insert it _before_ rendering post process.

# Follow On
I really should look into how to do the circle wipe in URP now that I have these scripts -- thanks, Sam!