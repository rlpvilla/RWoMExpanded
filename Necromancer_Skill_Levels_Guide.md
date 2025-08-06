# How to Add Levels to Necromancer Skills in RimWorld of Magic

## Overview

The RimWorld of Magic mod already has a skill system in place for necromancers, but the maximum levels are limited. This guide explains how to modify the mod to increase skill levels and add new skills.

## Current Necromancer Skills

Necromancers currently have the following skills that can be leveled up:

### 1. Raise Undead Skills
- **TM_RaiseUndead_pwr** (Powered Creation) - Increases number of undead created and their skills
- **TM_RaiseUndead_eff** (Cheating Death) - Reduces mana cost
- **TM_RaiseUndead_ver** (Hastened Undead) - Increases undead movement speed

### 2. Death Mark Skills
- **TM_DeathMark_pwr** (Overwhelming Mark) - Increases severity
- **TM_DeathMark_eff** (Efficient Mark) - Reduces mana cost
- **TM_DeathMark_ver** (Withering Curse) - Chance for additional afflictions

### 3. Fog of Torment Skills
- **TM_FogOfTorment_pwr** (Deadly Torment) - Increases damage and healing
- **TM_FogOfTorment_eff** (Unnatural Wind) - Reduces mana cost
- **TM_FogOfTorment_ver** (Lingering Fog) - Increases duration and radius

### 4. Consume Corpse Skills
- **TM_ConsumeCorpse_pwr** (Enhanced Consumption) - Increases mana restored
- **TM_ConsumeCorpse_eff** (Greedy Consumption) - Reduces mana cost
- **TM_ConsumeCorpse_ver** (Healing Consumption) - Heals caster and damages enemy undead

### 5. Corpse Explosion Skills
- **TM_CorpseExplosion_pwr** (Violent Explosion) - Increases damage and radius
- **TM_CorpseExplosion_eff** (Corpse Mastery) - Reduces mana cost
- **TM_CorpseExplosion_ver** (Volatile Toxins) - Reduces delay and increases radius

### 6. Death Bolt Skills
- **TM_DeathBolt_pwr** (Unholy Might) - Increases damage
- **TM_DeathBolt_eff** (Eternal Learning) - Reduces mana cost
- **TM_DeathBolt_ver** (Spreading Darkness) - Increases spread range

## How to Increase Skill Levels

### Option 1: Modify MagicPowerSkill.cs (Recommended)

The easiest way to increase skill levels is to modify the `MagicPowerSkill.cs` file in the `RimWorldOfMagic/RimWorldOfMagic/` directory.

**Current Default Levels:**
- Most skills default to 3 levels maximum
- Some skills have higher caps (5, 6, 15, 30, or 50 levels)

**To Increase Necromancer Skill Levels:**

1. Open `RimWorldOfMagic/RimWorldOfMagic/MagicPowerSkill.cs`
2. Find the constructor method `MagicPowerSkill(string newLabel, string newDesc)`
3. Add specific level caps for necromancer skills:

```csharp
// Necromancer skill level caps
else if (newLabel == "TM_RaiseUndead_pwr" || newLabel == "TM_RaiseUndead_eff" || newLabel == "TM_RaiseUndead_ver")
{
    this.levelMax = 8;
}
else if (newLabel == "TM_DeathMark_pwr" || newLabel == "TM_DeathMark_eff" || newLabel == "TM_DeathMark_ver")
{
    this.levelMax = 7;
}
else if (newLabel == "TM_FogOfTorment_pwr" || newLabel == "TM_FogOfTorment_eff" || newLabel == "TM_FogOfTorment_ver")
{
    this.levelMax = 6;
}
else if (newLabel == "TM_ConsumeCorpse_pwr" || newLabel == "TM_ConsumeCorpse_eff" || newLabel == "TM_ConsumeCorpse_ver")
{
    this.levelMax = 5;
}
```

### Option 2: Add New Skills

You can also add new skills for abilities that don't currently have them. For example, you could add skills for Lich Form or other abilities.

**To Add New Skills:**

1. **Add skill definitions in `MagicData.cs`:**
   ```csharp
   public List<MagicPowerSkill> MagicPowerSkill_NewAbility
   {
       get
       {
           bool flag = this.magicPowerSkill_NewAbility == null;
           if (flag)
           {
               this.magicPowerSkill_NewAbility = new List<MagicPowerSkill>
               {
                   new MagicPowerSkill("TM_NewAbility_pwr", "TM_NewAbility_pwr_desc"),
                   new MagicPowerSkill("TM_NewAbility_eff", "TM_NewAbility_eff_desc"),
                   new MagicPowerSkill("TM_NewAbility_ver", "TM_NewAbility_ver_desc")
               };
           }
           return this.magicPowerSkill_NewAbility;
       }
   }
   ```

2. **Add skill descriptions in `Languages/English/Keyed/TM_English.xml`:**
   ```xml
   <TM_NewAbility_pwr_desc>Description of what the power skill does.</TM_NewAbility_pwr_desc>
   <TM_NewAbility_eff_desc>Description of what the efficiency skill does.</TM_NewAbility_eff_desc>
   <TM_NewAbility_ver_desc>Description of what the versatility skill does.</TM_NewAbility_ver_desc>
   ```

3. **Add skill names:**
   ```xml
   <TM_NewAbility_pwr>Power Skill Name</TM_NewAbility_pwr>
   <TM_NewAbility_eff>Efficiency Skill Name</TM_NewAbility_eff>
   <TM_NewAbility_ver>Versatility Skill Name</TM_NewAbility_ver>
   ```

4. **Add level caps in `MagicPowerSkill.cs`:**
   ```csharp
   else if (newLabel == "TM_NewAbility_pwr" || newLabel == "TM_NewAbility_eff" || newLabel == "TM_NewAbility_ver")
   {
       this.levelMax = 5;
   }
   ```

## How Skills Work

### Skill Types
- **Power (_pwr)**: Usually increases damage, effectiveness, or number of targets
- **Efficiency (_eff)**: Usually reduces mana cost
- **Versatility (_ver)**: Usually adds secondary effects or improves utility

### Skill Leveling
- Skills are leveled up using ability points gained from leveling up
- Each skill level costs 1 ability point by default
- Some skills cost 2 ability points per level
- Skills have a maximum level cap

### Skill Effects
Skills modify the abilities in various ways:
- **Raise Undead Power**: Increases undead created per cast and their skill levels
- **Death Mark Efficiency**: Reduces mana cost of Death Mark
- **Fog of Torment Versatility**: Increases duration and radius
- **Consume Corpse Power**: Increases mana restored
- **Corpse Explosion Power**: Increases damage and explosion radius
- **Death Bolt Power**: Increases damage output

## Implementation Notes

### Files Modified
1. `RimWorldOfMagic/RimWorldOfMagic/MagicPowerSkill.cs` - Level caps
2. `RimWorldOfMagic/RimWorldOfMagic/MagicData.cs` - Skill definitions
3. `Languages/English/Keyed/TM_English.xml` - Skill descriptions and names

### Compilation
After making changes, you'll need to:
1. Recompile the mod
2. Test in-game to ensure skills work properly
3. Check that skill descriptions display correctly

### Balance Considerations
When increasing skill levels, consider:
- **Power scaling**: Higher levels should provide meaningful improvements
- **Cost balance**: More powerful skills might need higher costs
- **Progression**: Skills should feel rewarding to level up
- **Game balance**: Don't make necromancers overpowered

## Example: Adding High-Level Skills

For a more advanced implementation, you could add skills that unlock at higher levels:

```csharp
// In the ability code, check skill levels
MagicPowerSkill pwr = comp.MagicData.MagicPowerSkill_RaiseUndead.FirstOrDefault((MagicPowerSkill x) => x.label == "TM_RaiseUndead_pwr");

if (pwr.level >= 5)
{
    // Unlock special high-level ability
    // e.g., raise elite undead, mass resurrection, etc.
}
```

This allows for more complex progression systems where high-level skills unlock new gameplay mechanics.

## Troubleshooting

### Common Issues
1. **Skills not appearing**: Check that skill definitions are properly added to `MagicData.cs`
2. **Descriptions not showing**: Verify language file entries are correct
3. **Skills not leveling**: Ensure level caps are set correctly in `MagicPowerSkill.cs`
4. **Effects not working**: Check that ability code properly references skill levels

### Testing
- Use debug mode to add ability points for testing
- Check skill tooltips for proper descriptions
- Verify that skill effects are applied correctly in-game
- Test with different skill level combinations

## Conclusion

The RimWorld of Magic mod provides a flexible skill system that can be easily modified to add more depth to necromancer gameplay. By increasing skill levels and adding new skills, you can create more engaging progression systems and enhance the overall gameplay experience. 