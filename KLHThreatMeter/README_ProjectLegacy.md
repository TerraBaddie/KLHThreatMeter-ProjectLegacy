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
