## Method 1: Scripts - Body & Coat
If you have an idea of how to script, then you should know how to use the natives below.
A high overview 
1. Spawn a base horse model.
2. Use **character expressions** to modify the physical appearance of the body.
3. Use **metaped tags** to adjust horse coat textures and colours.

| Context | Native                                     | Params                                                                                                                                                       | Description                                                                                                                                                     |
| ------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Client  | GetCharExpression<br>0xFD1BA1EEF7985BB8    | `ped`<br>`index` (int)                                                                                                                                       | Gets the metaped expression at the specified index.<br><br>Returns: float                                                                                       |
| Client  | SetCharExpression<br>0x5653AB26C82938CF    | `ped`<br>`index` (int)<br>`value` (num `-1.0` to `1.0`)                                                                                                      | Sets the metaped expression at the specified index, morphing the appearance of the body.                                                                        |
| Client  | SetMetaPedTag<br>0xBC6DF00D7A4A6819        | `ped`<br>`drawable` (hash)<br>`albedo` (hash)<br>`normal` (hash)<br>`material` (hash)<br>`palette` (hash)<br>`tint0` (int)<br>`tint1` (int)<br>`tint2` (int) | Applies metaped components, replacing existing assets. <br>Albedo determines the texture itself.<br>Palette and Tint 0-2 determines the colour of the textures. |
| Client  | RemoveTagFromMetaPed<br>0xD710A5007C2AC539 | `ped`<br>`component` (hash)<br>`?` (int)                                                                                                                     | Removes the specified metaped component.<br><br>? param is unknown                                                                                              |
## Method 2: YMT Files - Body & Coat
#### 1. Editing existing YMTs
#### 2. Creating custom YMTs
I do not believe this is really worth going over at its current state. At least not until CFX updates and allows for streaming of additional file types. I have been waiting very patiently for [this pr](https://github.com/citizenfx/fivem/pull/3376) to be merged, as I suspect it will make more possible, from addon ymts to textures and more. Though, I am not 100% but one can dream.

Regardless, you can take a look at [the old files](Custom_Horse/Resources/OLD - Custom_Horse Repo/README) for more information on that one. This method has its own issues, from no reins to "spaghetti legs". This method can also be reviewed in the [creating custom horses forum thread](https://forum.cfx.re/t/how-to-add-a-new-custom-horse/5185418)
## My Method
The way I create custom horses and the way I would recommend you do it, mixes both of the above methods together into its own resource
- Use YMT files and utilize **outfits** to create new body appearances
- Use a script to set coats, height, stats and whatever additional config for each.

I edit the following YMT files, personally: 
- `a_c_horse_winter02_01.ymt` - Is a horse that is not normally used, so easy to edit and not mess up other scripts. I am confident that you can delete all existing outfits in this one and start fresh.
- `a_c_horsemulepainted_01.ymt` - I only use this for horses that I want to [bray](https://www.youtube.com/shorts/cJg2x2g0T9o)
