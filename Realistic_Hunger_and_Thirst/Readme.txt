Sydney666's Realistic Hunger and Thirst v 1.1 (7DTD v2.5)

A lore-friendly realism mod that overhauls food and water consumption in 7 Days to Die. Tired of eating and drinking every 5 minutes? Yeh, I hate that too. This mod adjusts hunger and thirst values so you only need to eat and drink 1-3 times per day, just like real life. This may be tweaked further, but I have created easy to follow xml files so you can change the values yourself. 

Key Changes:

    Increased food values across all items based on realistic nutritional content.
    Adjusted water/hydration values to match real-world expectations.
    A couple of canned goods or a proper cooked meal will keep you satisfied for hours.
    3-4 jars of water per day is enough to stay hydrated.
    Added water hydration to beer (because yes, beer does hydrate you) but please note, the hydration effect does not display but does work.
    Added dysentery risk to raw animal fat (because eating raw fat is disgusting).
    Balanced smoothies, drugs, and specialty foods for more realistic effects.
    Grandpa potions and elixirs have been left at default.

Version 1.1 Fixes display issues for Yucca Drink and Beer.


Survival should be about strategic resource management, not constant snacking. There is a reason I take Ozempic in real life. 

Installation Guide

    Download the mod folder called Realistic_Hunger_and_Thirst
    Locate your 7 Days to Die installation folder
    Navigate to the Mods folder. Create one if it doesn't exist. The Steam default location is: C:\Program Files (x86)\Steam\steamapps\common\7 Days To Die\Mods but yours may be different
    Copy the Realistic_Hunger_and_Thirst folder into the Mods folder
    For Multiplayer Servers: Install the mod on BOTH the server AND all clients. Place the mod in the server's Mods folder and restart the server. Then place the mod in each client's local Mods folder.
    Launch the game. The mod will load automatically.


Customizing Individual Item Values:

Want to tweak food and water values yourself? It's easy!

    Navigate to: Mods/Realistic_Hunger_and_Thirst/Config/items.xml
    Open items.xml with any text editor like Notepad, Notepad++, or VS Code
    Find the item you want to modify. Use Ctrl+F to search for item names.
    Change the value in the XML line


Generally only change $foodAmountAdd and $waterAmountAdd
For example, to change beer's water from 40 to 60, find the line that says value="40" and change it to value="60"
To change grilled meat's food from 90 to 100, find the line that ends with >90</set> and change it to >100</set>


Customizing Hunger and Thirst Depletion Rates:

If you want to adjust how fast your hunger and thirst bars drain over time:

    Navigate to: Mods/Realistic_Hunger_and_Thirst/Config/entityclasses.xml
    Open entityclasses.xml with a text editor
    You'll see two lines:
        FoodChangeOT controls how fast hunger depletes. Default mod value is .002
        WaterChangeOT controls how fast thirst depletes. Default mod value is .0028
    Lower values = slower depletion and less need to eat/drink. Higher values = faster depletion and more frequent eating/drinking.
    The vanilla game uses much higher values which is why you need to eat constantly.

Important Notes:

    Values are typically 1-150 where higher numbers mean more food or water (fills you up more)
    Negative values like -10 reduce water and make you thirsty
    Always keep a backup of your original file before editing
    Restart the game or server after making changes


Realistic Hunger and Thirst v1.0 - 7DTD 2.5

1.0 Changes:

1. Slowed hunger drain:
   - entityclasses.xml: FoodChangeOT = 0.002

2. Slowed thirst drain:
   - entityclasses.xml: WaterChangeOT = 0.0028

3. Buffed food:
   - items.xml: All $foodAmountAdd multiplied by ~3
   - Charred Meat = 90 food
   - Stews and large meals = 75–100+ food
   - Canned Food = 30-45 food

4. Buffed water:
   - items.xml: drinkJarBoiledWater $waterAmountAdd = 50
   - Boiled water = 50
   - Other water items scaled accordingly

Notes:
- 1–3 meals per in-game day
- 2–3 jars of water per in-game day
- Compatible with other mods not modifying food/water
- Based on 120 minute day/night cycle but should work with any.


Please report any bugs, balance issues or any missed food/drink items. 










