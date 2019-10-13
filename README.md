# QDMULTI
QDMULTI is a library for mods that install multiclass kits in the Enhanced Infinity Engine (v2.0+).

## Background
Beamdog released the Enhanced Infinity Engine (v2.0) with the release of their original game, Siege of Dragonspear. Among numerous other functionality changes, this patch enabled the character creation menus to display multiclass kits. Unfortunately, any kit that would appear in the multiclass menus would not gain any of its unique class bonuses, due to the way that the engine handled multiclass kits.

This library fixes that problem, and enables multiclass kits to both appear properly in character creation and apply the appropriate bonuses during character advancement.

## Usage
If you would like to tie your mod's multiclass kits into the qdmulti framework, then you first need to download the latest version of qdmulti and include it somewhere in your mod's installation files. After adding this file to your mod structure, you can enable the functionality in your mod's compenents by adding the following code to your mod's installation files (either .tp2 or .tpa).

This line should be included **before** you use the `ADD_KIT` function (if you are adding a new kit).
```
INCLUDE ~your/folders/here/qd_multiclass.tpa~
```

This line should be included after you use the `ADD_KIT` function (if you are adding a new kit).
```
LAF qd_multiclass
  STR_VAR
      kit_name = ~kitname~ // (mandatory) The internal name for your kit (e.g. QDMAGUS).
      kit_clab = ~kitclab~ // (optional) The internal name of your kit's clab file, without the .2da extension.
                           // Leave empty to use the CLAB entry defined in kitlist.2da instead.
      base_class = ~X~     // (optional) This can take 6 values: [F]ighter, [P]riest, [D]ruid, [R]anger, [M]age
                           // or [T]hief. Alternatively, specify a symbolic class name (e.g. FIGHTER) or their
                           // numeric class value (e.g. 2 for FIGHTER).
                           // Leave empty to have the function pick an appropriate base class automatically.
END
```

Of the parameters utilized, the only one that I feel needs further explanation is the "base_class" parameter. This parameter handles which class the kit abilities will be tied to; if you say that a multiclass kit's base class is fighter (`base_class = "F"`), then it will gain the assigned kit bonuses from increases in its fighter level.

It is also possible to override the `base_class` parameter by preceding CLAB table entries of the kit with a class token. This token allows you to fine-tune which ability will be applied by which class at level-up. For example, to specifically assign the CLAB entry `GA_SPCL423` to the thief class aspect (e.g. of a multiclass Fighter/Thief kit), precede it with `T` to create `TGA_SPCL423`.

### Limitations
Note that this library is only designed to handle class benefits that are granted through the class' clab file; it will *not* handle specialist mages' school restrictions, specific kit's item restrictions, or other effects tied to the kit's usability flags.

Furthermore, due to the length restrictions on internal names, **the library can only include abilities that have internal names of 7 characters or less**. Abilities with internal names of 8 characters (or more) will not be converted by the `qd_multiclass` function.

### Applications
This library is very versatile, and can be used in a variety of scenarios. Some of the things it can do include:
* giving a kit to a multiclass character at character creation (e.g. Swashbuckler/Mage).
* allowing a multiclass character to have one kit for each of their classes (e.g. a Berserker/Priest of Talos).
* allowing modders to implement their own multiclass kits that have abilities tied to each of the base classes (e.g. the Bladesinger Fighter/Mage kit).
* allowing a single- or multiclass character to gain the benefits of multiple kits of the same class.

## QDMULTI in Action
Mods that make use of QDMULTI include:
* **[Monastic Orders](https://github.com/aquadrizzt/MonasticOrders)** by aquadrizzt
* **[Eldritch Magic](https://github.com/AbdelAdrian/Eldritch_Magic)** by Abdel_Adrian
* **[Might and Guile](https://github.com/subtledoctor/Might_and_Guile)** by subtledoctor
* **[Arcane Archer](https://github.com/ArtemiusI/Arcane-Archer)** by ArtemiusI

## Permissions and Support
I am publishing QDMULTI so that other modders may make multi-class kits more reliably and efficiently. I do not require nor expect modders to contact me requesting permission to use this library. If you encounter errors while using this library, please contact me so that I may provide fixes and updates.
