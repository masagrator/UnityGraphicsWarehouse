# How to create your own file

You need to disassemble main from exefs. You can do that f.e. with free tool Ghidra.

After disassembling you need to find one of supported strings, f.e.

```
"UnityEngine.QualitySettings::set_pixelLightCount"
"UnityEngine.QualitySettings::set_shadowCascade2Split"
"UnityEngine.QualitySettings::set_vSyncCount"
```

and check xref for one function that is using them all. Decompile this function and as an example you will get:
```
FUN_7101667280("UnityEngine.QualitySettings::set_pixelLightCount",&LAB_71019f3ed0);
FUN_7101667280("UnityEngine.QualitySettings::set_shadowCascade2Split",&LAB_71019f41d0);
FUN_7101667280("UnityEngine.QualitySettings::GetQualityLevel",FUN_71019f4ac0);
```

We have two variables. Second variable has offset for function related to first variable. We need to copy that offset (which is after underscore), delete "71" from the beginning. So for example for `set_pixelLightCount` our offset is `019f3ed0`
