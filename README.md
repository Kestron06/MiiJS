# MiiJS
Read, Edit, Write, and make Special Miis from a Wiimote binary file or 3DS QR Code to a binary file or QR code


-# Note: The 3DS and Wii U do the same things with Miis. The Wii U does not however support Special Miis, and so 3DS is the name used for the 3DS/Wii U generation of Miis throughout this project. However, as long as you are not hoping to use Special Miis on the Wii U, as it cannot, the **3DS and Wii U are completely interchangeable** as far as Miis go otherwise.
-# Note: This package is for managing Wii, and 3DS/Wii U Miis.

## Installation
`npm i miijs` OR `npm install miijs`

## Making a Special Mii
To make a special Mii, read in the file using the appropriate function, set `mii.info.type="Special";`, and then write a new file with the appropriate function.
-# The Wii U does not support Special Miis.

# Functions
 - async read3DSQR(pathToQR), returns JSON
 - write3DSQR(miiJSON, path), writes QR
 - readWiiBin(pathToMii), returns JSON
 - writeWiiBin(miiJSON, path), writes new bin to the path specified
 - render3DSMiiFromJSON(miiJSON, path), writes PNG representation of Mii's face to the path specified
 - convertMii(miiJson, whatConsoleItIsForOriginallly ("3ds" or "wii")), converts the Mii JSON format
 - generateInstructions(miiJson, whatConsoleTheMiiIsFor, fullInstructions), returns a JSON object of different instruction fields. If full is not set, only the instructions that differ from a default Mii will be returned.

## Discrepancies in `convertMii` function
All of these discrepancies __only__ apply when converting from the **3DS to the Wii**, converting from the Wii to the 3DS should be a perfect conversion.
There is a reason that the Wii supports sending Miis to the 3DS, but not vice versa. Many of the fields on the 3DS are new, and not present on the Wii. This function does its absolute best to backport 3DS Miis, but it *is not perfect and never will be*. If you rely heavily on 3DS exclusive options in your Mii, the outputted Mii will likely not be satisfactory.

Here is a list of discrepancies this function attempts to handle.
 - The 3DS has four more face shapes, thus some are converted to the closest possible for the Wii.
 - The 3DS allows you to set Makeup and Wrinkles seperately, as well as having 7 more "makeup" (including beard shadow and freckles) types and 5 more wrinkle types. This is probably one of the messiest conversions since one field has to be ignored entirely if both are set. Since the 3DS has some that are not even close to anything the Wii has, it will ignore these if the other field is set, allowing for the other field to be added in its place, prioritizing wrinkles over makeup. The outputted Mii will almost certainly require further editing to be satisfactory if these fields are used.
 - The 3DS has 6 extra nose types, all on the second page - these are mapped to similar noses on the first page that the Wii has.
 - The 3DS has an extra page of mouth types containing 12 extra mouth types. These are mapped to similar mouths on the other two pages that the Wii supports.
 - The 3DS has two extra lip colors. These are changed into the default Orangey lip color if used since both of the extra colors are closest to this.
 - The Wii does not have the option to "squish" parts to be thinner. This function ignores this field as a result.
 - The 3DS has 60 extra hairstyles. These are mapped to hairstyles the Wii does have. This will not be a perfect conversion and has a decent chance of needing a manual change.
 - The 3DS has an extra page of eye types that the Wii does not, which the function maps to a similar eye type that the Wii does support if used. Will likely require a manual edit.
 - The 3DS has two extra mustaches and two extra beards. These are mapped to a similar beard or mustache if used - the two extra beards will likely need a manual change if used.
 
## Transferring Miis to and from the System
 - Wii
    - Method 1 (Recommended, doesn't require homebrew): Connect the Wiimote to your PC, Dolphin seems to be the easiest way to do so though there are some more difficult ways to do so, and use [WDMLMiiTransfer](https://sourceforge.net/projects/wdml/files/WDML%20-%20MiiTransfer/). Open the `readSlotX.bat` file for the slot you're trying to read from (Array notation, 0=1, 1=2, 2=3, and so on). The Mii will be in the same directory under the name `miiX.mii`, where X is the same number as the readSlot you opened. If you used `readSlotAll.bat`, then there will be 10 Miis (0-9) in the directory. Note that if no Mii was ever present in that slot ever on the Wiimote, it will still output a `miiX.mii` file, though it will not contain the Mii data correctly. To write to the Wiimote, make sure the Mii you're writing is in the same directory and named `miiX.mii`, where X is the slot you're writing to, and open `writeSlotX.bat`, where X is the slot you're writing to (in array notation).
    - Method 2 (Requires Homebrew, is untested by me): [Mii Installer](https://wiibrew.org/wiki/Mii_Installer) for writing from the SD card to the Wii, and [Mii Extractor](https://wiibrew.org/wiki/Mii_Extractor) for reading from the Wii.
 - 3DS and Wii U
    - Open Mii Maker, select "QR Code/Image Options", and then select the respective QR Code option, be it scanning a QR code or saving a Mii as a QR code.

-# If you are unable to transfer to the console you wish to, you can use the `generateInstructions` function provided here and manually recreate the Mii on the console using the provided instructions.

# Credits
 - [kazuki-4ys' MiiInfoEditorCTR](https://github.com/kazuki-4ys/kazuki-4ys.github.io/tree/master/web_apps/MiiInfoEditorCTR), I repurposed how to decrypt and reencrypt the QR codes from here, including repurposing the asmCrypto.js file in its entirety with very small modifications. I believe I also modified the code for rendering the Mii using Nintendo's Mii Studio from here as well, though I do not remember for certain.
