# ExtremeTempDrop

This is a mod for **The Long Dark** by Hinterland Games Studio, Inc. It allows the user to decide how fast the global temperature declines.

## [Patreon](https://www.patreon.com/ds5678)

I know many people might skip over this, but I hope you don't. You are so special, and I would appreciate your support. Modding takes lots of time, and I have expenses like food, internet, and rent. If you feel that I have improved your playing experience, please consider supporting me on my [Patreon](https://www.patreon.com/ds5678). Your support helps to ensure that I can continue making mods for you at the pace I am :)

## Installation

1. If you haven't done so already, install MelonLoader by downloading and running [MelonLoader.Installer.exe](https://github.com/HerpDerpinstine/MelonLoader/releases/latest/download/MelonLoader.Installer.exe).
2. Download the latest version of `ExtremeTempDrop.dll` from the [releases page](https://github.com/ttr/tld-ExtremeTempDrop/releases).
3. Download the latest version of `ModSettings.dll` from the [releases page](https://github.com/zeobviouslyfakeacc/ModSettings/releases).
4. Move `ExtremeTempDrop.dll` and `ModSettings.dll` into the Mods folder in your TLD install directory.

## Mods compability

This should be ok to use with any other mod that affect weather/temps.
However some mods might be adding this functionality - if so, remove this or set their overrides to 0.

This mods does expose temperature offset - this is to fill missing gap fter Hinterland derecaed GetOutdoorTempDropCelcius().

If Your mod does needs this, you can use this mods public function.
Here is example how to link dynamically to it (so it will not fail if this mod is missing)

```
namespace MyModNamespace 
{
  internall class MyMod : MelonMod
        internal static dynamic? ETDCommon = null;

        internal static void LinkDeps()
        {
            Assembly? ETD;
            try
            {
                ETD = Assembly.Load("ExtremeTempDrop");
            }
            catch
            {
                ETD = null;
            }
            if (ETD != null)
            {
                Type? ETDType;
                ETDType = ETD.GetType("ExtremeTempDrop.Common");
                //Type ETDType = Type.GetType("ExtremeTempDrop.Common, ExtremeTempDrop");
                //MelonLogger.Msg("temp (extreme1): " + ETDType);

                if (ETDType != null)
                {
                    ETDCommon = Activator.CreateInstance(ETDType);
                    MelonLogger.Msg("Linked to ExtremeTempDrop");
                }
            }
        }
(...)
  }
}
```
Then if needed, use it like this:
```
   if (ttrIndoorTemps.ETDCommon != null)
   {
       float tempdrop = (float)MyMod.ETDCommon.GetTempDropC();
   }
```
