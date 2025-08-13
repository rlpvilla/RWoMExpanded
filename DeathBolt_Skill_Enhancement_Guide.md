# Death Bolt Skill Enhancement Guide for Liches

## Overview

Death Bolt is a powerful ability that Liches gain when they transform. This guide shows you how to enhance Death Bolt skills to make them even more devastating and provide better progression for Lich gameplay.

## Current Death Bolt Skills

Death Bolt has three skill types that can be leveled up:

### 1. Power Skill (TM_DeathBolt_pwr)
- **Name**: Unholy Might
- **Effect**: Increases damage by 5 per skill level and Spreading Darkness by 3 per skill level
- **Current Max Level**: 8 (recently increased from 3)
- **Progression**:
  - Level 0: 18-35 damage; Spreading Darkness: 12-24
  - Level 1: 23-40 damage; Spreading Darkness: 15-27
  - Level 2: 28-45 damage; Spreading Darkness: 18-30
  - Level 3: 33-50 damage; Spreading Darkness: 21-33
  - Level 4: 38-55 damage; Spreading Darkness: 24-36
  - Level 5: 43-60 damage; Spreading Darkness: 27-39
  - Level 6: 48-65 damage; Spreading Darkness: 30-42
  - Level 7: 53-70 damage; Spreading Darkness: 33-45
  - Level 8: 58-75 damage; Spreading Darkness: 36-48

### 2. Efficiency Skill (TM_DeathBolt_eff)
- **Name**: Eternal Learning
- **Effect**: Reduces mana cost by 6% per skill level
- **Current Max Level**: 8 (recently increased from 3)
- **Progression**:
  - Level 0: 30% mana cost
  - Level 1: 28.2% mana cost
  - Level 2: 26.4% mana cost
  - Level 3: 24.6% mana cost
  - Level 4: 22.8% mana cost
  - Level 5: 21% mana cost
  - Level 6: 19.2% mana cost
  - Level 7: 17.4% mana cost
  - Level 8: 15.6% mana cost

### 3. Versatility Skill (TM_DeathBolt_ver)
- **Name**: Spreading Darkness
- **Effect**: Increases the spread range of darkness effects
- **Current Max Level**: 8 (recently increased from 3)
- **Progression**: Each level increases the area of effect and spread range

## How Death Bolt Works

### Base Ability
- **Damage**: 18-35 (base)
- **Cooldown**: 15 seconds
- **Range**: 40 tiles
- **Bolts**: 2 projectiles
- **Explosion Radius**: 1.6m
- **Mana Cost**: 30%

### Ability Levels
Death Bolt has multiple ability levels that unlock as you progress:
- **TM_DeathBolt**: Base ability
- **TM_DeathBolt_I**: Improved version (unlocked at skill level 1)
- **TM_DeathBolt_II**: Advanced version (unlocked at skill level 2)
- **TM_DeathBolt_III**: Master version (unlocked at skill level 3)

## Enhancing Death Bolt Skills

### Option 1: Increase Level Caps Further

You can increase the level caps even more for truly powerful Liches:

```csharp
// In MagicPowerSkill.cs
else if (newLabel == "TM_DeathBolt_pwr" || newLabel == "TM_DeathBolt_eff" || newLabel == "TM_DeathBolt_ver")
{
    this.levelMax = 12; // Increase to 12 levels for ultimate power
}
```

### Option 2: Add Special High-Level Bonuses

You can add special effects that unlock at high skill levels:

```csharp
// In the Death Bolt ability code
MagicPowerSkill pwr = comp.MagicData.MagicPowerSkill_DeathBolt.FirstOrDefault((MagicPowerSkill x) => x.label == "TM_DeathBolt_pwr");
MagicPowerSkill eff = comp.MagicData.MagicPowerSkill_DeathBolt.FirstOrDefault((MagicPowerSkill x) => x.label == "TM_DeathBolt_eff");
MagicPowerSkill ver = comp.MagicData.MagicPowerSkill_DeathBolt.FirstOrDefault((MagicPowerSkill x) => x.label == "TM_DeathBolt_ver");

// High-level bonuses
if (pwr.level >= 8)
{
    // Unlock special effects
    // e.g., instant kill chance, fear effects, etc.
}

if (eff.level >= 8)
{
    // Unlock mana regeneration bonuses
    // e.g., mana cost becomes 0, or grants mana back
}

if (ver.level >= 8)
{
    // Unlock area control effects
    // e.g., creates persistent darkness zones
}
```

### Option 3: Add New Death Bolt Variants

You can create new Death Bolt abilities that unlock at high skill levels:

```xml
<!-- In Abilities_TMagic_Necromancer.xml -->
<TorannMagic.TMAbilityDef ParentName="BaseMagicAbility">
    <defName>TM_DeathBolt_Master</defName>
    <label>Death Bolt: Master</label>
    <description>Ultimate version of Death Bolt that only Liches with maximum skill can wield.</description>
    <manaCost>0.15</manaCost>
    <learnChance>0</learnChance>
    <shouldInitialize>false</shouldInitialize>
    <learnItem>SpellOf_DeathBolt_Master</learnItem>
    <!-- Add special effects -->
</TorannMagic.TMAbilityDef>
```

## Advanced Skill Effects

### Power Skill Enhancements
- **Level 5+**: Add fear effects to targets
- **Level 7+**: Add instant kill chance (1% per level)
- **Level 8+**: Add life drain effect (heals Lich)

### Efficiency Skill Enhancements
- **Level 5+**: Reduce cooldown time
- **Level 7+**: Grant mana regeneration bonus
- **Level 8+**: Zero mana cost

### Versatility Skill Enhancements
- **Level 5+**: Increase number of bolts
- **Level 7+**: Add homing effect
- **Level 8+**: Create persistent darkness zones

## Implementation Examples

### Example 1: Fear Effect at High Levels
```csharp
// In the Death Bolt damage code
if (pwr.level >= 5)
{
    // Apply fear effect
    if (Rand.Chance(0.1f + (pwr.level * 0.02f)))
    {
        targetPawn.mindState.mentalStateHandler.TryStartMentalState(MentalStateDefOf.PanicFlee);
    }
}
```

### Example 2: Life Drain Effect
```csharp
// In the Death Bolt damage code
if (pwr.level >= 8)
{
    // Life drain effect
    float damageDealt = /* damage calculation */;
    float healAmount = damageDealt * 0.3f;
    caster.health.AddHediff(HediffDefOf.Heal, null, null, null);
    caster.health.hediffSet.GetFirstHediffOfDef(HediffDefOf.Heal).Severity = healAmount;
}
```

### Example 3: Persistent Darkness Zones
```csharp
// In the Death Bolt impact code
if (ver.level >= 8)
{
    // Create darkness zone
    TM_Action.CreateDarknessZone(impactPosition, ver.level * 2f, 60000); // 1 day duration
}
```

## Balance Considerations

When enhancing Death Bolt skills, consider:

1. **Progression**: Skills should feel rewarding at each level
2. **Power Scaling**: Don't make Liches overpowered too early
3. **Resource Cost**: Higher levels should require significant investment
4. **Game Balance**: Consider how it affects other classes and gameplay

## Recommended Skill Progression

### Early Game (Levels 1-3)
- Focus on Efficiency to reduce mana costs
- Basic Power increases for damage
- Versatility for better area control

### Mid Game (Levels 4-6)
- Balance all three skills
- Unlock advanced Death Bolt variants
- Add utility effects

### Late Game (Levels 7-8)
- Maximize Power for ultimate damage
- Efficiency for sustained casting
- Versatility for battlefield control

## Testing Your Enhancements

1. **Use Debug Mode**: Enable debug mode to add ability points for testing
2. **Test Different Levels**: Try various skill level combinations
3. **Check Balance**: Ensure the ability isn't too powerful or weak
4. **Verify Effects**: Make sure all skill effects work correctly

## Troubleshooting

### Common Issues
1. **Skills not leveling**: Check level caps in MagicPowerSkill.cs
2. **Effects not working**: Verify skill references in ability code
3. **Balance problems**: Adjust skill effects or level caps
4. **Performance issues**: Optimize skill calculations

### Debug Commands
- Use `DebugActions` to add ability points
- Check skill levels in the magic interface
- Test ability effects in combat scenarios

## Conclusion

Death Bolt is already a powerful Lich ability, but with enhanced skills, it can become truly devastating. The key is to provide meaningful progression while maintaining game balance. By increasing level caps and adding special effects at high levels, you can create a more engaging and powerful experience for Lich players.

Remember to test thoroughly and adjust based on gameplay feedback to ensure the enhanced Death Bolt skills provide the right level of power and progression for your mod. 