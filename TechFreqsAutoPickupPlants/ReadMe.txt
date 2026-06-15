TechFreqs Auto Pickup Plants is a mod, for 7 Days to Die that automates the pickup of harvestable plants within a configurable radius around the player. It supports single-player or client games UNTESTED on server however it awards XP, adds to challenges ui for plants. The mod dynamically reloads its configuration file (config.json) periodically without requiring manual commands, ensuring seamless updates to settings like pickup radius or toggle key.

Although it says AUTO PICKUP PLANTS its more of a semi automatic pickup plants through toggle key, to retain PERFORMANCE per client.

THE LAST CHANGELOG:
V1.5 for V2.6 

🚀 1. Complete Lag Elimination (Asynchronous Scanning)
Old: The mod scanned every block in a radius instantly on the main game thread. At a radius of 100, this meant checking ~11 million blocks in a single frame, which would completely freeze the game for several seconds.
New: The scan was moved into a Coroutine. It now scans one chunk, lets the game render a frame, then scans the next chunk. You can now set the radius to 100+ and the game will remain perfectly smooth while it scans in the background over a couple of seconds.

🧠 2. Memory Leak Fixed (processedPickups)
Old: Picked-up plants were saved into an infinite HashSet<string>. Over hours of gameplay, this list grew forever, slowly eating up RAM until the server/client bogged down.
New: Replaced with a lightweight Dictionary<Vector3i, float>. A built-in cleanup function now runs automatically at the start of every scan, permanently deleting the memory of any plant picked up more than 10 seconds ago. Your mod now uses practically zero RAM.

🗑️ 3. Garbage Collection Spikes Removed
Old: The code used Enum.Parse inside the Update loop (running 60+ times a second) and built text strings ($"...") inside the deepest block-scanning loops. This forced the game's Garbage Collector to work overtime, causing micro-stutters.
New: Enum.Parse now only runs exactly once when config.json is loaded, and all text strings were removed from the scanning loops.

🛡️ 4. Prevented "Scan Spamming"
Old: If a player mashed the End key, the mod would trigger multiple massive scans simultaneously on top of each other, multiplying the lag.
New: Added an isScanning lock. If a scan is currently happening, the mod ignores further auto/manual scan requests until the first one is finished.

🌍 5. Survives Returning to the Main Menu
Old: Your config-checking coroutine started inside InitMod(). If a player quit to the main menu and loaded a new world, the coroutine died and never restarted, breaking the auto-reload feature.
New: The config-checker is now started when the player spawns into the world (EntityPlayerLocal.Update initialization). It will now perfectly survive world-hopping and main menu trips.

🔗 6. Unbreakable XML Parsing
Old: You used xpath.IndexOf("name='") to read blocks.xml. If another mod or a future game update used double quotes (name="plant"), your mod would silently fail to recognize any plants.
New: Swapped to a Regular Expression (Regex). It will perfectly find the plant names regardless of how weird the quotes or spacing in the XML file get.

🎲 7. True Seed Randomization
Old: Created a floating GameRandom instance.
New: Now uses GameManager.Instance.World.GetGameRandom(). This ensures that your custom harvest drop math properly syncs with the specific world seed the player is currently playing on, just like vanilla harvesting.

🧹 8. Code Cleanup & Smart Debugging
Old: Had three "dead" Harmony patches that only printed logs, adding unnecessary processing overhead. Furthermore, LogAlways was heavily spammed during item calculations, which would fill up player log files even if they didn't want it to.
New: Removed the empty patches to speed up the mod. All your detailed production logging (item math, dropped counts, detected blocks) was restored, but routed through LogDebug. Now, you can see all the math when DebugLogging is true, but regular players won't be spammed with logs when it's false!

Thanks to Antigravity a i google angentic ide and google gemini 3.1 pro

( however untested in a server environment, a.k.a copied mod to server mod and client dll, as this is intended for solo or as a client mod only but still somehow works in a server environment if used in both mod directories USAGE MAY VARY)


Semi- Latest Changelog:
V1.4 BETA (V2.5)
- Updated for 7D2D V2.5 B32
- Changed startenabled to false in config json for those who wouldnt want that pickup plants to auto start
- BETA as untested for use on a dedicated server, still uses features from V1.3 Beta and previous revision ,same scalability for lving off the land perk

( however untested in a server environment, a.k.a copied mod to server mod and client dll, as this is intended for solo or as a client mod only)

- Fixed issue rendering a massing Nullreference expcetion due to ingame changes for Progression Add Level XP in the code expecpting a second parameter instead of a single version
Fixed issue in detail - 
[TechFreqsAutoPickupPlants] Error in EntityPlayerLocal.Update: Method not found: int .Progression.AddLevelExp(int,string,Progression/XPTypes,bool,bool)
StackTrace:   at TechFreqsPlayerUpdatePatch.Postfix (EntityPlayerLocal __instance) [0x001fd] in <0a38b36dc03f4ef2a84fba878a86e0d6>:0 



Latest Changelog:
V1.3 BETA
- Added New Feature, An Independant AutoToggle & Manual Key Defaulted to Home and or End Key in configurable json
- Added New Feature, Play Pickup Sound for true or false in configurable json
- Previous Fixes and features From v1.2,v1.1,v1.0

CONFIG ADDITIONS
  "AutoToggleKey": "Home",
  "ManualToggleKey": "End",
  "PlayPickupSound": false or true


V1.2 BETA
- Fixed issue for Plant placeable block should give harvest and seed drops.


V1.1 BETA
- Added AutoPickup Feature via radius
Automatic Vicinity Pickup: Plants within a configurable radius are automatically harvested when you move a minimum distance (e.g., 5 blocks). No need to press keys— just walk/run!
- Added NEW Manual/ Automatic Mode/Toggle: Press a key (default: End) to enable/disable auto-pickup or trigger a one-time manual scan. Supports two modes:
Automatic: Scans on movement (if enabled).
Manual: Only scans when you press the toggle key.
- Server-Compatible Harvesting: On dedicated servers, plants are properly removed (harvested), items added to inventory, XP awarded (default: 10 per plant), and challenges triggered (e.g., "challengeHarvest"). No duplicates or exploits for xp.
- Inventory Handling: Items are added directly to your backpack. If full, they drop on the ground with a tooltip/sound alert—no unwanted bags.
- Enhanced Debug Logging: Detailed logs for troubleshooting (toggleable in config). Tracks scans, pickups, XP, and errors.
- Same features from V1.0 For manual methods

== NEW CONFIG JSON CONFIGURATION
{
  "PickupRadius": 10.0,          // Distance around player to scan for plants (clamped to MaxPickupRadius)
  "MaxPickupRadius": 100.0,      // Maximum allowed radius
  "ToggleKey": "End",            // Key to toggle enable/disable or manual scan (Unity KeyCode names)
  "ConfigCheckInterval": 300.0,  // Seconds between config reload checks (default: 5 min)
  "DebugLogging": true,          // Enable detailed logs (set to false for less spam)
  "StartEnabled": true,          // Auto-pickup starts enabled on load
  "MinScanDistance": 5.0,        // Minimum blocks player must move to trigger auto-scan
  "Mode": "automatic",           // "automatic" (scan on movement) or "manual" (only on toggle key)
  "ShowTooltips": true           // Show in-game messages (e.g., "Config reloaded", "Inventory full")
}

==
How It Works
Scanning: On player update, checks if you've moved enough (MinScanDistance) blocks. Scans chunks around you for plants listed in the mods blocks.xml.
Harvesting: For each detected plant, runs custom logic to collect items, award XP/challenges, and request server pickup (removes block).
Server Sync: Uses Harmony patches on NetPackagePickupBlock to handle dedicated server processing, ensuring blocks are removed and no exploits.
Safeguards: Skips air/null/already-processed blocks. Handles inventory gracefully.

ISSUES FOR V1.1:
Microstuttering while walking, depending on the Pickup Radius set, I've had some stutter when walking around with it set to 30, However this was on a server not client
beware of higher pickup radius for even more performance loss , as it tries to collect plants or crops within that configurable radius.



V1.0 (V2.2)
- V2.2 of 7D2D
- NON EAC


Key Features:
Semi - Automatic Plant Pickup: 
Harvests plants (e.g., plantedSnowberry3Harvest, plantedBlueberry3Harvest) within a configurable radius (default 10, max 100) when toggled on.
Defaulted to "End" key, using the Unity Key Code Documentation for various configurable toggles for the mod functionality. You Can toggle it on and off repeatedly for areas with crops or plants that you'd like to harvest and what not. 
UNTESTED if it adds on with the Living off the land perk! so keep that in mind! may no be guaranteed results for seeds and all. UNTESTED

Based on the TechFreqs Pickup Plants Mod! via blocks xml sharing the same properties, but also hinting at DaDiGui Auto Pickup Arrows and Bolts concept idea! Forming a version for this mod that i had in mind for quite sometime.


Configurable Toggle Key:
Enable/disable auto-pickup with a configurable key (default End). Use Unity Key Codes for key toggle changes

Dynamic Config Reloading:
(Due to issues with implementing a reload config command)
THIS is an alternate method where the modm, Checks the config.json every 5 minutes (adjustable) for changes, applying updates without restarting the game for changed Radius.

XP and Quest Tracking:
Awards 10 XP per plant and tracks harvest challenges via QuestEventManager.

Inventory Overflow Handling:
Drops items near the player with a sound (ui_denied) if the inventory is full.

Multiplayer Compatibility:
Works on client, with permission checks to prevent unauthorized pickups.

Air Block Fix:
Skips invalid blocks (e.g., air, terrDirt) to prevent errors or log spam as previous implementation.

Configurable Logging:
Minimal logs for plant pickups and toggle events; detailed logs (e.g., chunk scanning, config checks) when DebugLogging is enabled.

Performance Optimization:
Scans ~1000 blocks at radius 10 (~9 chunks) and ~25000 blocks at radius 100 (~49 chunks), with minimal performance impact for config checks.



CONFIG JSON Configuration
Edit config.json in Mods/TechFreqsAutoPickupPlants/ to customize settings.
Changes are automatically applied within ConfigCheckInterval seconds in mod code (defaulted to 5 minutes).
CONFIG JSON
{
  "PickupRadius": 10.0,
  "MaxPickupRadius": 100.0,
  "ToggleKey": "End",
  "ConfigCheckInterval": 300.0,
  "DebugLogging": true
}


Config JSON Explanation:
PickupRadius: Radius in game units for plant pickup (e.g., 10.0, 50.0, 100.0). Clamped to MaxPickupRadius.

MaxPickupRadius: Maximum allowed radius (default 100.0).

ToggleKey: Unity KeyCode to toggle auto-pickup (e.g., End, P, F1). See Unity KeyCode documentation.

ConfigCheckInterval: Seconds between config.json checks (e.g., 300.0 for 5 minutes, 60.0 for 1 minute). Avoid very low values (<10) to prevent performance issues.

DebugLogging: true for detailed logs (e.g., chunk scanning, config checks); false for minimal logs (plant pickups and toggle events only).


Usage:

Toggle Auto-Pickup:

Press the configured ToggleKey (default End) to enable/disable auto-pickup.

Logs show: [TechFreqsAutoPickupPlants] Auto-pickup enabled/disabled for Player: <ID>.


Semi Automatic Plant Pickup:

When enabled, plants within PickupRadius are harvested automatically.


Successful pickups award 10 XP, play item_plant_pickup sound, and update quest challenges.

If no plants are found or picked up, a tooltip ("No plants found within radius!" or "No plants picked up!") and ui_denied sound are triggered.



Dynamic Config Updates:

Edit config.json (e.g., change PickupRadius to 20.0 or ToggleKey to however you prefer) and save.

Within ConfigCheckInterval seconds, the mod reloads the config, shows a tooltip ("TechFreqsAutoPickupPlants: Config reloaded"), and logs:

[TechFreqsAutoPickupPlants] Config reloaded: PickupRadius=20.0, MaxPickupRadius=100.0, ToggleKey=P, ConfigCheckInterval=300.0, DebugLogging=true


Manual Pickup:

Interact with plants directly to harvest them, with the same XP, sound, and quest tracking as the semi auto-pickup thats functional at the time being via key toggle, HOWEVER DOES NOT interefere with the TechFreq PickUp Plants Mod, making it compatible for both!.








Disclaimer:
By using this mod, you acknowledge that TechFreq is not responsible for any issues, crashes, or conflicts caused by its use.
Use at your own risk. Please backup your game files before installing any type of mod.
Thanks for downloading and enjoy!



Installation: 
Make sure harmony mod exist in the mod directory as it's required.
Download the mod files, Extract Mod files.
Please backup your world, save, and or game files.
Place them in your Mods directory of your 7 Days to Die Game.
EAC must be disabled, although i hope in the future that can be changed, as for now DLLS are not EAC supported however XML has no issue
THIS IS CLIENT SIDE ONLY but maybe perhaps this is also, server side and client side compatibility?
No further setup needed. Enjoy!




Support Notice:
For those who’d like to support 'TechFreqs' work, This mod may or may not be crossposted. Downloading via 7daystodiemods website through ModsFire (ad-powered, which earns per download) and helps me a ton if posted on there or other example mod sites HOWEVER!
(although its 10 cents per download) There is also a direct mirror for using NexusMods website instead which also features direct links BUT those aren't earned per click or per downloads and run off Donation Points through a different system. The best way to support TechFreq other than downloading mods, sharing the mod with friends, leaving feedback and endorsing the mod in general is all that i ask for, but if you want to go the extra mile although not necessary you may use Donation Links through paypal or ko-fi pages which again helps me a bunch!
However, Donations aren't expected, every little bit of support helps along the way & fuels more mods, music, and bug fixes in the future,so thanks again for reading and being awesome in general and checking out the mod post.





CREDITS:
Thanks to TechFreq & A.I, ChatGPT or Microsoft CoPilot A.I or Grok AI from Twitter or X, for helping me create the modlet, aswell as with very little modding knowledge for the game and learning as i go i couldn't do this without it and overall brainstorming and or the modding community.
I’d very much appreciate it and or any feedback for the mod(s) aswell



Social Media:
If you appreciate 'TechFreqs' work and want to show support, use this donation link, although not necessary.
Kofi Page: https://ko-fi.com/techfreq
I appreciate it in general for just checking out the mod posts, sharing and enjoying any of the mods in itself. Thank you again! and Happy gaming!
Love this mod? Got feedback or ideas or need to troubleshoot?

Join the TechFreq Pretty Rad Squad Discord Server! https://discord.com/invite/SQCnGjNUhw
Chill with us on Discord for game chat, memes, and even more mod updates!
As for TechFreqs music, it's royalty-free music to use in your projects or for casual listening!

Source music files are available feel free to ask away, available in the discord! or for more content! 
TechFreqs Socials: https://beacons.ai/techfreq
Checkout the behind-the-scenes vibes today! Thank you again for checking out the mod post.




License: CC BY-NC-SA 4.0  
This mod is licensed under Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International. You can use it for personal play in *7 Days to Die*. Modifications or sharing require crediting TechFreq, linking to the mod page, and using the same license for derivatives. Contact me at beacons.ai/techfreq for permission for any modifications or changes. 
See LICENSE.txt or http://creativecommons.org/licenses/by-nc-sa/4.0/ for full terms.  
Note: Monetized videos/blogs showcasing this mod are allowed along as with credit to TechFreq.

