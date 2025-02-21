# CoDriven Advanced UI documentation

[Go back](index.md)

# How to install



CoDriven Advanced UI require some packages if you want to experiment with multiplayer game added as sample to StefanWyszynski\CoDrivenAdvancedUI\Examples\ folder.

If you don't want to use this examples then you can remove that folder and you won't have to install below packages

1. **Mirror**

If you want to use/analyze multiplayer game sample which is bundled with CoDriven Advanced UI, then You will need to install Mirror package from asset store.

Or you can add this to your project packages
"com.mirror-networking.mirror": "https://github.com/vis2k/Mirror.git#release",


2. **Animation Rigging (Optional)**

This is required if you want proper character animation in multiplayer sample.

you can install it from package manager or by adding to your procject packages
"com.unity.animation.rigging": "1.2.1"


3. **xNode (optional) - not required by game but by graph editor**

CoDriven Advanced UI has graph node editor build-in, but it is optional.

If you want to use it then you will have to unpack extension package located in CoDriven Advanced UI folder

then you will have to install base xNode external package (see https://github.com/Siccity/xNode) by using package manager or inserting this to your project package.json

"com.github.siccity.xnode": "https://github.com/siccity/xNode.git",

this is what I mean by graph editor:

![graph editor image](images/features/graph_editor.png)
