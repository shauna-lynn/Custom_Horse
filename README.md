In summary, I intend for this repo to be a collection of my research into creating custom horses and horse systems. Somewhere along the line, I may sneak in some assets for you to use once I am comfortable with sharing them!

I do not consider myself an advanced developer by any means and I do use AI to assist me in my research and learning. What I share here is what I've come to understand of the systems and how they work for me within my own scripts I've developed over the last few years. 
## Scope & Prerequisites
- Some knowledge of scripting RedM in LUA. I will provide natives and how to use them as well as some code snippets for use. I will not be providing full scripts.
- Some knowledge of game files, how they work and navigation of them would be beneficial. A lot of this work is done by viewing and/or modifying these files.
- Some knowledge of ped modification.
## Tools & Resources
- [Ped Modification and MetaData Overview](https://forum.cfx.re/t/ped-modification-and-metadata-overview-outfits-assets-expressions-shop-items-overlays-etc/5124046) forum thread as an introduction to ped modification.
- [How To Add a New Custom Horse](https://forum.cfx.re/t/how-to-add-a-new-custom-horse/5185418) forum thread as an introduction to custom horses.
- [DexyFex's CodeX](https://www.patreon.com/dexyfex/join) for exploring and editing game files. Download links are in their Discord server once you get Patreon roles. At the time of writing, v30 is the most recent version but I've had more luck with v29 for most things I work with. I use this program for SO MUCH! From searching horse related files to creating game maps or creation of custom objects... it is so handy! [RDR2 Strings](Resources/Codex.Games.RDR2.strings.txt) is a file of strings for use in CodeX. It compiles strings from CodeX itself as well as other sources found online (OpenIV, etc.) merged into a single file for use. It does not look pretty. It is not sorted nice like CodeX files or otherwise. However, it does what it needs to do and exposes a fair number of horse-related strings.
- As a free alternative, [OpenIV](https://www.rdr2mods.com/downloads/rdr2/tools/45-openiv/) may work. I have not and would not use it myself, so I am not going to give guidance to that one.
- Lists of character expressions, used throughout the different methods defined below: [List by DISQUSE](https://pastebin.com/9jb88FXW) or [List by T3CHMAN](https://pastebin.com/Ld76cAn7) additionally, my own compiled list of horse-related ones.
## Methods to Create Horses
### Method 1: Scripting
Two natives stand out when we want to create horses via script: 
#### [SetMetaPedTag](https://rdr3natives.com/?native=0xBC6DF00D7A4A6819)
This native will change the components of a horse. Not just the model but also the textures and tints! I recommend using this to apply different components (such as manes, tails, feathers) and create new coats and colours.
#### [SetCharExpression](https://rdr3natives.com/?native=0x5653AB26C82938CF)
This native will morph the appearance of a spawned entities body. 
The issue I've come to find is synchronisation across clients. If you want everyone to see the same thing, you're out of luck. Some expressions do not sync across clients at all. Additionally, any values set outside of the `-1.0 to 1.0` range may not sync properly for the expressions that do happen to work.

If you are looking to modify the overall appearance of a horses body, I would not rely on a script to do it for you and would look into **method 2** or **my recommended hybrid method**.
### Method 2: YMT files
A YMT file is a metadata file. It stores structured definitions and configurations. They define the appearance of each entity, such as equipped components, expression morphs, scaling, etc. For horses, we'll be focusing on **outfits** and **explicit assets** within those files.
### Method 3: My recommended hybrid method
Simply put, I combine both method 1 and method 2 to create my custom horses within my scripts. 
#### Breed Bodies
In order to create new breeds and define per-breed body shapes, I choose a horse ymt file and then create an outfit for each breed I create. I keep a master list of each breed with outfit numbers and when I spawn them in-game, I simply set the ped outfit to the corresponding outfit number for that breed. 

My preferences are as follows:
- `a_c_horse_winter02_01.ymt` for my standard horses. I believe it to be a horse model that is not natively used within RedM, therefore, I am comfortable with removing the existing ped outfits for this and then defining an outfit for each breed setup I have. 
- `a_c_horsemulepainted_01.ymt` for any horses I want to [bray](https://www.youtube.com/shorts/cJg2x2g0T9o). I just simply add my breed setups as additional outfits, without removing the default ones.

By using only two ymt files as my bases, I am ensuring I do not overwrite any base game horses that may otherwise be used either by choice or other scripts. If you do choose to use another horse ymt, just ensure it is not used in the base game or by other scripts and you do not replace the default ped outfits and are simply adding to the file.
#### Custom Coats
I simply define my custom coats in a config file within my own scripts. Once a horse is spawned and the proper ped outfit is set for the breed, my script pulls the coat data and applies the requested coat to the spawned horse using metaped tags.
