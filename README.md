# A Scalability Mod for [FFVII Rebirth](https://store.steampowered.com/app/2909400/FINAL_FANTASY_VII_REBIRTH/)
This mod improves upon the game's graphical scalability by providing better than console visuals at the highest settings, while also trying to provide an optimized Lowest preset for devices like the Steam Deck.

# WindowsScalability.ini Tweaks
The following modifications affect the in-game graphics options.

## Background Model Detail
Internally known as `sg.StaticMeshQuality`, this setting controls the LOD of everything except for NPC's. It also controls this cvar: `r.MassiveEnvironment.LODFixedViewHeight`. For resolutions >= 1440p, only Best sets this value to equal to your vertical resolution (so 1440:1440p, 2160:2160p), while High to Low sets this value to 1080 (so 1080:1440p, 1080:2160p).

|Setting|Low|Medium|High|Best||
|--|--|--|--|--|--|
|`r.Streaming.MassiveEnvironmentPoolSizeMB`|(900)|(1600)|2000 (1600)|(2500)|Too low of a value can cause textures to alternate from high quality mips to very low quality mips.|
|`r.Streaming.MassiveEnvironmentMipBiasForDeformMeshlets`|2 (-)|1 (-)|0 (-)|0 (-)|Higher values theoretically improve performance at the cost of worse quality mipmaps of animated objects (e.g. swaying tree trunks). Values above 2 or negative values completely cull these animated polygons.|
|`r.Streaming.MassiveEnvironmentMipBias`|0 (-)|0 (-)|0 (-)|0 (-)|Higher values improve performance at the cost of worse quality mipmaps. Values > 0 can cause terrain to have 'gaps', while values above 2 looks too awful to be worth the FPS. Likewise, any negative value makes the polygons look like it came from PS2.|
|`r.MeshletCulling.ViewLODScale`|0.5 (0.2)|0.5 (0.2)|0.5 (0.2)|0.5 (0.2)|Controls the LOD of light emitting Objects (e.g. light poles, lamps).|
|`r.MassiveEnvironment.ViewLODScale`|1 (0.1)|1 (0.15)|1 (0.2)|1 (0.15)|Controls the LOD of every object within the MassiveEnvironment system. Encompasses both `CoverageLODScale` and `SolidLODScale` into a single value. These four LODScale values have a sort of 'budget' that needs to be balanced between each other, otherwise the dreaded grass flickering issue would occur.|
|`r.MassiveEnvironment.SolidLODScale`|0.1 (-)|0.2 (-)|0.3 (-)|0.5 (-)|Controls the LOD of Rocks and Buildings.|
|`r.MassiveEnvironment.CoverageLODScale`|0.1 (-)|0.2 (-)|0.4 (-)|1 (-)|Controls the LOD of ALL vegetation (trees & grass). Affects the value below.|
|`r.MassiveEnvironment.SimpleInstanceLODScale`|0.001 (-)|0.01 (-)|6 (-)|2 (-)|Separately controls the grass mip transition distance. The main cause of pop-in near the player's feet.|
|`r.MassiveEnvironment.ControlPointScreenSizeThresholdForSimpleInstance`|0.02 (0.05)|0.012 (0.025)|0.011 (0.015)|0.01 (0.01)|Controls the vegetation draw distance. No FPS difference, but can cause memory problems when set too high. Causes a lot of pop-in.|

## Character Model Detail
Internally known as `sg.SkeletalMeshQuality`, this setting controls the NPC LOD.

|Setting|Low|High||
|--|--|--|--|
|`r.SkeletalMeshLODBias`|0 (0.1)|0 (0)|Higher values make the NPCs look like they're from the PS2 era. No noticeable performance impact.|

## Fog Quality
|Setting|Low|High||
|--|--|--|--|
|`r.VolumetricFog.CheckerBoard`|1|0|Setting this to Low makes the volumetric fog use checkerboard rendering to save some FPS at the cost of increased flickering, pixelation, jitter, and ghosting near fog emitting objects.|

# Engine.ini Tweaks
These are optional cvars that aren't scalable. Unfortunately, we can only do so much to mitigate TAA's blurriness.

|Setting|Value||
|--|--|--|
|`r.Shadow.MaxCSMResolution`|(2048)/4096/8192|A value of 8192 is very expensive but it does improve shadows substantially. Any values greater than 8192 breaks the shadows. Likewise, values lower than 2048 are too low to properly display shadows.|
|`r.Shadow.CSM.TransitionScale`|2|Unfortunately, the game doesn't allow increasing the shadow distance which limits how far the sharpest shadow cascade can be displayed. This setting should help hide the transition.|
|`r.VolumetricFog.DepthDistributionScale`|16 (32)| Too high or too low a value increases the jitter. Low values also affect the 'intensity' of the fog.|
|`r.VolumetricFog.GridDivisor`|64 (120)|Decreases the percievable noise. Lower values increases noise and jitter, while higher values "supersamples" the fog resolution.|
|`r.VolumetricFog.GridSizeZ`|64 (128)|Controls the fog resolution. Higher values are more intense.|
|`r.MassiveEnvironment.UseHZBControlPoints`|1 (1)|A performance optimization to cull occluded objects. A lower quality mip will be loaded until the occluding object is removed out of the way. Unfortunately, this movement often causes the mip to jump quality, causing a pop-in. Disabling can cost upwards of 40fps depending on the scene.|
|`r.TemporalAASamples`|2 (8)|Lowers the amount of TAA (not TAAU) jitter at the cost of higher (but arguably unnoticeable) geometric aliasing, especially at lower resolutions.|
|`r.TemporalAA.FilterSize.Alternative`|0.05 (1)|Anything lower will cause the whole screen to flicker or go black. It is dependent on the value above.|

The following table shows the optimal values between these cvars:

|`r.TemporalAASamples`|`r.TemporalAA.FilterSize.Alternative`|
|--|--|
|1|0.03|
|2|0.05|
|4|0.08|
|8|0.10|
