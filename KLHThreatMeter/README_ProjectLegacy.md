## PL4 - Pet death/token safety

- Fixes `KTM_PetMod.lua:45: table index is nil` when the player dies while a Hunter/Warlock pet remains active.
- Caches the last valid pet name and level so 1.12 client transitions where `UnitName("pet")` / `UnitLevel("pet")` temporarily return nil cannot be used as table keys or numeric values.
- If the owner is dead and the pet unit token disappears while the pet is still fighting, KTM keeps the cached pet identity and can continue accepting that pet's combat-log damage instead of immediately dropping it.
- Safely clears the cached pet threat entry once the normal pet unit/combat state becomes available again.


## PL3 - VMaNGOS resource threat model

- Changes KTM resource-gain threat from the older retail-theory `Mana 0.5 / Rage 5 / Energy 5` constants to the behavior found in current upstream **VMaNGOS 1.12** source.
- Natural Mana/Energy regeneration remains **0 threat** (it is not parsed as an energize event).
- Direct/instant Energize gains are **0 threat** for Mana, Rage, and Energy.
- Periodic Mana energize is **0 threat**.
- Periodic Rage and Energy energize are **0.5 threat per actual point gained**.
- The existing `powergain(amount, powertype, spellid)` API is unchanged so the Project Legacy DPSMate KTM hook remains compatible.
- KTM already models VMaNGOS-style healing threat correctly: base healing threat is 0.5x effective healing, with KTM's Paladin healing multiplier reducing Paladin healing to 0.25x effective healing.


## PL2 - Holy school hardening

- Extends the Project Legacy Holy fallback to normal Paladin Holy damage such as **Judgement of Righteousness**, Seal of Righteousness, Consecration, Holy Shield, Holy Shock, Exorcism, Holy Wrath, Hammer of Wrath, and Seal/Judgement of Command.
- This does **not** add bonus threat to those spells. It only supplies the Holy school when an old 1.12 combat-log form omits it, allowing KTM's existing Holy-only Righteous Fury logic to run.
- Crusader Strike and Crusader's Inquest remain plain Holy damage with no innate threat modifier.
- No cast-to-white-hit Crusader Strike correlation is added yet; that requires confirming the exact Project Legacy combat-log sequence so a real autoattack cannot be misclassified.

# KLHThreatMeter - Project Legacy

Project Legacy compatibility fork based on **KLHThreatMeter 17.39.243**, the release immediately before the TurtleWoW threat API was added.

Project Legacy still uses the **WoW 1.12.1 client/API**; its website's current content patch label is separate from the client interface version.

## PL1 changes

- Preserves the original 1.12 combat-log threat engine and network protocol.
- Preserves KTM's existing **Holy-only Righteous Fury scope** on Project Legacy. Physical/white damage is not multiplied by Righteous Fury.
- Corrects Improved Righteous Fury to the Vanilla-style 16/33/50% improvement of Righteous Fury's +60% bonus:
  - 0/3: 1.600x Holy threat
  - 1/3: 1.696x Holy threat
  - 2/3: 1.798x Holy threat
  - 3/3: 1.900x Holy threat
- Recognizes **Crusader Strike** and **Crusader's Inquest** as Project Legacy Holy damage when a 1.12 combat-log line omits the damage school. They have no added threat multiplier of their own.
- Recognizes **Protector's Command** as a taunt and routes it through KTM's existing taunt threat-equalization logic.
- Adds Project Legacy **Reverberation** support for Shock threat reduction. The addon reads the live talent tooltip for the actual percentage instead of hard-coding an unverified value.
- Stops treating **Improved Heroic Strike** as a Rage-cost reduction on Project Legacy; its damage change is already reflected in combat-log damage.
- Adds names for Project Legacy abilities used by the threat parser: Crusader Strike, Crusader's Inquest, Protector's Command, Mongoose Bite, Carve, and Envenom.

## Class-change audit

Hunter Mongoose Bite/Carve, Rogue Envenom, Priest Shadowform damage changes, Druid Balance damage changes, and Warrior damage changes are naturally reflected by KTM because KTM uses the actual damage reported by the 1.12 combat log unless an ability has a documented special threat rule.

## Testing priorities

1. Paladin without Righteous Fury: Crusader Strike/Inquest threat should equal their Holy damage (subject only to normal global modifiers).
2. Paladin with untalented Righteous Fury: Holy threat should be 1.600x.
3. Paladin with 1/3, 2/3, 3/3 Improved Righteous Fury: verify 1.696x, 1.798x, 1.900x.
4. Protector's Command: verify KTM equalizes threat like Taunt.
5. Shaman Reverberation: run KTM's threat diagnostic and verify the displayed Project Legacy Shocks multiplier matches the in-game talent tooltip.

This is a community compatibility fork and is not affiliated with Project Legacy or the original KLHThreatMeter authors.
