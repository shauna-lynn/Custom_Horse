It is possible to add on drawables, textures, etc. rather than streaming replacements. 
It is done by registering the new assets in their respective assets_* files, supplying the new assets via script and then applying those new assets with the horses appearance.

| File                            | Purpose                                                                                  |
| ------------------------------- | ---------------------------------------------------------------------------------------- |
| Drawable (`.ydd`)               | The model geometry.                                                                      |
| Texture Dictionary (`.ytd`)     | A dictionary of texture images that can be referenced. It may contain multiple textures. |
| Asset Registry (`assets_*.ymt`) | Registers the asset GUIDs so recipes can use them.                                       |
Adding a new drawable or texture dictionary to your scripts stream folder does not automatically register the asset unless it is named after an already existing asset of the same type, which it would then replace. To integrate these assets as an add on, you need to register them in their respective `assets_*.ymt` file. 

The instruction from here forward assumes you've already designed your desired asset and are ready to add it to your resource.

## How to add on new assets
### Stream the asset files. 
Put your desired assets into your resources `stream` folder.
### Register the assets. 
Locate the registry files corresponding with your asset (examples: `assets_drawable.ymt` for drawables, `assets_albedo.ymt` for textures)
Add your asset as an entry in the appropriate section. Set the `guid` to match your asset. Check the category and match tags against an existing component of its type. For albedos, update its self-referencing `matchTags` entry. `guidHd` is a high-detail identifier used only when a `+hi` file exists.
Ensure no guid hash is a duplicate.
### Save binary registries. 
Convert your updated `assets_*.ymt` file using Codex or equivalent, back into its binary form. 
### Install the client registries. 
Ensuring RedM is closed, find its installation folder and create the following path should it not exist:
`RedM.app/citizen/platform/packs/base/data/metapeds` place your converted binary files there.
`%LOCALAPPDATA%\RedM` is an easy way to get access to this folder.

==**Every player that connects to your server will need to complete this step!**== 
Attempting to stream these files using `METAPED_ASSETS` / `PED_METADATA_FILE` mounters crashed in my testing.
### Apply the assets.
Use the ymt or script method to apply your new assets to your desired horse.