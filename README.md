# JDFixer

Was once based on Kylemc1413's NjsFixer but has grown to much more.

I wanted a stripped down mod that focused only on JD modification to fix floaty maps without NJS/BPM modification since I don't use those features. I felt there was a gap between Njsfixer and Leveltweaks that isn't filled for JD-focused players and this is my interpretation for meeting those needs.

Supports CustomCampaigns, Tournament Assistant, Multiplayer, OST / DLC / Base Campaign, Chirality and more. Score posting is unaffected. For Beat Saber 1.16.1+.

## New Features
- **Selected map's original JD and RT is displayed.** You can easily decide if you want to use JDFixer without having to play the map to feel it. Saves time.
- **Selected map's lowest JD and RT allowed by the game is displayed.** When it seems like JDFixer "isn't working", check if you exceeded these values shown in grey.
- **Automated JD and RT fixing.** Preferences has been changed to selecting the NJS-JD (or RT) pair that is equal or lower to the selected map's NJS. This allows you to cover large ranges without having to add many values and also handles the rare non-integer NJS. For RT Preferences: Automatically sets a map's JD to give your preferred reaction time for a given NJS. Ability to fix constant JD or RT across all maps (no need to worry about slider getting moved).
- **Heuristic to bypass Preferences** where if the selected map by default runs closer than the JD in the matching NJS-JD pair, the map will run at its original JD (and likewise for RT). 
- **Upper and Lower NJS Thresholds where Preferences will be bypassed.** If a map's NJS is at or exceeds thresholds, the map will run at its original JD and RT.
- **Multiple UIs and Options** with choice of linked or single sliders for JD and RT. New purpose-built UI for Tournaments.
- **Mod Settings menu** to easily configure UI options and Thresholds.

![screenshot](https://github.com/zeph-yr/JDFixer/blob/BS_1.26/Screenshots/6.0.0_menu_main_1.png)
![screenshot](https://github.com/zeph-yr/JDFixer/blob/BS_1.26/Screenshots/6.0.0_mod_settings.png)
![screenshot](https://github.com/zeph-yr/JDFixer/blob/BS_1.26/Screenshots/6.0.0_menu_linked_1.png)
![screenshot](https://github.com/zeph-yr/JDFixer/blob/BS_1.26/Screenshots/6.0.0_menu_unlinked_2.png)
![screenshot](https://github.com/zeph-yr/JDFixer/blob/BS_1.26/Screenshots/6.0.0_preferences.png)


## How To Use
- Place JDFixer.dll in Plugins folder
- Select a map or difficulty to see its original JD and RT
- Set up Automated Preferences to select JD or RT based on map's NJS with in-game menu (If you require finer decimal values for NJS, JD and RT, you can edit your preferences in `/UserData/JDFixer.json`)
- To access the RT Preferences menu, set `Automated Preferences` to `ReactionTime` and click `Configure RT Preferences` (the purple button)
- Setting `Automated Preferences` to `JumpDistance` or `ReactionTime` will override the JD and RT sliders
- If you enable `Bypass Preferences if map is closer`, you **must** set base game settings to `Dynamic Default`
- You can configure `Bypass Preferences if NJS is` equal to, less, or greater than `Lower` and `Upper Thresholds` in Mod Settings (for ≤v5.x.x, edit values in `/UserData/JDFixer.json`)
- Hover over menu in-game for additional explanations
- **v6.0.0 for BS 1.26.0+ requires BSIPA, BSML, and SiraUtil**
- **Not compatible with NjsFixer and LevelTweaks.** Using with these mods may result in conflicts and unexpected behavior.

## UI Customization
1. **Linked JD and RT sliders:** The JD or RT slider will remember its last value when you click between maps. Scroll to the bottom of JDFixer's tab to swap. This is the default UI (v3.1.0+)
2. **One slider for JD or RT, and a display value for other:** Go to Mod Settings and toggle on `Unlink JD and RT sliders` (for ≤v5.x.x, change `legacy_ui_enabled` in `/UserData/JDFixer.json` to "true"). Scroll to the bottom of the JDFixer's tab to swap between JD and RT
3. **Show (or hide) the default and minimum Reaction Times for maps:** Go to Mod Settings and toggle on/off `Show map Reaction Times` (for ≤v5.x.x, change `rt_display_enabled` in `/UserData/JDFixer.json` to "false")
4. **TA and MP dedicated UI:** Choose to set your map by JD or RT. You can only set one at a time (v4.0.0+)
5. Min and max ranges for JD and RT sliders can be changed in `/UserData/JDFixer.json`

<div id="image-table">
    <table>
	    <tr>
    	    <td style="padding:2px">
              <img src="https://github.com/zeph-yr/JDFixer/blob/BS_1.26/Screenshots/6.0.0_menu_linked_1_cropped.png" width="500">
      	    </td>
            <td style="padding:2px">
              <img src="https://github.com/zeph-yr/JDFixer/blob/BS_1.26/Screenshots/6.0.0_menu_linked_hide_rt_cropped.png" width="500">
            </td>
        </tr>
    </table>
</div>
Above Left: Show map Reaction Times is ON. Above Right is OFF.

## Understanding Preferences Behavior
Suppose your Jump Distance Preferences contain these NJS-JD pairs: 22-18, 21-16, 18-15.

**Example 1:**
Your selected map's NJS is 22 and JD is 20. 
The map will run at 18 JD because there is an exact match for 22 NJS

**Example 2:**
Your selected map's NJS is 21.5 and JD is 20. 
The map will run at 16 JD because 21 NJS is the closest lower match.

**Example 3:**
Your selected map's NJS is 21.5 and JD is 14 *and* `Bypass Preferences if map is closer` is toggled ON.
The map will run at its original 14 JD because it is lower than your matching preference (21-16).

**Example 4:**
Your selected map's NJS is 23 and JD is 20 *and* your `Upper Threshold` is set to 23 NJS.
The map will run at its original 20 JD because it triggered the threhold.

**Example 5:**
To run every map at a constant JD regardless of its NJS, create a single preference with 0 NJS and your desired JD (e.g. 0 NJS - 18 JD)

- If `Automated Preferences` is set to `JumpDistance` but no Preferences are set, the map will run at JD and RT slider value
- If you enable the `Bypass Preferences if map is closer` heuristic, you **must** set base game settings to `Dynamic Default`. Failing to do so give you inconsistent results.
- Heuristic and Thresholds override Preferences only
- If you need decimal values for Preferences, you can manually edit them in JDFixer.json.

**Reaction Time Preferences:** 
This works exactly the same as JD Preferences. The five examples above apply, except in Reaction Time. Reaction Time is a function of the map's original NJS and Jump Distance. This means that RT Preferences automatically sets the map's JD to give your preferred RT for its given NJS.

## Tournaments and MP
- **Tournament Assistant:** Supports Default, Dual Sync and AutoPause matches. You can only use one of the sliders at a time. As usual, enabling Preferences override both sliders. *Avoid opening the Preferences menu in TA! You will be stuck in it until you relaunch the game or the coordinator lets you out (this is by design in TA). However if you do choose to get yourself stuck inside just before a match, your match will still play fine when the coordinator starts it (but I hope you've set your Preferences correctly lol).*
- **Multiplayer:** Shares the UI with TA. As usual, you can only use one of the sliders at a time and enabling Preferences override both sliders. It is safe to open the Preferences menu here lol.
- **CustomCampaigns:** Comes with display for map's default JD and RT (v3.1.0+)
- **OST, DLC Levels, Base Campaign:** These are fully supported.

<div id="image-table">
    <table>
	    <tr>
    	    <td style="padding:2px">
        	    <img src="https://github.com/zeph-yr/JDFixer/blob/BS_1.26/Screenshots/6.0.0_ta_1_cropped.png" width="500">
      	    </td>
            <td style="padding:2px">
            	<img src="https://github.com/zeph-yr/JDFixer/blob/BS_1.26/Screenshots/6.0.0_ta_2_cropped.png" width="500">
            </td>
        </tr>
    </table>
</div>

![screenshot](https://github.com/zeph-yr/JDFixer/blob/BS_1.26/Screenshots/6.0.0_mp.png)

## Versions
- v6.0.0 for BS 1.26.0+
- v5.x.x for BS 1.20.0+ only
- 4.0.0 for BS 1.19.0 / 1.19.1
- v3.x.x for BS 1.19.0 / 1.19.1 requires SiraUtil 3.0.0+ (Do not use 3.x.x versions for TA or MP)
- ≤v2.1.6 for BS ≤1.18.3 requires BS_Utils
- v2.1.3+ will import your settings file
- v2.1.0 is not compatible with settings files from previous versions: Delete or rename your old JDFixer.json and allow the mod to generate a new one. Re-enter your settings in-game. If you are knowledgeable, you can copy the relevant data from the old json file to the new one. Just make sure you do it correctly.

## About
This is my first time writing a mod. I made it for my own needs but friends thought it useful so I think it would be beneficial to share it. I hope others find this useful.
I welcome feedback and suggestions :)

## Support and Donate
Love JDFixer and would wish to show your support? [Become a patron](https://www.patreon.com/xeph_yr) and get your name featured on the Supporters panel in-game!

## Credits
Thanks @Shurdoof for autoupdate!
Thanks Kyle for the original [NjsFixer](https://github.com/Kylemc1413/NjsFixer) and thanks to the cool peeps in BSMG for the help and advice :)
