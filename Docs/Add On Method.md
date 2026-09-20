# Add-on drawables and textures

New horse drawables and albedo textures can be streamed from a RedM server without replacing stock assets or asking players to install files into RedM. This was demonstrated on RedM b1491 in September 2026.

**Registry refresh is required on every client.** Streaming the files and mounting their registries is not enough by itself. Server owners need to refresh each client's MetaPed registry before that client uses the custom assets, including late joins and reconnects. Restricting the refresh resource to the joining client has worked in development tests; the behavior is explained below, without implementation instructions.

| File | Purpose |
| --- | --- |
| Drawable (`.ydd`) | The model geometry and its internal drawable dictionary entries. |
| Texture dictionary (`.ytd`) | Texture images referenced by an appearance recipe. |
| Asset registry (`assets_*.ymt`) | Registers the asset GUIDs so the game can resolve those recipes. |

Putting a uniquely named file in `stream` does not automatically register a new MetaPed asset. Reusing a stock asset's identity makes a replacement; an add-on needs its own identity and registry entry.

## How to stream new assets

### 1. Prepare and stream the assets

Place the `.ydd` and `.ytd` files in the resource's `stream` folder. Use unique identities and check for collisions with stock assets and other resources.

For a new drawable, check the dictionary entry inside the `.ydd` as well as the filename. Renaming the file alone does not change that internal key. The tested Marwari head used a new internal drawable key matching its registered GUID; its geometry was unchanged.

### 2. Create additive registries

Use the structure of the corresponding stock registry as a reference, but include only your additional entries. The working drawable registry contained one new entry, rather than a complete copy of the stock registry.

- Register horse drawables in the `drawableAnimal` section of `assets_drawable.ymt`.
- Register horse albedo textures in the `albedoAnimal` section of `assets_albedo.ymt`.
- Match each `guid` to the intended asset identity. Preserve compatible category and match tags from the relevant stock component; update an albedo's self-referencing match tag to its new identity.
- Only reference an HD counterpart through `guidHd` when that asset is actually supplied.

The tests covered a new head drawable and additional eye albedos. They do not establish that every other registry type or arbitrary model/texture combination works.

### 3. Convert and verify with CodeX

Convert the edited registry XML to binary `.ymt` using **CodeX**, then reopen the saved binary in CodeX and verify its entries. Renaming an XML file to `.ymt` is not conversion. A successful binary round trip validates the file structure, not its in-game behavior.

### 4. Mount the registries from the resource

Keep the binary registries beside `fxmanifest.lua`, with the drawable and texture files under `stream`. Add these declarations to the resource's RedM manifest:

```lua
files {
    'assets_drawable.ymt',
    'assets_albedo.ymt',
}

data_file 'METAPED_ASSETS' 'assets_drawable.ymt'
data_file 'METAPED_ASSETS' 'assets_albedo.ymt'
```

Declare only the registries you actually supply. This example is the asset-mounting portion of a manifest, not a complete initialization system.

Earlier experiments crashed when mounting registries and led to advice to install client-side overrides. Later tests demonstrated additive registries working through `METAPED_ASSETS` with client registry initialization completed. Manual installation into `RedM.app/citizen/platform/packs/base/data/metapeds` is therefore not required for the demonstrated server-streamed method. This finding does not establish `PED_METADATA_FILE` as an alternative asset-registry mounter.

### 5. Refresh the registry on every client

The registry is local to each player's game. A refresh on the server or on the horse owner's client does not initialize another player's game. Starting the resource at server boot is not proof that a later joining client has refreshed its registry.

Find a way to complete the refresh on **every client**, including late joins and reconnects, before loading/spawning peds that depend on it. Coordinate it with the spawn flow and verify that a new join does not disturb existing players. Refreshing a horse's appearance after applying components is a separate operation; it does not replace registry initialization.

The demonstrated approach restricts which clients receive a small registry-refresh resource:

1. The joining client loads the custom assets and reaches the initialization stage.
2. Only that ready client receives the refresh resource. Existing players and clients that are not ready are excluded from that refresh cycle.
3. The server cycles the resource, but only the selected client loads and unloads it. Existing players do not repeat their registry refresh.
4. Once initialization completes, the joining client is allowed to spawn. Each later connection goes through its own initialization.

The restriction applies to delivery of the refresh resource, not to everyone else's gameplay or to the custom assets themselves. The server resource lifecycle is still global; controlling which client receives the resource makes its effect specific to that client. The Python proxy used to enforce this restriction is described below. Server owners still need to supply and validate that part of their setup.

### How the Python proxy controls delivery

The development setup uses a small Python HTTP/HTTPS proxy between the clients and FXServer. It forwards connection and resource-download requests, but filters the resource list returned to each client. Normal gameplay traffic still goes directly to FXServer over UDP.

The Python program does not modify the custom drawable/texture files, patch the game, or perform the registry rebuild itself. It controls whether a particular connection is offered the refresh helper:

- Initially, it hides the helper from the joining client's resource list while allowing the normal custom assets to load.
- A server-side controller queues ready clients and authorizes one connection at a time. The proxy then allows that connection to receive the helper when the server starts it.
- After the selected client acknowledges loading the helper, the controller stops it. Unloading the helper's empty MetaPed registry triggers the engine's registry finalization while the actual custom registries remain mounted.
- Other clients do not receive the helper during that cycle, so already initialized players do not repeat the rebuild.
- The proxy records which connection tokens have been offered the helper in a small SQLite database. This survives proxy restarts and prevents the same connection from being offered it again. A fresh connection must be initialized separately.

An offer is not proof that initialization completed: the controller's acknowledgement and the client's helper-stop event provide the readiness checks. An interrupted first delivery requires a fresh connection in this prototype. The spawn flow waits for the client's initialization to finish.

The server still starts and stops the resource globally. Filtering its delivery makes the load/unload effect specific to the selected client. The Python script, server controller and setup instructions are not supplied in this repository; this is a description of the tested approach.

### 6. Apply and verify the appearance

Use the registered GUIDs in a horse YMT's `explicitAssets` or apply them through your appearance script. Start from a compatible, known-working recipe and preserve the drawable, albedo, normal, material, palette and tint values you are not changing. Apply the base outfit first so it does not overwrite your custom components afterward.

Check that the asset request succeeds, that the expected GUID is present on the rendered ped, and that the result looks correct. Repeat the checks from another client viewing the same networked horse, including a client that joins after the horse already exists.

## What has been demonstrated

- A genuinely additional Marwari head drawable streamed and rendered on RedM b1491. The inward-curving ears were visually confirmed, and existing stock drawable entries remained available.
- An additive registry of 41 custom eye albedos passed CodeX round-trip and texture loading checks. A selected custom eye albedo was subsequently confirmed by rendered-ped readback in multiplayer testing; this is not visual approval of all 41 colours.
- Two local clients read the same custom head GUID (`C647E149`) and selected eye albedo GUID (`BC0833FC`) on the same networked horse, including a late join and a rejoin from a fresh game process after registry initialization.

These are development results, not a production-stability guarantee. Remote PCs, larger groups of players and long sessions still need validation. Fresh-process rejoining also does not prove that disconnect/reconnect within the same game process is reliable.

## Potential beyond horses

The restriction and refresh operate on the MetaPed registry, not on a horse-specific streaming system. In principle, the same approach could support additional drawables and textures for other animals or human MetaPeds, including clothing, when their assets and registry entries are compatible.

That broader use is a hypothesis, not a tested result here. Each asset category needs its own registration, appearance and multiplayer checks. This is not a general solution for every streamed asset type: maps, props and other systems can have different mounters and initialization requirements.

For outfit and `explicitAssets` details, see [YMT Method](YMT%20Method.md).
