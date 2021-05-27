# The-Drawing-Apprentice-Apprentice
***
Implementation of our version of Davis's Drawing Apprentice (Davis, N. Drawing apprentice: An enactive co-creative agent for artistic collaboration.2015)

This version uses an evolutionary algorithm instead of a Reinforcement Learning one.

## More information about the project
***
<https://documentcloud.adobe.com/link/review?uri=urn:aaid:scds:US:94f32264-95ee-4933-845d-bbdb40ac5905>

## Basic explanation
***
The only requirements to run this code is un up to date version of TouchDesigner. 

Press F1 when the file opens to start the Performance Mode

I will use this file so as comments in the code.
The operator RoundedPaintBrushExample2 holds the code, while "Controls" just holds reset and BrushSize sliders

Navigating inside the RoundedPaintBrushExample2, the main network is showed.
On the bottom of the network, the calculation to re-range and aspect correct the x,y value are done in CHOPS.
These CHOPS provide the values that would be used to draw multiple successive lines, rendered afterwards by the "render1" TOP
The generated drawing is added at this level if it has been chosen as definitive one. Then the TOP goes into a feedback
loop that will permanently draw the lines on the canvas

Above this, a "Record" component can be seen, which is in charge of timing the input and record it, doing some needed transformations.

The "Record" output its value to 4 operators called "Translate*" which are in charge of re-drawing what the used drew.
They are done in 4 different operators because the distortion is applied at this level as sum of noise to the input x,y values.

The ouput of these operators are then fed into the "Mod*", which is in charge of applying the scale/rotate/transform
operations as decided from the parameters sets.

The "Record" also sends values needed to do computation on the texture level. It sends information about the input drawing region
to the "Mod*" operators. Such informations are the edges of the region so maximum and minimum X and Y and center location X and Y.
 
Finally, the "Record" sends a boolean "done", to the "RandomTable" operator to tell to generate new random sets of parameters.

The DAT network on the up-right side of the "Record" operator is in charge of applying the Genetic Algorithm and keep the parameters sets.
The 2 "Random_Cross_Breed" arrays are the ones choosing which paramters cross-breed when it comes to the next generation.

Finally, the sets of parameters evolved are merged to a random generated one and can be seen in the "ValueTable", from which
the "Mod*" and "Translate*" take their parameters to apply the transformations. 

On the top-left side there are the buttons that generates new drawings.
