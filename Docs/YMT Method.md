If you are looking to create a horse via ymt, you can use the following [Blank horse YMT template](../Resources/Templates/a_c_horse_winter02_01.ymt.xml)
This is readable XML based on `a_c_horse_winter02_01.ymt` and has the minimum requirements for a horse body to spawn. It is not spawn-ready until the file is saved back to binary `.ymt` format with CodeX or equivalent.

You are going to be creating new outfits and modifying `explicitAssets` and `expressions` for each outfit.

Each option is explained below. Add each horse as a new `<Item>` under `<outfits>`. Outfit presets are zero-indexed: the first `<Item>` is outfit `0`, the second is outfit `1`, and so on.

| Option                     | Description (May be inaccurate)                                                                                                                                                                   |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`                     | Outfit name. It may be left blank.                                                                                                                                                                |
| `ethnicity`                | A shared MetaPed field primarily intended for human peds. Full horse outfits commonly use `caucasian`; preserve it for compatibility.                                                             |
| `slodDwd`                  | An SLOD-related drawable/reference used for very distant rendering. Observed horse files leave it empty.                                                                                          |
| `voiceGroup` _(outfit)_    | Optional default voice-group override for the outfit. Observed horse files leave it empty.                                                                                                        |
| `damagePack`               | Optional damage-appearance override. Observed horse files leave it empty.                                                                                                                         |
| `goreType` _(outfit)_      | Optional per-outfit gore override. Leave empty to inherit the top-level `default_horse` setting. Observed horse files leave it empty.                                                             |
| `scale`                    | Overall outfit scale. For this base model, `1.0` is approximately 64–65 inches at the withers. Expressions can alter the measured height slightly.                                                |
| `cutsceneScale`            | Optional scale override used in cutscenes. `0` appears to disable the separate override and use the regular scale. Most full horse outfits use `0`.                                               |
| `scaleRandomOffset`        | Allows the engine to vary the spawned scale around the configured `scale`. Use `0` for an exact, repeatable size. Observed horse files commonly use `0.015625` for slight variation.              |
| `coatShine`                | Primary coat-shine control. Observed horse outfits commonly use `1`; partial outfits commonly use `0` so they do not replace the existing coat setting. The exact shader calculation is unknown.  |
| `coatShineBase`            | Appears to provide the base value used by the coat-shine settings. Observed horse outfits use `0`.                                                                                                |
| `coatShineMin`             | Appears to provide the minimum coat-shine value or variation. Observed horse outfits use `0`.                                                                                                     |
| `coatShineMax`             | Appears to provide the maximum coat-shine value or variation. Observed horse outfits commonly use `0.2`, although the exact calculation still needs testing.                                      |
| `eyeRedness`               | Controls the amount of redness applied to the eyes. Observed horse outfits use `0`.                                                                                                               |
| `removeLimbBone`           | Identifies a bone that should be removed or hidden. Observed horse files use `ID_INVALID`.                                                                                                        |
| `fullOutfit`               | When `True`, the entry defines a complete outfit and replaces the current appearance. When `False`, it acts as a partial outfit, such as a saddle-only setup.                                     |
| `priorityLoad`             | Streaming hint that asks the game to prioritize loading the outfit’s assets. Observed horse files use both `True` and `False`.                                                                    |
| `preStream`                | Streaming hint that asks the game to load the outfit’s assets ahead of use. Observed horse files use both values.                                                                                 |
| `assets`                   | Tag-based component selection rules. The game resolves components using property, match, filter, and tint tags. Leave empty when defining every component explicitly.                             |
| `explicitAssets`           | Exact components that make up the horse, including each drawable, albedo, normal, material, palette, tints, and probability. This is the primary section used to construct a custom horse outfit. |
| `expressions`              | MetaPed expression ID and value pairs that morph the horse’s body. These define features such as muscle, neck thickness, head shape, legs, and hooves.                                            |
| `subOutfits`               | Additional outfit definitions or references used to compose an outfit. Observed horse files leave it empty.                                                                                       |
| `variations`               | Alternate asset configurations used by the outfit-variation system. Observed horse files leave it empty.                                                                                          |
| `tags`                     | Tags used to classify or select the outfit. These are separate from the component tags used under `assets`. They may remain empty.                                                                |
| `shopItems`                | Shop-item entries associated with the outfit, including components with wearable-state behavior. Observed horse files leave it empty.                                                             |
| `voiceGroup` _(top level)_ | Optional default voice-group override for the YMT. Observed horse files leave it empty. A neigh/bray override has not yet been verified in game.                                                  |
| `pulverizeVfx`             | Optional visual effect used when the ped is pulverized or destroyed. Observed horse files leave it empty.                                                                                         |
| `goreType` _(top level)_   | Selects the horse gore and damage configuration. Observed horse files use `default_horse`.                                                                                                        |
| `type`                     | Defines the MetaPed as an animal. Observed horse files use `MPT_ANIMAL`.                                                                                                                          |
| `version`                  | Internal MetaPed version/identifier. It is not the outfit number or outfit count.                                                                                                                 |
| `allowRandomExpressions`   | Allows the engine to introduce randomized MetaPed expression values. Use `False` when each breed should retain the exact body shape defined under `expressions`.                                  |
## Explicit Assets
Explicit assets, referred to as "components" within my script and related documents, make up the horse. If a component is defined in the ymt, it will appear in-game when the horse is spawned. This is also where you can define textures and tints.

Horse bodies usually consist of the following components, modified to your tastes.
Note that the components listed below have a bunch of varieties, be it drawables or albedos. 
I do not currently have a master list prepared, it is up to you to discover them.

| Component                  | Explicit Asset Sample                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Head<br>*Required*         | `<Item>`<br>     `<drawable>p_c_horse_01_head_000</drawable>`<br>     `<albedo>p_c_horse_01_head_000_c0_897_ab</albedo>`<br>     `<normal>p_c_horse_01_head_000_c0_000_nm</normal>`<br>     `<material>p_c_horse_01_head_000_c0_000_m</material>`<br>     `<palette>metaped_tint_horse</palette>`<br>     `<tint0 value="63" />`<br>     `<tint1 value="2" />`<br>     `<tint2 value="195" />`<br>     `<probability value="255" />`<br>`</Item>`      |
| Body<br>*Required*         | `<Item>`<br>     `<drawable>p_c_horse_01_hand_000</drawable>`<br>     `<albedo>p_c_horse_01_hand_000_c0_897_ab</albedo>`<br>     `<normal>p_c_horse_01_hand_000_c0_000_nm</normal>`<br>     `<material>p_c_horse_01_hand_000_c0_000_m</material>`<br>     `<palette>metaped_tint_horse</palette>`<br>     `<tint0 value="63" />`<br>     `<tint1 value="56" />`<br>     `<tint2 value="171" />`<br>     `<probability value="255" />`<br>`</Item>`     |
| Eyes<br>*Required*         | `<Item>`<br>     `<drawable>p_c_horse_01_teef_000</drawable>`<br>     `<albedo>p_c_horse_01_teef_000_c0_000_ab</albedo>`<br>     `<normal>p_c_horse_01_teef_000_c0_000_nm</normal>`<br>     `<material>p_c_horse_01_teef_000_c0_000_m</material>`<br>     `<palette />`<br>     `<tint0 value="0" />`<br>     `<tint1 value="0" />`<br>     `<tint2 value="0" />`<br>     `<probability value="255" />`<br>`</Item>`                                   |
| Tearline<br>*Recommended*  | `<Item>`<br>     `<drawable>p_c_horse_01_tas2_000</drawable>`<br>     `<albedo>p_c_horse_01_tas2_000_c0_000_ab</albedo>`<br>     `<normal>p_c_horse_01_tas2_000_c0_000_nm</normal>`<br>     `<material>p_c_horse_01_tas2_000_c0_000_m</material>`<br>     `<palette />`<br>     `<tint0 value="0" />`<br>     `<tint1 value="0" />`<br>     `<tint2 value="0" />`<br>     `<probability value="255" />`<br>`</Item>`                                   |
| Eyelashes<br>*Recommended* | `<Item>`<br>     `<drawable>p_c_horse_01_mis2_000</drawable>`<br>     `<albedo>p_c_horse_01_mis2_000_c0_000_ab</albedo>`<br>     `<normal>p_c_horse_01_mis2_000_c0_000_nm</normal>`<br>     `<material>p_c_horse_01_mis2_000_c0_000_m</material>`<br>     `<palette>metaped_tint_horse</palette>`<br>     `<tint0 value="62" />`<br>     `<tint1 value="62" />`<br>     `<tint2 value="62" />`<br>     `<probability value="255" />`<br>`</Item>`      |
| Mane<br>*Recommended*      | `<Item>`<br>     `<drawable>p_c_horse_01_mane_006</drawable>`<br>     `<albedo>p_c_horse_01_hair_000_c0_999_ab</albedo>`<br>     `<normal>p_c_horse_01_hair_000_c0_000_nm</normal>`<br>     `<material>p_c_horse_01_hair_000_c0_000_m</material>`<br>     `<palette>metaped_tint_horse</palette>`<br>     `<tint0 value="138" />`<br>     `<tint1 value="138" />`<br>     `<tint2 value="138" />`<br>     `<probability value="255" />`<br>`</Item>`   |
| Tail<br>*Recommended*      | `<Item>`<br>     `<drawable>p_c_horse_01_hair_001</drawable>`<br>     `<albedo>p_c_horse_01_hair_000_c0_999_ab</albedo>`<br>     `<normal>p_c_horse_01_hair_000_c0_000_nm</normal>`<br>     `<material>p_c_horse_01_hair_000_c0_000_m</material>`<br>     `<palette>metaped_tint_horse</palette>`<br>     `<tint0 value="138" />`<br>     `<tint1 value="138" />`<br>     `<tint2 value="138" />`<br>     `<probability value="255" />`<br>`</Item>`   |
| Feathers<br>*Optional*     | `<Item>`<br>     `<drawable>p_c_horse_01_feather_000</drawable>`<br>     `<albedo>p_c_horse_01_hair_000_c0_997_ab</albedo>`<br>     `<normal>p_c_horse_01_hair_000_c0_000_nm</normal>`<br>     `<material>p_c_horse_01_hair_000_c0_000_m</material>`<br>     `<palette>metaped_tint_horse</palette>`<br>     `<tint0 value="1" />`<br>     `<tint1 value="2" />`<br>     `<tint2 value="4" />`<br>     `<probability value="255" />`<br>`</Item>`      |
| Mustache<br>*Optional*     | `<Item>`<br>     `<drawable>p_c_horse_01_moustache_000</drawable>`<br>     `<albedo>p_c_horse_01_hair_000_c0_998_ab</albedo>`<br>     `<normal>p_c_horse_01_hair_000_c0_000_nm</normal>`<br>     `<material>p_c_horse_01_hair_000_c0_000_m</material>`<br>     `<palette>metaped_tint_horse</palette>`<br>     `<tint0 value="56" />`<br>     `<tint1 value="60" />`<br>     `<tint2 value="18" />`<br>     `<probability value="255" />`<br>`</Item>` |
## Expressions
Each horse base is essentially the same horse model with morphs. 
By modifying expression values, you morph the horse to fit your taste.

There is a native that can be used in scripts to alter these morphs. However, it has been observed that not all expression IDs reliably sync across clients and values set beyond `-1.0` and `1.0` do not reliably sync. Therefore, I recommend modifying them via ymt to get the most out of them.

An expression `id` identifies a deformation channel used by the horse's expression dictionary. It is not necessarily a single bone: one channel may reshape several related parts of the model. The accompanying `value` controls how strongly, and in which direction, that deformation is applied.

Observed YMT files themselves contain values outside `-1.0` to `1.0`.

The descriptions below are working names compiled from [T3CHMAN's MetaPed expression list](https://pastebin.com/Ld76cAn7) and compared against the expression IDs present in the exported stock horse YMT files. They are not official descriptions. Entries that still have no reliable identification are marked **Unknown** rather than guessed.
#### Horse morphs
The below expressions have been observed in horse files.

|      ID | Working description                                  |
| ------: | ---------------------------------------------------- |
| `10726` | Overall horse body size                              |
|  `3015` | Muscle size                                          |
| `18278` | Belly size or vertical position                      |
| `60649` | Belly horizontal position                            |
| `42991` | Height of the base of the neck                       |
| `26839` | Neck thickness                                       |
| `15833` | Shoulder height                                      |
| `41478` | Back or chest width                                  |
| `62347` | Hindquarter or butt size                             |
| `11904` | Rear-back or croup height                            |
| `36550` | Thigh size                                           |
|  `8420` | Front-leg size                                       |
| `16934` | Hind-leg size                                        |
| `60975` | Ankle or fetlock size                                |
| `39436` | Hoof size                                            |
| `48003` | Overall head size                                    |
|  `1589` | Under-jaw sag or depth                               |
| `62196` | Nose-bridge depth                                    |
|  `3054` | Muzzle or nose length                                |
| `55026` | Forehead height                                      |
| `23050` | Right-ear size                                       |
| `22538` | Left-ear size                                        |
| `22549` | Muzzle or nose size                                  |
| `29982` | Nose-bridge height                                   |
| `36120` | Right-nostril size                                   |
| `35608` | Left-nostril size                                    |
| `43213` | Horse head width                                     |
|  `2075` | Throat or jowl size                                  |
| `34850` | Right-eye size                                       |
| `17697` | Right-eye forward or backward position               |
| `17698` | Right-eye height                                     |
| `34338` | Left-eye size                                        |
| `17185` | Left-eye forward or backward position                |
| `17186` | Left-eye height                                      |
|  `9675` | Hoof length                                          |
| `33485` | Anterior trapezius or front neck-and-shoulder muscle |
|  `8147` | Muscle tone or vein definition                       |
| `57577` | Belly size                                           |
| `10002` | Neck height                                          |
| `63348` | Belly size; a separate region from `57577`           |
| `19812` | Left ear forward or backward position                |
| `19813` | Left ear horizontal position                         |
| `19780` | Right ear forward or backward position               |
| `19781` | Right ear horizontal position                        |
| `54287` | Tail angle                                           |
| `26933` | Knee and hock size                                   |
| `46240` | Chest height; described as female-only               |
|  `8991` | Butt or hip size; described as female-only           |
| `41611` | Horse gender morph: `0.0` male, `1.0` female         |
| `52553` | **Unknown**                                          |
|  `3437` | **Unknown**                                          |
|  `9584` | **Unknown**                                          |
| `16009` | **Unknown**                                          |
| `38169` | **Unknown**                                          |
|  `9586` | **Unknown**                                          |
| `43894` | **Unknown**                                          |
| `55710` | **Unknown**                                          |
| `55711` | **Unknown**                                          |
#### Shared body and facial morphs
These channels are part of the broader MetaPed expression system and are also used by human peds. Observed horse YMTs commonly include them, although some produce little or no visible change on particular horse components.

| ID | Working description |
| ---: | --- |
| `18046` | Shoulder-blade or back-muscle definition |
| `46032` | Limb size; called arm size on human peds |
| `27779` | Chest shape or size |
| `50460` | Waist width |
| `49787` | Hip width or stomach size |
| `64834` | Thigh size |
| `42067` | Calf or lower-leg size |
| `50039` | Shoulder size |
| `7010` | Shoulder thickness |
| `34006` | Head width |
| `41396` | Face width |
| `13059` | Brow height |
| `12281` | Brow width |
| `19153` | Brow depth |
| `49231` | Ear depth |
| `46798` | Ear angle |
| `10308` | Ear height |
| `60720` | Earlobe or ear-base shape |
| `27147` | Cheekbone height |
| `43983` | Cheekbone width |
| `13709` | Cheekbone depth |
| `36106` | Jaw height |
| `60334` | Jaw width |
| `7670` | Jaw depth |
| `15375` | Chin height |
| `50098` | Chin width |
| `58147` | Chin depth |
| `35627` | Eyelid height |
| `7019` | Eyelid width |
| `60996` | Eye depth |
| `53862` | Eye angle |
| `42318` | Distance between the eyes |
| `56827` | Eye height |
| `28287` | Nose width |
| `13425` | Overall nose size |
| `1013` | Nose height |
| `13489` | Nose angle |
| `61782` | Nose curvature |
| `22046` | Distance between the nostrils |
| `61541` | Mouth width |
| `43625` | Mouth depth |
| `31427` | Mouth horizontal position |
| `16653` | Mouth vertical position |
| `6656` | Upper-lip height |
| `37313` | Upper-lip width |
| `50037` | Upper-lip depth |
| `47949` | Lower-lip height |
| `45232` | Lower-lip width |
| `23830` | Lower-lip depth |
| `55182` | Jaw vertical position |
| `57350` | Left mouth-corner width |
| `40950` | Left mouth-corner depth |
| `46661` | Left mouth-corner height |
| `22344` | Left mouth-corner lip separation |
| `60292` | Right mouth-corner width |
| `49299` | Right mouth-corner depth |
| `55718` | Right mouth-corner height |
| `9423` | Right mouth-corner lip separation |
| `22421` | Right eyelid opening or closing |
| `52902` | Left eyelid opening or closing |
| `36277` | Neck width |
| `60890` | Neck depth |
Of these 120 known IDs, 117 appear somewhere in the exported horse YMT files examined. `46240`, `8991`, and `41611` do not appear in those observed YMT expressions.
