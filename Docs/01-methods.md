## Method 1: Scripts - Body & Coat
##### [GetCharExpression](https://rdr3natives.com/?native=0xFD1BA1EEF7985BB8) (*0xFD1BA1EEF7985BB8*) - Client
Gets MetaPedExpression at the index specified.
Params: `ped`, `index` (integer)
Returns: float
##### [SetCharExpression](https://rdr3natives.com/?native=0x5653AB26C82938CF) (*0x5653AB26C82938CF*) - Client
Sets MetaPedExpression at the specified index. It will morph the body.
Params: `ped`, `index` (integer), `value` (number from `-1.0` to `1.0`)
##### [SetMetaPedTag](https://rdr3natives.com/?native=0xBC6DF00D7A4A6819) (0xBC6DF00D7A4A6819) - Client
Applies metaped components. Replaces existing assets.
Params: `ped`, `drawable` (hash), `albedo` (hash), `normal` (hash), `material` (hash), `palette` (hash), `tint0` (integer), `tint1` (integer), `tint2` (integer)
##### [RemoveTagFromMetaPed](https://rdr3natives.com/?native=0xD710A5007C2AC539) (0xD710A5007C2AC539) - Client
Removes metaped components.
Params: `ped`, `component` (hash), `?` (integer)
I am not entirely sure what the `?` param represents
## Method 2: YMT Files - Body & Coat
#### 1. Editing existing YMTs
#### 2. Creating custom YMTs
I do not believe this is really worth going over at its current state. At least not until CFX updates and allows for streaming of additional file types. I have been waiting very patiently for [this pr](https://github.com/citizenfx/fivem/pull/3376) to be merged, as I suspect it will make more possible, from addon ymts to textures and more. Though, I am not 100% but one can dream.

Regardless, you can take a look at [the old files](Custom_Horse/Resources/OLD - Custom_Horse Repo/README) for more information on that one. This method has its own issues, from no reins to "spaghetti legs". This method can also be reviewed in the [creating custom horses forum thread](https://forum.cfx.re/t/how-to-add-a-new-custom-horse/5185418)
## My Method
The way I create custom horses and the way I would recommend you do it, mixes both of the above methods together into its own resource
- Use YMT files and utilize **outfits** to create new body appearances
- Use a script to set coats, height, stats and whatever additionals

I edit the following YMT files, personally: 
- `a_c_horse_winter02_01.ymt` - Is a horse that is not normally used, so easy to edit and not mess up other scripts. I am confident that you can delete all existing outfits in this one and start fresh.
- `a_c_horsemulepainted_01.ymt` - I only use this for horses that I want to bray [![Braying](https://img.youtube.com/vi/cJg2x2g0T9o/0.jpg)](https://www.youtube.com/watch?v=cJg2x2g0T9o)

