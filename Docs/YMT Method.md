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
An `explicitAssets` recipe selects registered assets; adding a name here does not itself register a new streamed asset. That guide explains asset preparation, client registry merging, streaming, texture compatibility and applying the complete appearance.

Explicit assets, referred to as "components" within my script and related documents, make up the horse. A recipe in the selected YMT outfit tells the game which registered drawable and textures to apply when the horse is spawned. The referenced assets must also be available to the client. This is also where you can define textures and tints.

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

### Names and verification

The descriptions below are readable **working labels**, originally compiled from [T3CHMAN's MetaPed expression list](https://pastebin.com/Ld76cAn7). They are not verified original Rockstar names for the individual numeric controls. Preserve the numeric ID when discussing or testing a morph; unresolved descriptions remain **Unknown**.

The proper asset names verified in both exported dictionaries include `pack:/p_c_horse01_customization.expr`, `pack:/p_c_horse01.expr`, `pack:/p_c_horse01_lod1.expr`, `pack:/p_c_horse01_lod2.expr`, `pack:/p_c_horsefat.expr` and `pack:/rigfatxml_horse.expr`. These name expression programs, not individual sliders. A program can read several inputs and affect several bones.

The runtime setter is `_SET_CHAR_EXPRESSION` (`0x5653AB26C82938CF`); the getter is `_GET_CHAR_EXPRESSION` (`0xFD1BA1EEF7985BB8`). Older references call them `_SET_PED_FACE_FEATURE` and `_GET_PED_FACE_FEATURE`; see the [RDR3 native database](https://github.com/alloc8or/rdr3-nativedb-data/blob/master/natives.json). The expression ID is not an index into the skeleton. For exact skeletal names and IDs, use [Verified horse bones](Horse%20Bones.md).

The table now distinguishes two checks, repeated on 2026-09-20:

- **YMT**: the ID occurs in an outfit's `expressions/Item/id` in the current 242-file horse/mule MetaPed export collection (147 distinct basenames across archive versions).
- **YED read**: an instruction whose type starts with `TrackGet` reads that ID on track `64` in **both** inspected horse dictionaries. This checks direct reads only; it does not establish the complete dependency graph, visible effect, value direction or multiplayer synchronization.

**Not found** means absent from this specific check, not proven unsupported. A shared facial label or a stored YMT value alone does not prove a visible horse morph. This is source verification; no new in-game or two-client tests were performed for this documentation update.

#### Horse morphs
Horse-oriented working labels and unresolved controls; use the evidence columns to distinguish observed entries from candidates.

| ID | Working description | YMT | YED read |
| ---: | --- | --- | --- |
| `10726` | Overall horse body size                              | Present | Read |
|  `3015` | Muscle size                                          | Present | Read |
| `18278` | Belly size or vertical position                      | Present | Read |
| `60649` | Belly horizontal position                            | Present | Read |
| `42991` | Height of the base of the neck                       | Present | Read |
| `26839` | Neck thickness                                       | Present | Read |
| `15833` | Shoulder height                                      | Present | Read |
| `41478` | Back or chest width                                  | Present | Read |
| `62347` | Hindquarter or butt size                             | Present | Read |
| `11904` | Rear-back or croup height                            | Present | Read |
| `36550` | Thigh size                                           | Present | Read |
|  `8420` | Front-leg size                                       | Present | Read |
| `16934` | Hind-leg size                                        | Present | Read |
| `60975` | Ankle or fetlock size                                | Present | Read |
| `39436` | Hoof size                                            | Present | Read |
| `48003` | Overall head size                                    | Present | Read |
|  `1589` | Under-jaw sag or depth                               | Present | Read |
| `62196` | Nose-bridge depth                                    | Present | Read |
|  `3054` | Muzzle or nose length                                | Present | Read |
| `55026` | Forehead height                                      | Present | Read |
| `23050` | Right-ear size                                       | Present | Read |
| `22538` | Left-ear size                                        | Present | Read |
| `22549` | Muzzle or nose size                                  | Present | Read |
| `29982` | Nose-bridge height                                   | Present | Read |
| `36120` | Right-nostril size                                   | Present | Read |
| `35608` | Left-nostril size                                    | Present | Read |
| `43213` | Horse head width                                     | Present | Read |
|  `2075` | Throat or jowl size                                  | Present | Read |
| `34850` | Right-eye size                                       | Present | Read |
| `17697` | Right-eye forward or backward position               | Present | Read |
| `17698` | Right-eye height                                     | Present | Read |
| `34338` | Left-eye size                                        | Present | Read |
| `17185` | Left-eye forward or backward position                | Present | Read |
| `17186` | Left-eye height                                      | Present | Read |
|  `9675` | Hoof length                                          | Present | Read |
| `33485` | Anterior trapezius or front neck-and-shoulder muscle | Present | Not found |
|  `8147` | Muscle tone or vein definition                       | Present | Read |
| `57577` | Belly size                                           | Present | Read |
| `10002` | Neck height                                          | Present | Read |
| `63348` | Belly size; a separate region from `57577`           | Present | Read |
| `19812` | Left ear forward or backward position                | Present | Read |
| `19813` | Left ear horizontal position                         | Present | Read |
| `19780` | Right ear forward or backward position               | Present | Read |
| `19781` | Right ear horizontal position                        | Present | Read |
| `54287` | Tail angle                                           | Present | Read |
| `26933` | Knee and hock size                                   | Present | Read |
| `46240` | Chest height; described as female-only               | Not found | Not found |
|  `8991` | Butt or hip size; described as female-only           | Not found | Not found |
| `41611` | Horse gender morph: `0.0` male, `1.0` female         | Not found | Read |
| `52553` | **Unknown**                                          | Present | Not found |
|  `3437` | **Unknown**                                          | Present | Not found |
|  `9584` | **Unknown**                                          | Present | Not found |
| `16009` | **Unknown**                                          | Present | Not found |
| `38169` | **Unknown**                                          | Present | Not found |
|  `9586` | **Unknown**                                          | Present | Not found |
| `43894` | **Unknown**                                          | Present | Not found |
| `55710` | **Unknown**                                          | Not found | Not found |
| `55711` | **Unknown**                                          | Not found | Not found |
#### Shared body and facial morphs
These channels are part of the broader MetaPed expression system and are also used by human peds. Observed horse YMTs commonly include them, although some produce little or no visible change on particular horse components.

| ID | Working description | YMT | YED read |
| ---: | --- | --- | --- |
| `18046` | Shoulder-blade or back-muscle definition | Not found | Not found |
| `46032` | Limb size; called arm size on human peds | Present | Not found |
| `27779` | Chest shape or size | Present | Not found |
| `50460` | Waist width | Present | Not found |
| `49787` | Hip width or stomach size | Present | Not found |
| `64834` | Thigh size | Present | Not found |
| `42067` | Calf or lower-leg size | Present | Not found |
| `50039` | Shoulder size | Present | Not found |
| `7010` | Shoulder thickness | Present | Not found |
| `34006` | Head width | Present | Not found |
| `41396` | Face width | Present | Not found |
| `13059` | Brow height | Present | Not found |
| `12281` | Brow width | Present | Not found |
| `19153` | Brow depth | Present | Not found |
| `49231` | Ear depth | Present | Not found |
| `46798` | Ear angle | Present | Not found |
| `10308` | Ear height | Present | Not found |
| `60720` | Earlobe or ear-base shape | Present | Not found |
| `27147` | Cheekbone height | Present | Not found |
| `43983` | Cheekbone width | Present | Not found |
| `13709` | Cheekbone depth | Present | Not found |
| `36106` | Jaw height | Present | Not found |
| `60334` | Jaw width | Present | Not found |
| `7670` | Jaw depth | Present | Not found |
| `15375` | Chin height | Present | Not found |
| `50098` | Chin width | Present | Not found |
| `58147` | Chin depth | Present | Not found |
| `35627` | Eyelid height | Present | Not found |
| `7019` | Eyelid width | Present | Not found |
| `60996` | Eye depth | Present | Not found |
| `53862` | Eye angle | Present | Not found |
| `42318` | Distance between the eyes | Present | Not found |
| `56827` | Eye height | Present | Not found |
| `28287` | Nose width | Present | Not found |
| `13425` | Overall nose size | Present | Not found |
| `1013` | Nose height | Present | Not found |
| `13489` | Nose angle | Present | Not found |
| `61782` | Nose curvature | Present | Not found |
| `22046` | Distance between the nostrils | Present | Not found |
| `61541` | Mouth width | Present | Not found |
| `43625` | Mouth depth | Present | Not found |
| `31427` | Mouth horizontal position | Present | Not found |
| `16653` | Mouth vertical position | Present | Not found |
| `6656` | Upper-lip height | Present | Not found |
| `37313` | Upper-lip width | Present | Not found |
| `50037` | Upper-lip depth | Present | Not found |
| `47949` | Lower-lip height | Present | Not found |
| `45232` | Lower-lip width | Present | Not found |
| `23830` | Lower-lip depth | Present | Not found |
| `55182` | Jaw vertical position | Present | Not found |
| `57350` | Left mouth-corner width | Present | Not found |
| `40950` | Left mouth-corner depth | Present | Not found |
| `46661` | Left mouth-corner height | Present | Not found |
| `22344` | Left mouth-corner lip separation | Present | Not found |
| `60292` | Right mouth-corner width | Present | Not found |
| `49299` | Right mouth-corner depth | Present | Not found |
| `55718` | Right mouth-corner height | Present | Not found |
| `9423` | Right mouth-corner lip separation | Present | Not found |
| `22421` | Right eyelid opening or closing | Present | Not found |
| `52902` | Left eyelid opening or closing | Present | Not found |
| `36277` | Neck width | Present | Not found |
| `60890` | Neck depth | Present | Not found |
### Evidence summary and limits

The current export collection contains **114 of the 120 listed IDs**. `8991`, `18046`, `41611`, `46240`, `55710` and `55711` were not found in this collection's outfit expression entries. The earlier 117-ID count came from a different export collection; it should not be treated as a universal count.

**46 of the listed IDs are directly read by both horse YED dictionaries.** In particular, `41611` is read even though it is absent from the inspected outfit entries. Conversely, `46240` and `8991` are absent from both checks, so their inherited female-only descriptions remain unverified candidates. The nine **Unknown** IDs remain unidentified; their presence in a file does not supply a reliable name or visible effect.

The current YMT snapshot contains values from `-2.0001` to `2.0`. These are observed values, not universal safe limits. A YMT value range is separate from the observed synchronization limits of runtime expression changes.

Inspected sources:

- Horse/mule `.ymt.pso.xml` exports under `CodeX Exports/horse meta/RDR2`; repeated archive versions are retained, and IDs are counted uniquely.
- `p_c_horse01.yed.xml` and `p_c_horse01.version2.yed.xml`: direct instruction reads, not every `BoneId` occurrence in the file. The latter filename is a local export label and does not establish its game build or archive precedence.

| Dictionary export | SHA-256 of inspected XML |
| --- | --- |
| `p_c_horse01.yed.xml` | `692e40452d65bfd472185610c640c1e702bef9c7539099f9223fc364ca326d6c` |
| `p_c_horse01.version2.yed.xml` | `47a66cd552af3f6d687770f5a80aad3d357b9eeaab9de87143025b2bec45c8ee` |
