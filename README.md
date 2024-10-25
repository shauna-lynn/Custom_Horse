If you were watching my stream - I swear it worked! I must have got lucky the first time I played with it. 
This is the RedM version of the Smokey Black Black Star Friesian horse from https://www.nexusmods.com/reddeadredemption2/mods/2352

When streaming an edited ymt file based solely on the information provided in https://forum.cfx.re/t/how-to-add-a-new-custom-horse it works to an extent - the reins are missing and the legs are deformed coining the "spaghetti names" nickname.
However, when I was playing with the files earlier - I noticed if I add the model name into the ymt, then we can name the model whatever we wanted AND it fixes the model deformalities. 

Unfortuantely, to fix the reins I believe we have to add the model to reinsdefinition.ymt which I suspect uses the REINS_DEFINITIONS_FILE data loader for the fxmanifest which is not supported by CFX at the time. 

Some information that you will find helpful when reviewing this, if you didn't see the stream:
<explicitAssets itemType="MetaPedDefExplicitAsset">          =          The different components that go on the horse, these consist of: 
drawable = the model
albedo = the texture
normal = the normal texture (the bump map, the texture of the texture)
material
palette = the colour palette
tint 0-2 = the colours from the palette
probability = i have no clue

What are each drawable?
teef = horse eyes
feet = horse hooves (I believe)
head = horse head
hands = horse body
mis2 = horse eyelashes
mane = horse mane
hair = horse tail
feather = horse feathers
mustache = mustache

You can add or remove the drawables by adding or removing them from the ymt. 

Known Horse-Related MetaPedExpressions
Format: ID - What it changes
I haven't tested the lowest possible for each of these, but the highest without issues should be around 1.0 (lets go with a range of 0.1 to 1.0)
General Body Structure
10726 - Horse body size
3015 - Horse muscles
8147 - Horse muscle tone / veins
57577 - Horse belly size
63348 - Horse belly size (duplicated value)
41611 - Horse gender (0.0 = male, 1.0 = female)

Head and Face
48003 - Horse head size
43213 - Horse head width
1589 - Horse under jaw sagging
62196 - Horse nose bridge depth
3054 - Horse nose length
22549 - Horse nose size
29982 - Horse nose bridge height
36120 - Horse right nostril size
35608 - Horse left nostril size

Ears
23050 - Horse right ear size
22538 - Horse left ear size
19812 - Horse left ear forward/backward
19813 - Horse left ear X-position
19780 - Horse right ear forward/backward
19781 - Horse right ear X-position

Eyes
34850 - Horse right eye size
17697 - Horse right eye forward/backward
17698 - Horse right eye height
34338 - Horse left eye size
17185 - Horse left eye forward/backward
17186 - Horse left eye height

Neck and Throat
42991 - Horse base of neck height
26839 - Horse neck thickness
10002 - Horse neck height
2075 - Horse throat size
Shoulders, Chest, and Back
15833 - Horse shoulder height
41478 - Horse back width / chest size/width

Legs
8420 - Horse front legs
16934 - Horse hind legs
60975 - Horse ankles
26933 - Horse knee and hock size

Hooves
39436 - Horse hooves
9675 - Horse hooves length

Rear and Tail
62347 - Horse butt size
11904 - Horse rear back height
54287 - Horse tail angle
