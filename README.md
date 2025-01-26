# A Scalability Mod for [FFVII Rebirth](https://store.steampowered.com/app/2909400/FINAL_FANTASY_VII_REBIRTH/)
This mod improves upon the game's graphical scalability by providing better than console visuals at the highest settings, while also trying to provide an optimized Lowest preset for devices like the Steam Deck.

# WindowsScalability.ini Tweaks
The following modifications affect the in-game graphics options.

## Background Model Detail
Internally known as `sg.StaticMeshQuality`, this setting controls the LOD of everything except for NPC's. This setting now modifies the following cvars:

|Setting|Low|Medium|High|Best||
|--|--|--|--|--|--|
|`r.Streaming.MassiveEnvironmentMipBiasForDeformMeshlets`|2 (-)|1 (-)|0 (-)|0 (-)|Higher values theoretically improve performance at the cost of worse quality mipmaps of animated objects (e.g. swaying tree trunks). Values above 2 or negative values completely cull these animated polygons.|
|`r.Streaming.MassiveEnvironmentMipBias`|2 (-)|1 (-)|0 (-)|0 (-)|Higher values theoretically improve performance at the cost of worse quality mipmaps of animated objects (e.g. swaying tree trunks). Values above 2 or negative values completely cull these animated polygons.|
|`r.MeshletCulling.ViewLODScale`|0.05 (-)|0.1 (-)|0.2 (-)|0.5 (-)|Controls the LOD of light emitting Objects (e.g. light poles, lamps).|
|`r.MassiveEnvironment.SimpleInstanceLODScale`|0.001 (-)|0.001 (-)|1.8 (-)|4 (-)|Controls the LOD of ground vegetation, excluding trees.|
|`r.MassiveEnvironment.CoverageLODScale`|0.02 (-)|0.1 (-)|0.2 (-)|1 (-)|Controls the LOD of ALL vegetation, overrides the value above.|
|`r.MassiveEnvironment.SolidLODScale`|0.1 (-)|0.2 (-)|0.4 (-)|1 (-)|Controls the LOD of Rocks and Buildings.|
|`r.MassiveEnvironment.ViewLODScale`|1 (0.1)|1 (0.15)|1 (0.2)|1 (0.15)|Controls the LOD of every object within the MassiveEnvironment system. Possibly the main cause of the pop-in issues.|

## Character Model Detail
Internally known as `sg.SkeletalMeshQuality`, this setting controls the NPC LOD.

|Setting|Low|High||
|--|--|--|--|
|`r.SkeletalMeshLODBias`|0 (0.1)|0 (0)|Higher values make the NPCs look like they're from the PS2 era. No noticeable performance impact.|

# Engine.ini Tweaks
These are optional cvars that aren't scalable. Unfortunately, we can only do so much to mitigate TAA's blurriness. UPDATE: Also moved shadow quality here.

|Setting|Value||
|--|--|--|
|`r.Shadow.MaxCSMResolution`|2048/4096/8192|A value of 8192 is very expensive but it does improve shadows substantially. Any values greater than 8192 breaks the shadows. Likewise, values lower than 2048 are too low to properly display shadows.|
|`r.Shadow.CSM.TransitionScale`|2|Unfortunately, the game doesn't allow increasing the shadow distance which limits how far the sharpest shadow cascade can be displayed. This setting should help hide the transition.|
|`r.TemporalAASamples`|2 (8)|Lowers the amount of TAA (not TAAU) jitter at the cost of higher (but arguably unnoticeable) geometric aliasing, especially at lower resolutions.|
|`r.TemporalAA.FilterSize.Alternative`|0.05 (1)|Anything lower will cause the whole screen to flicker or go black. It is dependent on the value above.|

The following table shows the optimal values between these cvars:

|`r.TemporalAASamples`|`r.TemporalAA.FilterSize.Alternative`|
|--|--|
|1|0.03|
|2|0.05|
|4|0.08|
|8|0.10|
