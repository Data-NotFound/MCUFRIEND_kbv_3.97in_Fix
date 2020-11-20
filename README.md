# Details
This fork corrects the display cropped issue on a 272x480 Display with a ILI9486/ILI9488 (ID of 0x9486/0x9488)  
(You can check your by uploading diagnose_TFT_support sketch).  

Normally 272x480 display won't be equipped with ILI9486/ILI9488.  
It's normally equipped with 320x480 display.  
But for some weird reason 272x480 display with ILI9486/ILI9488 is really common in stores near me and on a lot of sketchy online store. Especially the board that came from China.

First solution from the store that I brought from is to just manually change the elements on the screen by adding/subtracting position to avoid going into the cropped area.  
Obviously... It worked! but it's not practical and really annoying.  

The reason of this issue is it is detected as a 320X480 display but in reality it's only 272x480  
Identifying overflowed pixels is really simple  

320-272 = 48 pixels were overflowed out of the display.  
48/2 = 24 pixels overflowed each side of the display.  

So adding simple offset would be enough to fix this.  

# Usage
Simply add `tft.fixDisplay();` at your `setup()`
```cpp
#include <MCUFRIEND_kbv.h>
MCUFRIEND_kbv tft;

void setup()
{
  tft.fixDisplay();
}
```
Or if you only want to enable the offset alignment for only ILI9486/ILI9488 not both
Just pass the ID as the argument
```cpp
#include <MCUFRIEND_kbv.h>
MCUFRIEND_kbv tft;

void setup()
{
  uint16_t ID = tft.readID();
  tft.fixDisplay(ID); //0x9486 or 0x9488
  tft.begin(ID);
}
```

# Original MCUFRIEND_kbv README 
Library for Uno 2.4, 2.8, 3.5, 3.6, 3.95 inch mcufriend  Shields

1. The Arduino Library Manager should find and install MCUFRIEND_kbv library

2. Install the Adafruit_GFX library if not already in your User libraries.

3. Insert your Mcufriend style display shield into UNO.   Only 28-pin shields are supported.

4. Build any of the Examples from the File->Examples->Mcufriend_kbv menu.  e.g.

graphictest_kbv.ino: shows all the methods.

LCD_ID_readreg.ino:  diagnostic check to identify unsupported controllers.

MCUFRIEND_kbv inherits all the methods from 
the Adafruit_GFX class: https://learn.adafruit.com/adafruit-gfx-graphics-library/overview 
and Print class: https://www.arduino.cc/en/Serial/Print

The only "new" methods are hardware related: 
vertScroll(), readGRAM(), readPixel(), setAddrWindow(), pushColors(), readID(), begin()

readReg(), pushCommand() access the controller registers

The File layout changed with v2.9.3.   If replacing a pre-v2.9.3 library:
Please leave IDE.  Delete the existing MCUFRIEND_kbv folder.  Start the IDE.  Install from Library Manager.

HOW TO INSTALL AND USE: is now in "mcufriend_how_to.txt"

CHANGE HISTORY:         is now in "mcufriend_history.txt"
