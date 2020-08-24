# List of currently supported games

- Furi 1.5.137 (TID: 01009D3008D20000)
- Good Job! 1.0.0 (TID: 0100B0500FE4E000)
- Layers of Fear: Legacy 1.0.0 (TID: 0100BF5006A7C000)
- New Super Lucky's Tale 1.5.9 (TID: 010017700B6C2000)
- Ori and the Blind Forest: Definitive Edition (TID: 010061D00DB74000)
- Pokémon Mystery Dungeon™: Rescue Team DX 1.0.2 (TID: 01003D200BAA2000)

---

# How to create your own file

This tutorial is dedicated to games made with Unity 2017.x and newer. It's possible to make files from older Unity games, but it needs different take.

You need to disassemble main from exefs. You can do that f.e. with free tool Ghidra.

After disassembling you need to find one of supported strings, f.e.

```
"UnityEngine.QualitySettings::set_pixelLightCount"
"UnityEngine.QualitySettings::set_shadowCascade2Split"
"UnityEngine.QualitySettings::set_vSyncCount"
```

and check xref for one function that is using many strings looking similar to one you have found (if you have found function that is using only one string, this function is wrong). Decompile this function and as an example you will get:
```
FUN_7101667280("UnityEngine.QualitySettings::set_pixelLightCount",&LAB_71019f3ed0);
FUN_7101667280("UnityEngine.QualitySettings::set_shadowCascade2Split",&LAB_71019f41d0);
FUN_7101667280("UnityEngine.QualitySettings::GetQualityLevel",FUN_71019f4ac0);
```

We have two variables. Second variable has offset for function related to first variable. We need to copy that offset (which is after underscore), delete "71" from the beginning. So for example for `set_pixelLightCount` our offset is `019f3ed0`

You can use template.txt to paste offsets in correct order.

After completing all offsets (if there is no offset available in game code or this function crashes game, leave it as `00000000`. Overlay will not show any option related to it) you can use regular expressions to delete everything unnecesary, f.e in Notepad++. 

Find&Replace:

Find: `^(.*)=0x`

Replace: leave empty window

You should now have all lines only with 8 characters in each line. 
Select them all, copy it and paste in hex editor to hex window, f.e. by using HxD.

Save this file as `build_id`.hex and put to folder with title_id of game as folder name. title_id folder put to `sdmc:/SaltySD/plugins/UnityGraphics`

For example for Pokemon Mystery Dungeon 1.0.2 full path file will be:
```
sdmc:/SaltySD/plugins/UnityGraphics/01003D200BAA2000/3AB632DEE82D59448599B2291F30994A00000000000000000000000000000000.hex
```

If you want to pull request new games/versions, you need to upload hex file with template file. You can check folder stated above as an example.
UnityGraphics will introduce new settings in next few months and each new update like this can break those files, so template is necessary to convert files to newer versions faster.
