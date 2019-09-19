Description:
Reagent Data is a comprehensive library of all reagents used in tradeskills in World of Warcraft.
It also contains a variety of common item classes to provide a rich reagent library for other mod
developers.  In addition, it provides an access API to give developers flexibility when dealing
with the data as well as direct access to its data arrays so authors can get exactly what they
want from it.

Users: 
This mod is a base mod used by several other addons.  There is no need to directly interact
with this addon and you should not delete or otherwise alter it unless you're certain it's not
currently in use.

Mod Authors: 
Reagent Data was designed with you in mind.  It provides you a massive reagent library and
API that will automatically translate to other languages, giving your mod additional flexibility at no
coding cost.  It is as comprehensive as possible and designed to be flexible and lightweight so you
don't have to worry about coding or storing the reagent data yourself.

Installation:
Reagent Data will normally be packaged along with another addon.
If you have downloaded a standalone copy, unzip it into your
World of Warcraft directory.  This will create a ReagentData
directory in your Interface/AddOns folder.  Aside from that, it
doesn't do anything unless another mod interacts with it.

Mirror #1: http://ui.worldofwar.net/ui.php?id=617
Mirror #2: http://www.curse-gaming.com/mod.php?addid=851


Here are the standards for version numbering for this mod.  I will adhere to these as best I can.
The mod will use a three dot notation for version numbering: X.Y.Z.  In the event that the third
dot is omitted, it is understood to be a zero.

The X portion of the number refers to the version "family" of the mod.  New versions of the mod will
remain in the same family provided there are no significant changes to the API that break functionality.
This means that any mod that is compatible with the X family should be compatible with all versions of
the X family.  The mod may not full use of features introduced later in the family, but it should still
run.  X level upgrades will, therefore, be rare and only occur when a significant change to the mod is
made that will break previous addons.

The Y portion of the version number refers to the revision level of the mod.  New revisions may include
new data tables (such as the introduction of rogue poison reagents in 1.1.0), new API calls that
provide significant new functionality, and structure changes to Reagent Data tables.  No Y change should 
break a previous mod, however.  The only exception to this would be mods that directly access base
data tables.  If the Y change includes a table change, some mods may experience a nil error.  This will
be documented in the change log.

The Z portion of the version number refers to the current patch level of the mod.  This will be the most
frequently changing number of the mod.  The Z number will be updated for Blizzard TOC changes, minor
typographical errors (spelling, grammar, etc), or minor bug fixes to the API.  Z changes do not indicate
a major change in functionality.

As a final note, the three numbers are not on a fixed scale.  This means that any of the three numbers
does not have a fixed upper; they will increment as much as necessary.  If there are not a huge number of
changes, the version number could conceivably reach things like 1.2.14 as the UI TOC changes, though this
is not likely.

----------------------
-- API Information: --
----------------------

There are two primary ways of accessing data in Reagent Data: By
accessing the ReagentData table itself or by using the various
API functions.  

ReagentData Table:
The ReagentData table is a collection of subtables that hold
various base item and profession information.  The following
indices are available:

Base Item Classes:
alchemyfish - Alchemy Fish
bandage - Bandages
bar - Metal Bars
cloth - Cloth
cookingfish - Cooking Fish
element - Elements (such as Elemental Earth)
gem - Gems
herb - Herbs
hide - Hides (including the cured versions)
leather - Leather
ore - Metal ores
poison - Rogue poisons
potion - Various potions
reagent - Spell reagents (not assocaited with any class)
scale - Scales
stone - Stone

Item Classes Produced by Tradeskills
armor - Only those used in tradeskills
bolt - Cloth bolts
grinding - Grinding stones
oil - Various oils such as Blackmouth Oil and Frost Oil
other - Items that don't fir in other categories
power - Blasting powders
part - Engineering parts (including the vendor purchased ones)
rod - Metal rods

Enchanting Reagents:
dust - Enchanting dusts
essence - Enchanting essences
shard - Enchanting shards

Vendor Items:
drink - Drinks used in tradeskills
dye - Dyes
flux - Fluxes
food - Food used in tradeskills
salt - Salts (including refined deeprock)
spice - Cooking spices
thread - Threads
vendorother - Other vendor items
vial - Vials
wood - Enchanting woods

Other Item Classes:
monster - Items primarily obtained from monsters
feather - Feathers (not light feather)
spidersilk - Spider silks

Professions (Tradeskills that produce a finished product):
alchemy - Alchemy
blacksmithing - Blacksmithing
cooking - Cooking
enchating - Enchanting
engineering - Engineering
firstaid - First Aid
leatherworking - Leatherworking
tailoring - Tailoring

Gather Skills (Tradeskills that create raw materials):
fishing - Fishing
herbalism - Herbalism
mining - Mining
skinning - Skinning

Helper Tables:
professions - Contains the localized text version of profession names
gathering - Contains the localized text version of the gather skills
reverseprofessions - This allows you to easily get the index for
a profession from the localized text name.
reversegathering - This allows you to easily get the index for a
gather skill from the localized text name.
spellreagents - A multidimensional table of all classes and the
spell reagents they use.
vendor - A collection of all item clases that come from vendors
monsterdrops - A collection of all item classes that come from
monster drops

ReagentData Design Principles:
The ReagentData table holds the complete reagent information for this addon.  It was created with two 
principles in mind.  

First, each reagent will only appear by name once.  That means that there will only be one place
that says "Light Leather".  Any other references to the item will call the table reference to that base name.
This cuts down on potential typos, makes translations easier, and cuts down on memory usage by using LUA's
table reference mechanisms instead of flinging multiple copies of the strings into memory.

Second, reagents will be broken down into logical base groups based on a common attribute.  For example,
all leathers appear in a ReagentData['leather'] category because they're all leathers.  After the base groups,
other logical groups such as professions and vendor items are built by referencing the base groups as 
mentioned earlier.

One benefit of this mechanism is that only the base groups need to be altered for a translation.  By creating
a new GetLocale() if block that contains translations for the base groups, all references to those items are
automatically translated into the new language based on the client's settings.  For example, if your code
references ReagentData['leather']['light'], it will resolve to "Light Leather" on English clients.  However,
if a German client runs your mod, it will automatically resolve to "Leichtes Leder" without any special
effort on your part.

API Functions

ReagentData provides a few functions to make developing your
addon a little easier.

ReagentData_ClassSpellReagent(item)

This function takes an item name (such as "Fish Oil") and returns
an array of classes that use the reagent {"Shaman"}.  It returns the
translated text version of the name.

ReagentData_GatheredBy(item)

This function takes an item name (such as "Light Leather") and returns
an array of gather skills that are used to gather the item.  For example, 
calling ReagentData_GatheredBy("Light Leather") on an English client
will return {"Skinning"}.  Results are not sorted, so be sure to run them 
through table.sort if you want them in alphabetical order.

I can't think of any items that are gathered by more than one skill, but 
this way the function behaves the same as other API calls and is flexible 
in case we  can one day skin herbs or something.

ReagentData_GetItemClass(class)

Returns the data array for the requested item class.  This is the 
Reagent Data name for the item, NOT the translated name.  This means
you'll need to run it through ReagentData['reverseprofessions'] or
ReagentData['reversegatherskills'] first.  This function does NOT
flatten the returned function either, so keep that in mind when loading
professions; it doesn't apply to base classes such as ReagentData['bar'].

Most authors will simply want to access the ReagentData tables directly
instead of using this function, but it's provided anyway.

ReagentData_GetProfessions(item)

Returns a table that contains a translated list of all professions
that use the specified item.  For example, calling
ReagentData_GetProfessions("Light Leather") on an English client
will return {"Blacksmithing", "Engineering", "Leatherworking", "Tailoring"}.
Results are not sorted, so be sure to run them through table.sort if you
want them in alphabetical order.

ReagentData_GetSpellReagents(class)

Returns a table that contains all spell reagents used by the specified
class.  For example, calling ReagentData_GetSpellReagents("shaman"}
will return {"Ankh", "Fish Oil", "Shiny Fish Scales"}.  If class
is omitted or specified as "all", all classes and spell reagents will
be returned in a multi-dimensional array.

Boolean Functions:

ReagentData_IsMonsterDrop(item)

A Boolean function that indicates if the specified item is primarily 
obtained from monster drops.  Item is expected to be a localized string 
such as "Tiger Meat".

ReagentData_IsUsedByProfession(item, profession)

A Boolean function that indicates if the specified profession
uses the specified item.  Both profession and item are expected
to be the localized text version of the name (such as
"Copper Bar" and "Blacksmithing").

ReagentData_IsVendorItem(item)

A Boolean function that indicates if the specified item is primarily
obtained from vendors.  Item is expected to be a localized string such as "Heavy Stock".

-----------------------------------------------

Final Notes:

As I mentioned, this library was created with addon authors in
mind.  Until now, authors who wanted to use reagent data either
had to compile their own list (which is VERY time consuming) or
rely on Sea (which provides a lot of unnecessary extras, is
incomplete, and has a negative stigma).  With the release of
Reagent Data, these problems should now be solved.  If you find a
problem with Reagent Data or would like something added to it,
please contact me at reagentwatch@tarys.com.  This is your mod,
so why not try and make it the best it can be?  :-)

I'm definitely not stopping here.  With the release of Reagent
Data, I'm also releasing Reagent Info as a demonstration addon.
This mod is essentially a replacement for Reagent Helper and took
a single afternoon to develop from start to finish due in part to
the flexibility of the Reagent Data library.  In the future,
tradeskill information can also be included as is done via the
Reagent Tips addon.  Reagent Watch 3.0 and above will also
utilize Reagent Data and I'm considering a few other things to
create a Reagent Suite.  The sky's the limit!  (No Cosmos pun intended.)

Thanks To:
   * My wife for putting up with my bizarre coding desires and
   having some good data structure sense.
   * Celdor for assisting with some design concepts and
   suggesting some API features...even if your suggestions suck.
   ;-)
   * Xadros for your German translation.  You originally provided
   it for little ol' Reagent Watch and now look at what it's
   become.
   * Tuatara and Alexander for the design and prototype of the
   item link version.  This allowed Reagent Data to expand to a
   whole new level of usefulness.
   * Bima for the recipe information.  How cool is this stuff anyway?