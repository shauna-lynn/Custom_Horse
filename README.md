
# Smokey Black Black Star Friesian Horse for RedM

** NOTE 10/25/2024 - It has been brought to my attention that the reason the spaghetti legs are gone is because I used CodeX to save the file as a binary - not because of the name in the file. I did not realise at the time that I was doing a step that wasn't initially explained! I am unsure if adding the name to the file itself has any sort of benefit or if it's just something silly that doesn't need to be there. **

If you were watching my stream – I swear it worked! I must have gotten lucky the first time I played with it. This is the RedM ported version of the [Smokey Black Black Star Friesian horse](https://www.nexusmods.com/reddeadredemption2/mods/2352) from Nexus Mods. Looking back on the VOD, the first attempt did not work as in the `fxmanifest.lua` we had written `files 'metapeds.ymt'` which is incorrect - it should have been `file 'metapeds.ymt'` or ```files { 'metapeds.ymt' }``` but for a VERY ROUGH video tutorial on the process you can view my [VOD on Twitch](https://www.twitch.tv/videos/2284448223) - just keep in mind it didn't work because of the fxmanifest. The process remains the same. I am positive the native skins did not work because of the naming conventions of the textures.

## Overview

When streaming an edited `.ymt` file based on the information provided in [How to Add a New Custom Horse](https://forum.cfx.re/t/how-to-add-a-new-custom-horse), the horse model works to an extent, but there are issues:

- **Missing Reins**: The horse spawns without reins.
- **Deformed Legs**: Coining the nickname "spaghetti legs."

Interestingly, I found that adding the model name into the `.ymt` file allows us to name the model whatever we want *and* it resolves the model deformations. However, fixing the reins seems to require adding the model to `reinsdefinition.ymt`, which I suspect uses the `REINS_DEFINITIONS_FILE` data loader in `fxmanifest`. Unfortunately, this feature is not supported by CFX at this time.

## MetaPedDefExplicitAsset Breakdown

### Components Overview

- **Drawable**: The model.
- **Albedo**: The base texture.
- **Normal**: The bump map (texture of the texture).
- **Material**: Defines surface properties.
- **Palette**: The color palette.
- **Tint 0-2**: Colors derived from the palette.
- **Probability**: Currently unknown.

### Common Drawable Components

| Drawable | Description            |
| -------- | ---------------------- |
| `teef`   | Horse eyes             |
| `feet`   | Horse hooves (believed)|
| `head`   | Horse head             |
| `hands`  | Horse body             |
| `mis2`   | Horse eyelashes        |
| `mane`   | Horse mane             |
| `hair`   | Horse tail             |
| `feather`| Horse feathers         |
| `mustache`| Mustache              |

You can add or remove drawables by editing the `.ymt` file directly.

## Known Horse-Related MetaPed Expressions

Below is a list of known MetaPed expressions affecting different horse body parts compiled by [information provided by T3CHMAN](https://pastebin.com/Ld76cAn7). The typical value range is from **0.1 to 1.0** for most expressions:

### General Body Structure
- **10726** - Body size
- **3015** - Muscle mass
- **8147** - Muscle tone / veins
- **57577** - Belly size
- **63348** - Belly size (duplicate)
- **41611** - Gender (0.0 = male, 1.0 = female)

### Head and Face
- **48003** - Head size
- **43213** - Head width
- **1589** - Under jaw sagging
- **62196** - Nose bridge depth
- **3054** - Nose length
- **22549** - Nose size
- **29982** - Nose bridge height
- **36120** - Right nostril size
- **35608** - Left nostril size

### Ears
- **23050** - Right ear size
- **22538** - Left ear size
- **19812** - Left ear forward/backward
- **19813** - Left ear X-position
- **19780** - Right ear forward/backward
- **19781** - Right ear X-position

### Eyes
- **34850** - Right eye size
- **17697** - Right eye forward/backward
- **17698** - Right eye height
- **34338** - Left eye size
- **17185** - Left eye forward/backward
- **17186** - Left eye height

### Neck and Throat
- **42991** - Base of neck height
- **26839** - Neck thickness
- **10002** - Neck height
- **2075** - Throat size

### Shoulders, Chest, and Back
- **15833** - Shoulder height
- **41478** - Back width / chest size

### Legs
- **8420** - Front legs
- **16934** - Hind legs
- **60975** - Ankles
- **26933** - Knee and hock size

### Hooves
- **39436** - Hoof size
- **9675** - Hoof length

### Rear and Tail
- **62347** - Butt size
- **11904** - Rear back height
- **54287** - Tail angle
