# A Scalability Mod for [FFVII Rebirth](https://store.steampowered.com/app/2909400/FINAL_FANTASY_VII_REBIRTH/)
The following write-up breaks down each relevant graphics setting to show what cvars they modify under the hood.

## Screen Resolution
|Setting||
|--|--|
|`r.RenderTargetPoolMin`|At 1080p and below, this value is set to `800`, at 1440p and higher, it's set to `1800`.|
|`r.SetRes`|Controls the screen resolution.|
|`r.CompositeLayer.FixedHeight`|At 1080p and below, this value is set to `1080`, at 1440p and higher, it's set to `2160`.|
|`r.CompositeLayer.FixedWidth`|At 1080p and below, this value is set to `1920`, at 1440p and higher, it's set to `3840`.|

## Framerate
Controls `t.MaxFPS`

## Display Sync Technology
|Setting|VRR|V-Sync|
|--|--|--|
|`r.VSync`|0|1|

## Brightness
The slider sets this cvar: `r.DeviceCorrector.Brightness.Interpolation` from values 0 to 1 at 0.1 intervals.

## HDR Luminance
The slider sets this this cvar: `r.DeviceCorrector.HDRStrength.Interpolation` from values 0 to 1 at 0.1 intervals.

## Dynamic Resolution Scaling (Minimum & Maximum)

|Setting|33%|50%|66%|100%|
|--|--|--|--|--|
|`r.DynamicRes.MaxScreenPercentage`|0.33|0.50|0.66|1|
|`r.DynamicRes.MinScreenPercentage`|0.33|0.50|0.66|1|

## Background Model Detail
This setting controls the LOD of everything except for NPC's. It is the most expensive setting overall, but it also affects the game presentation the most. It controls these cvars through **Scalability** under `sg.StaticMeshQuality`:

|Setting|Low|Medium|High|Best||
|--|--|--|--|--|--|
|`r.Streaming.MassiveEnvironmentPoolSizeMB`|1300 (900)|(1600)|2000 (1600)|(2500)|Too low of a value can cause textures to alternate from high quality mips to very low quality mips.|
|`r.Streaming.MassiveEnvironmentMipBiasForDeformMeshlets`|2 (-)|1 (-)|0 (-)|0 (-)|Higher values theoretically improve performance at the cost of worse quality mipmaps of animated objects (e.g. swaying tree trunks). Values above 2 or negative values completely cull these animated polygons.|
|`r.MeshletCulling.ViewLODScale`|0.25 (0.2)|0.33 (0.2)|0.5 (0.2)|1 (0.2)|Controls the LOD of light emitting Objects (e.g. light poles, lamps).|
|`r.MassiveEnvironment.ViewLODScale`|1 (0.1)|1 (0.15)|1 (0.2)|1 (1)|Controls the LOD of every object within the MassiveEnvironment system. Encompasses both `CoverageLODScale` and `SolidLODScale` into a single value. These four LODScale values have a sort of 'budget' that needs to be balanced between each other, otherwise the dreaded grass flickering issue would occur.|
|`r.MassiveEnvironment.SolidLODScale`|0.2 (-)|0.4 (-)|0.6 (-)|1 (-)|Controls the LOD of Rocks and Buildings.|
|`r.MassiveEnvironment.CoverageLODScale`|0.25 (-)|0.33 (-)|0.5 (-)|1 (-)|Controls the LOD of ALL vegetation (trees & grass). Affects the value below.|
|`r.MassiveEnvironment.SimpleInstanceLODScale`|2 (-)|3 (-)|4 (-)|4 (-)|Separately controls the grass mip transition distance. The main cause of pop-in near the player's feet.|
|`r.MassiveEnvironment.ControlPointScreenSizeThresholdForSimpleInstance`|0.024 (0.05)|0.020 (0.025)|0.016 (0.015)|0.012 (0.01)|Controls the vegetation draw distance. Can cause grass flickering problems when set too high. Causes a lot of pop-in.|

Additionally, it also controls these cvars by **Code**. As of update 2.0, these values are now locked to 1080p inside **DefaultEngine.ini**:

|Setting|Low|Medium|High|Best||
|--|--|--|--|--|--|
|`r.MassiveEnvironment.LODFixedViewHeight`|1080|1080|1080|1080 (1440)|Modifying this value seems to act similarly to how ViewLODScale encompasses multiple LODScale values into one, except this one encompasses ALL of them.|
|`r.MassiveEnvironment.TexelFactorFixedScreenSize`|1080|1080|1080 (1440)|1080 (1440)||

The **Low** setting stands out as the only option that reduces vegetation density. By setting sg.StaticMeshQuality to 2 (Best), which ensures the highest LOD quality for Solid, Coverage, and SimpleInstance, a noticeable difference in grass visibility remains between the [Low and Medium](https://imgsli.com/MzQ0MzM4) settings.

## Ocean Detail
|Setting|Low|Medium|High||
|--|--|--|--|--|
|`r.Ocean.LevelShift`|2|1|0|Higher values reduce how 'ripply' the ocean waves are.|

## Character Model Detail
Internally known as `sg.SkeletalMeshQuality`, this setting controls the NPC LOD.

|Setting|Low|High||
|--|--|--|--|
|`r.SkeletalMeshLODBias`|0 (0.1)|-1 (0)|Values > 0 make the NPCs look like they're from the PS2 era. Values < 0 increase the distance at which higher quality normal maps are loaded (I've only noticed it on Cloud's Buster sword on his back). No noticeable performance impact. |

## Texture Resolution
Known internally as `sg.TextureQuality`. It controls the following cvars:

|Setting|Low|Medium|High||
|--|--|--|--|--|
|`r.Streaming.PoolSize`|2048 (1100)|4096 (2700)|8192 (4000)|I've increased the streaming pool size across the board to hopefully help with loading 4K LODs from HD mods.|
|`r.Streaming.MaxTempMemoryAllowed`|100|400 (500)|1600 (500)|
|`r.Streaming.TextureResidentLODBias`|1|0|0|
|`r.Streaming.EnableTextureRuntimeLODBias`|1|1|0|
|`r.Streaming.AmortizeCPUToGPUCopy`|1 (-)|1 (-)|1 (-)|Has no effect unless the cvar below is set to a value other than 0.|
|`r.Streaming.MaxNumTexturesToStreamPerFrame`|4 (-)|16 (-)|64 (-)|0 is infinite. Values less than 4 causes the texture streaming to stall (delayed texture loading). Don't know if it has a beneficial effect on performance that makes that slow texture loading worthwhile.|
|`r.Streaming.LimitPoolSizeToVRAM`|1 (-)|1 (-)|1 (-)|By default, this setting is set to 0 which might only be beneficial for GPUs with more than 20GB of VRAM, otherwise, it should probably be 1.|

## Shadow Quality
By default, the game only modifies these cvars by **Code**:

|Setting|Low|High|Best|Extreme||
|--|--|--|--|--|--|
|`r.Shadow.FixedScreenSizeForShadow`|1920|2560|2560|2560||
|`sg.ShadowQuality`|0|1|2|3|Best and Extreme can be accessed by modifying GameUserSettings.ini or running this command: `sg.ShadowQuality`|

With the mod, it now has **Scalability**:
 
|Setting|Low|High|Best|Extreme||
|--|--|--|--|--|--|
|`r.Shadow.CSM.TransitionScale`|2 (-)|2 (-)|2 (-)|2 (-)|Unfortunately, the game doesn't allow increasing the shadow distance which limits how far the sharpest shadow cascade can be displayed. This setting should help hide the transition.|
|`r.Shadow.MaxResolution`|1024 (-)|2048 (-)|4096 (-)|8192 (-)|Controls the resolution of spotlight shadows, most noticeable at night.|
|`r.Shadow.MaxCSMResolution`|1024 (-)|2048 (-)|4096 (-)|8192 (-)|Controls the resolution of cascade shadow maps,  most noticeable at daytime.|
|`r.Shadow.WholeSceneShadowCacheMb`|500 (-)|1000 (-)|1500 (-)|2000 (-)|Too low of a value can cause shadow flickering. Values above 2047 fail to initially load some shadow maps.|

Additionally, I've decided to bind the Fog Quality within `sg.ShadowQuality` because the in-game Fog Quality setting doesn't use **Scalability**.

|Setting|Low|High|Best|Extreme||
|--|--|--|--|--|--|
|`r.VolumetricFog.DepthDistributionScale`|16 (-)|32 (-)|48 (-)|64 (-)|Too high or too low a value increases the jitter. Too low of a values can also limit the 'intensity' of the fog.|
|`r.VolumetricFog.GridDivisor`|60 (-)|120 (-)|180 (-)|240 (-)|Decreases the percievable noise. Lower values increases noise and jitter, while higher values "supersamples" the fog resolution.|
|`r.VolumetricFog.GridSizeZ`|64 (-)|128 (-)|192 (-)|256 (-)|Controls the fog resolution. Higher values look more 'intense'.|

## Fog Quality
|Setting|Low|High||
|--|--|--|--|
|`r.VolumetricFog.CheckerBoard`|1|0|Setting this to Low makes the volumetric fog use checkerboard rendering to save some FPS at the cost of increased flickering, pixelation, jitter, and ghosting near fog emitting objects.|

## Anti-Aliasing Quality
|Setting|TAA|TAAU|DLSS|
|--|--|--|--|
|`r.TemporalAA.Method`|0|1|3|

## Low-Resolution Font
This setting is only available at resolutions lower than 1080p, otherwise, it's greyed out.

# DefaultEngine.ini Tweaks
For cvars that aren't scalable.

|Setting|Value||
|--|--|--|
|`r.Shadow.TexelsPerPixel`|6.4||
|`r.Shadow.TexelsPerPixelRectlight`|6.4||
|`r.Shadow.TexelsPerPixelPointlight`|6.4 (.63662)||
|`r.Shadow.TexelsPerPixelSpotlight`|6.4 (1.27324)|A high enough value allows higher resolution shadows to display properly.|
|`r.TemporalAASamples`|4 (8)|Lowers the amount of TAA (not TAAU) jitter at the cost of lower (but arguably unnoticeable) geometric aliasing, more noticeable at lower resolutions.|
|`r.TemporalAA.FilterSize.Alternative`|0.1 (1)|Anything lower will cause the whole screen to flicker or go black. It is dependent on the value above.|
