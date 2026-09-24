# Horse YED expressions: conversion and custom ear controls

Research recorded 24 September 2026. These findings concern the inspected horse dictionary and RedM on game build 1491. They are not a claim that every YED, game build, or CodeX release behaves the same way.

## What was tested

The base ped name is `p_c_horse_01`. Its tested expression dictionary is **`p_c_horse01.yed`**, without the underscore before `01`, extracted from `update_4.rpf/x64/anim/expressions.rpf`.

The original contains 39 expressions, 39 instruction streams and 1,029 jumps. Its decoded resource is 327,680 bytes. The original compressed file uses Oodle; our successful rebuilt files use raw DEFLATE inside an RSC8 container.

Replacing this existing dictionary through a resource's `stream` directory worked. The tested manifest did not register a special YED `data_file` mounter. This proves replacement of this existing filename, not registration of an arbitrary new expression dictionary.

| Asset | Evidence |
| --- | --- |
| Original stock dictionary | Horse visible and moving normally after character selection; user confirmed. |
| Unchanged XML round trip through the original local conversion path | Reproduced the crash, without needing custom ear instructions. |
| Corrected stock round trip | All instruction buffers and relevant metadata matched; user confirmed normal horse appearance and animation. |
| Custom ear-tip curl | User confirmed visible in-game deformation. |
| Curl plus ear stance | User confirmed both controls working and supplied an in-game screenshot. |
| Ear width plus upper-tip hook | Binary/static checks and Blender previews passed; in-game result has not yet been confirmed. |

The failing client reported `RDR2_b1491.exe+2914E99`, legacy crash hash `two-ceiling-hot`, after character selection. A crash signature alone cannot identify its cause; the unchanged-stock control isolated a conversion defect in this case.

## The jump calculation that fixed the round trip

There are three instruction buffers: vector/structured operands (Data1), scalar/other operands (Data2), and opcodes (Data3). A jump's opcode cursor has already advanced one instruction, and its Data2 cursor has consumed the 12-byte jump operand.

For an instruction at index `i` with relative instruction offset `r`:

```text
target index = i + 1 + r
Data1 displacement = target Data1 position - current Data1 position
Data2 displacement = target Data2 position - (current Data2 position + 12)
Data3 displacement = r
```

This interpretation matched all 1,029 jumps in the inspected original. The earlier conversion used the wrong target index and Data2 origin. Reject targets outside the instruction stream. Recalculate byte displacements when operand lengths or instruction positions change.

During the independent converter work, all 1,036 occurrences of opcode `0x70` in the decoded stock corpus also matched this same relative-displacement relationship. The independent tool preserves their targets through labels as well. This is an additional structural observation; the full runtime semantics of `0x70` remain unconfirmed.

The corrected XML path also preserved dictionary `Name`, `NameHash`, and each expression's `Unknown_80` field, which the earlier XML path omitted. `Unknown_80` remains an unknown field; it has not been established as a checksum.

Validation compared all three buffers of every stream, scalar expression metadata, tracks, springs and variables against the original. XML readback matched and a second rebuild was byte stable. A reader reopening its own writer's output is not sufficient: the broken writer also produced files its reader could open. Compressed file bytes and allocation layout need not match the original for decoded content to match.

## Custom controls

These are locally chosen experimental input IDs, not Rockstar-assigned or globally reserved IDs. An expression input ID is not a skeleton bone ID, even when an exported track field is called `BoneId`.

| Input | Control | Implementation and status |
| --- | --- | --- |
| 65000 | Ear-tip curl | Distributed rotational offsets on the second and third ear bones; 65 and 45 degrees per unit in this experiment. In-game deformation confirmed. |
| 65001 | Ear stance | Base-ear rotation, including parallel facial/analog helper paths; 30 degrees per unit. In-game deformation confirmed. |
| 65002 | Ear width | Adds `0.25 * input` to the local X scale contribution on the ear root/base targets, retaining existing scale terms. Runtime confirmation pending. |
| 65003 | Upper-tip hook | Tip-focused rotation, 70 degrees per unit on available third-ear/helper targets. Runtime confirmation pending. |

High and medium expressions (`rigfatxml_horse.expr` and `rigfatxml_horse_lod1.expr`) were changed. The lowest LOD remains stock, so shape can change with distance.

New inputs use Track 64 / Format 2 and an unused register slot. The experiment adds coefficients to existing expression calculations instead of replacing the entire ear animation. Unknown opcodes provisionally interpreted as register storage and affine blending remain inferred, not officially documented semantics. Zero new input retains the previous coefficients. Full interaction with all normal ear animations still needs observation.

No original mesh, UV coordinates, skin weights or skeleton were changed. A custom YED changes expression evaluation, not the underlying mesh topology.

## Sliders, persistence and synchronization

The creator stores these inputs in its existing `appearance.expressions` mapping and draft/export flow. Its ordinary slider range is initially `-2` to `2`; shared input validation accepts `-10` to `10`. These are application limits, not proven engine-safe or anatomically sensible ranges. The same signed value can produce very different motion depending on its coefficients.

The existing expression setter is `0x5653AB26C82938CF`, the getter used is `0xFD1BA1EEF7985BB8`, and the existing appearance refresh flow uses `0xCC8CA3E88256E58F`. A successful setter/readback alone does not establish visible deformation or replication.

**The YED does not supply a multiplayer synchronization mechanism.** Clients need the dictionary and the correct per-horse values applied through the horse system. Draft/export source checks passed; a second-client test and persistence across respawn/reconnect have not been established for these new controls. Local diagnostic commands do not persist or broadcast the changes.

## A repeatable test

1. Keep the original dictionary and record hashes for each candidate.
2. Test stock, unchanged rebuilt stock, then one custom control at a time.
3. Fully close RedM, restart the changed resource, and reconnect with a fresh client.
4. Confirm the expected streamed hash, select the character, and spawn the intended horse. Cache receipt proves delivery, not successful expression evaluation.
5. Check zero, positive and negative values; reset; observe idle ear animation and LOD changes.
6. Repeat with another client and after saving/respawning the horse before claiming synchronization or persistence.

Do not overwrite a streamed binary in place while the server cache may hard-link it. Stage a new file and replace the directory entry so old cached content remains consistent with its hash. Keep a rollback copy and avoid restoring unrelated resource/configuration changes.

## Converter ownership and scope

The converter used for the successful tests above was a command-line wrapper around a locally corrected CodeX reader/writer. It was **not** an independent conversion engine. CodeX remains third-party software; our wrapper and fixes do not establish ownership or redistribution rights over its source or assemblies. No CodeX source, DLLs, compression DLLs or game assets are included with this guide.

A separate [independent converter](https://github.com/shauna-lynn/rdr2-yed-converter) uses Python's standard library with no CodeX dependency. Its first experimental version accepts the inspected DEFLATE-compressed horse YEDs and exports its own preservation-oriented XML. It does not read native Oodle-compressed input or the older CodeX XML schema, and it does not construct arbitrary dictionaries from scratch.

Its four unchanged fixture round trips were byte-exact. Rebuilding the four custom controls from a stock base produced matching tracks, instruction buffers and checked metadata across all 39 expressions, including confirmation through a separate reader. That reader was used only as a development cross-check and is not included or required by the tool. Edited output from the independent writer still needs an in-game test; earlier runtime success does not automatically validate the new allocation/writer path.

## Reference hashes

SHA-256 values identify the exact local evidence; the binaries are not distributed here.

| Asset | SHA-256 |
| --- | --- |
| Original stock | `e4e7ac42e8a6da42e938cd6657d6c514ea9ee1c8ff9a9605b1944aee54cb09ee` |
| Failing unchanged rebuild | `ce7b26566fde5d70b65681c2334d94d3e69f51f92de2f80086e4cd7115c53653` |
| Corrected unchanged rebuild | `8689d2e87c73d4b4d7ee69881d6c5d40d903a4dacd405f7945b665b381cf8b1d` |
| Working curl and stance | `0f5cf8fb44ef2abddeed7fa4c93440ecd7cc2d5e7f37982c3e1884895c9da9cc` |
| Width and hook candidate, runtime pending | `0d2de8c65b10c3cd23c4858227f74b4709fc2b0bae9cb00e2b29414c4d0fa2a5` |
