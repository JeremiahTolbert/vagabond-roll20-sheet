# Vagabond Roll20 Character Sheet Specification

## Project Overview

Create a fully functional Roll20 character sheet for the Vagabond TTRPG system. The sheet must include automated calculations, dice rolling with favor/hinder mechanics, and a layout that closely matches the official PDF character sheet while optimizing for Roll20 usability.

---

## Technical Requirements

### Platform
- **Target**: Roll20 Virtual Tabletop
- **Components**: HTML, CSS, JavaScript (Roll20 sheet workers)
- **Responsive**: Multi-column layout above 768px, single column at/below 768px
- **Template Complexity**: Medium to Complex HTML/CSS

### Roll20 Sheet Structure
- Use Roll20's character sheet framework
- Implement sheet workers for auto-calculations
- Use Roll20's roll template system for chat output
- Follow Roll20 naming conventions (`attr_`, `repeating_`, etc.)

---

## Layout & Visual Design

### Overall Structure
- **Desktop (>768px)**: Multi-column layout matching PDF structure
  - Left sidebar: Stats display
  - Center-left: Character info, HP, armor, speed, saves, skills
  - Center-right: Attacks section
  - Right column: Inventory, Abilities, Magic
- **Mobile (≤768px)**: Single column, same sections stacked vertically

### Visual Style
- Approximate the official PDF aesthetic (dark sidebar, bordered sections)
- Black and white color scheme with high contrast
- Clear section headers with bold styling
- Input fields with visible borders
- Checkboxes for training/toggles
- Professional, readable fonts (system fonts acceptable)

### Section Borders & Organization
- Clear visual separation between major sections
- Boxed sections for: Hit Points, Armor, Saves, Skills, Attacks, Inventory, Wealth, Magic
- Header branding area with "VAGABOND PULP FANTASY ROLEPLAYING GAME"

---

## Data Fields & Calculations

### 1. Character Header Section

#### Input Fields
- **Name** (text input)
- **Ancestry** (dropdown): Human, Dwarf, Elf, Halfling, Draken, Goblin, Orc
- **Class** (dropdown): Alchemist, Barbarian, Bard, Dancer, Druid, Fighter, Gunslinger, Hunter, Luminary, Magus, Merchant, Pugilist, Revelator, Rogue, Sorcerer, Vanguard, Witch, Wizard
- **Level** (number input, min: 1, max: 10, default: 1)
- **XP** (number input, default: 0)
- **Size** (dropdown): Small, Medium, Large
- **Being Type** (text input, default: "Humanlike")

---

### 2. Stats Section (Left Sidebar)

#### Six Core Stats
Each stat displayed in a large box with the stat name and value:
- **Might** (number input, min: 2, max: 7, default: 2)
- **Dexterity** (number input, min: 2, max: 7, default: 2)
- **Awareness** (number input, min: 2, max: 7, default: 2)
- **Reason** (number input, min: 2, max: 7, default: 2)
- **Presence** (number input, min: 2, max: 7, default: 2)
- **Luck** (number input, min: 2, max: 7, default: 2)

#### Character Creation Helper (Toggleable)
- **Toggle Button**: "Character Creation Mode" (checkbox to show/hide)
- **Points Remaining Display**: Shows `14 - (sum of all stats - 12)` remaining points
- **Validation**: 
  - Prevent stats from going below 2 or above 7
  - Show warning if points remaining is negative
  - Show success message when exactly 0 points remain
- **Helper Text**: "Distribute 14 points among your stats (min 2, max 7 per stat)"

#### Auto-Calculated Derived Values (Display Only)
- **Max HP** = `Might × Level` (displayed near HP section)
- **Inventory Slots (Max)** = `8 + Might` (displayed in Inventory section)

---

### 3. Hit Points & Status Section

#### Hit Points
- **Current HP** (number input, default: equal to Max HP)
- **Max HP** (calculated display: `Might × Level`)
- **HP Input Field**: Large, prominent input
- **Max HP Display**: Shown as "/ [max]" next to current HP

#### Armor
- **Armor Rating** (number input, min: 0, max: 3, default: 0)
- Display as shield icon or "AR: [value]"

#### Fatigue
- **Fatigue Boxes**: 5 checkboxes in a row
- **Labels**: Box 1-4: "Fatigue", Box 5: "DEAD" (red text/styling)
- **Visual**: Each box is a clickable checkbox
- **Effect Note**: "Each Fatigue: -1 Attack damage, +1 damage taken"

---

### 4. Speed Section

#### Speed Fields
- **Speed** (calculated display)
  - Formula based on Dexterity:
    - DEX 2-3: 25 ft
    - DEX 4-5: 30 ft
    - DEX 6-7: 35 ft
- **Speed Bonus** (number input, default: 0) - for magical/equipment bonuses
- **Total Speed** (calculated display): `Speed + Speed Bonus`
- **Crawl Speed** (calculated display)
  - Formula based on Dexterity:
    - DEX 2-3: 75 ft
    - DEX 4-5: 90 ft
    - DEX 6-7: 105 ft
- **Travel Speed** (calculated display)
  - Formula based on Dexterity:
    - DEX 2-3: 5 miles
    - DEX 4-5: 6 miles
    - DEX 6-7: 7 miles

---

### 5. Luck Pool Section

#### Current Luck
- **Current Luck** (number input, min: 0, max: equal to Luck stat, default: equal to Luck stat)
- **Max Luck** (display only: equal to Luck stat)
- **Display Format**: "Current: [input] / Max: [Luck stat]"
- **Quick Buttons**: 
  - "Spend 1" (decrements Current Luck by 1)
  - "Rest" (resets Current Luck to Max Luck)

---

### 6. Saves Section

#### Three Saves (Display Only, Auto-Calculated)
Each save shows: **Save Name**, **Formula Reference**, **Difficulty Number**

1. **Reflex** `[DEX + AWR]`
   - **Formula**: `20 - (Dexterity + Awareness)`
   - **Display**: "Reflex: [calculated number]"
   - **Subtitle**: "Reflex and evasion"
   - **Roll Button**: Rolls d20 vs difficulty

2. **Endure** `[MIT + MIT]`
   - **Formula**: `20 - (Might + Might)` = `20 - (Might × 2)`
   - **Display**: "Endure: [calculated number]"
   - **Subtitle**: "Withstand poison and death"
   - **Roll Button**: Rolls d20 vs difficulty

3. **Will** `[RSN + PRS]`
   - **Formula**: `20 - (Reason + Presence)`
   - **Display**: "Will: [calculated number]"
   - **Subtitle**: "Resist curses and enthrallment"
   - **Roll Button**: Rolls d20 vs difficulty

---

### 7. Skills Section

#### 12 Skills with Training Checkboxes
Each skill row contains:
- **Checkbox** (trained/untrained)
- **Skill Name**
- **Associated Stat** (in brackets, grayed out)
- **Difficulty** (calculated display)
- **Roll Button**
- **Description** (small text below name)

#### Skill List & Calculations

**Formula**:
- **Untrained**: `20 - Stat`
- **Trained** (checkbox checked): `20 - (Stat × 2)`

1. **Arcana** `[RSN]`
   - Description: "Magic knowledge & esoteric sense"
   - Difficulty: `20 - Reason` or `20 - (Reason × 2)` if trained

2. **Brawl** `[MIT]`
   - Description: "Physical prowess & brute force"
   - Difficulty: `20 - Might` or `20 - (Might × 2)` if trained

3. **Craft** `[RSN]`
   - Description: "Artisanal talent & value perception"
   - Difficulty: `20 - Reason` or `20 - (Reason × 2)` if trained

4. **Detect** `[AWR]`
   - Description: "Perception, deduction & reflex"
   - Difficulty: `20 - Awareness` or `20 - (Awareness × 2)` if trained

5. **Finesse** `[DEX]`
   - Description: "Physical coordination & talent"
   - Difficulty: `20 - Dexterity` or `20 - (Dexterity × 2)` if trained

6. **Influence** `[PRS]`
   - Description: "Social talent & soul force"
   - Difficulty: `20 - Presence` or `20 - (Presence × 2)` if trained

7. **Leadership** `[PRS]`
   - Description: "Social prowess & diplomacy"
   - Difficulty: `20 - Presence` or `20 - (Presence × 2)` if trained

8. **Medicine** `[RSN]`
   - Description: "Clinical knowledge & talent"
   - Difficulty: `20 - Reason` or `20 - (Reason × 2)` if trained

9. **Mysticism** `[AWR]`
   - Description: "Supernatural knowledge & rites"
   - Difficulty: `20 - Awareness` or `20 - (Awareness × 2)` if trained

10. **Performance** `[PRS]`
    - Description: "Social chicanery & cultural talent"
    - Difficulty: `20 - Presence` or `20 - (Presence × 2)` if trained

11. **Sneak** `[DEX]`
    - Description: "Physical concealment & chicanery"
    - Difficulty: `20 - Dexterity` or `20 - (Dexterity × 2)` if trained

12. **Survival** `[AWR]`
    - Description: "Natural knowledge & adaptability"
    - Difficulty: `20 - Awareness` or `20 - (Awareness × 2)` if trained

---

### 8. Attacks Section (Repeating Section)

#### Repeating Attack Rows
Use Roll20's `repeating_attacks` section. Each row contains:

1. **Weapon Name** (text input) - e.g., "Longsword", "Dagger"
2. **Attack Type** (dropdown):
   - Melee `[MIT]`
   - Brawl `[MIT]`
   - Ranged `[AWR]`
   - Finesse `[DEX]`
3. **Trained** (checkbox) - affects attack roll difficulty
4. **Grip** (text input) - e.g., "2H", "1H", "V"
5. **Damage** (text input) - e.g., "d8", "2d6", "d10"
6. **Properties** (text input) - e.g., "Brutal, Keen", "Long, Thrown"
7. **Attack Button** - Rolls attack check
8. **Damage Button** - Rolls damage

#### Attack Roll Mechanics
When Attack Button clicked:
1. Show popup: "Does this roll have Favor or Hinder?"
   - Options: Favor / Normal / Hinder
2. Roll attack based on selection:
   - **Normal**: Roll d20
   - **Favor**: Roll d20 + d6
   - **Hinder**: Roll d20 - d6
3. Calculate difficulty based on attack type and training:
   - **Melee**: 
     - Untrained: `20 - Might`
     - Trained: `20 - (Might × 2)`
   - **Brawl**: 
     - Untrained: `20 - Might`
     - Trained: `20 - (Might × 2)`
   - **Ranged**: 
     - Untrained: `20 - Awareness`
     - Trained: `20 - (Awareness × 2)`
   - **Finesse**: 
     - Untrained: `20 - Dexterity`
     - Trained: `20 - (Dexterity × 2)`
4. Compare roll to difficulty:
   - **Critical**: Unmodified d20 = 20 (before Favor/Hinder d6)
   - **Pass**: Total roll ≥ difficulty
   - **Fail**: Total roll < difficulty
5. Output to chat (see Roll Output Format section)

#### Damage Roll Mechanics
When Damage Button clicked:
1. If attack was a **Critical Hit**, add stat damage:
   - Melee/Brawl: Add Might
   - Ranged: Add Awareness
   - Finesse: Add Dexterity
2. Roll damage dice as specified in Damage field
3. Output total damage to chat

---

### 9. Inventory Section (Repeating Section)

#### Repeating Inventory Rows
Use Roll20's `repeating_inventory` section. Each row contains:
- **Item Name** (text input)
- **Slots** (number input, min: 0, default: 1)

#### Inventory Calculations (Display Only)
- **Total Slots Used** (calculated): Sum of all Slots values in inventory rows
- **Max Slots** (calculated): `8 + Might`
- **Display Format**: "Occupied: [used] / Max: [max]"
- **Visual Warning**: If used > max, display in red text

#### Add Item Button
- "+" button to add new inventory row
- Remove row button (X) on each row

---

### 10. Wealth Section

#### Currency Tracking
- **Gold (G)** (number input, default: 0)
- **Silver (S)** (number input, default: 0)
- **Copper (C)** (number input, default: 0)

#### Display
- Show as: "G: [input] S: [input] C: [input]"
- Optional: Show total value in gold (1g = 10s = 100c)

---

### 11. Abilities Section (Repeating Section)

#### Repeating Ability Entries
Use Roll20's `repeating_abilities` section. Each entry contains:
- **Ability Name** (text input)
- **Ability Description** (textarea, multi-line)
- **Post to Chat Button** - Posts ability name and description to chat
- **Remove Button** (X) to delete the ability

#### Add Ability Button
- "+" button or "Add Ability" button to create new ability entry

#### Chat Output Format
When "Post to Chat" is clicked:
```
[Character Name]'s [Ability Name]
[Full ability description]
```

---

### 12. Magic Section

#### Mana Tracking
- **Current Mana** (number input, min: 0, default: 0)
- **Max Mana** (calculated display)
- **Casting Max** (calculated display)

#### Mana Calculations

**Max Mana Formula** (based on Class dropdown):
- Use a class multiplier lookup table
- Formula: `Class Multiplier × Level`

**Class Multiplier Table**:
| Class | Multiplier |
|-------|-----------|
| Alchemist | 2 |
| Barbarian | 0 |
| Bard | 2 |
| Dancer | 2 |
| Druid | 2 |
| Fighter | 0 |
| Gunslinger | 0 |
| Hunter | 0 |
| Luminary | 2 |
| Magus | 2 |
| Merchant | 0 |
| Pugilist | 0 |
| Revelator | 2 |
| Rogue | 0 |
| Sorcerer | 2 |
| Vanguard | 0 |
| Witch | 2 |
| Wizard | 2 |

**Casting Max Formula** (based on Class):
Varies by class spellcasting stat:
- **Arcana casters** (Magus, Wizard): `Reason + (Level ÷ 2, round up)`
- **Mysticism casters** (Druid, Luminary, Witch): `Awareness + (Level ÷ 2, round up)`
- **Influence casters** (Sorcerer): `Presence + (Level ÷ 2, round up)`
- **Leadership casters** (Revelator): `Presence + (Level ÷ 2, round up)`
- **Alchemist/Bard/Dancer**: Use Reason + (Level ÷ 2, round up)
- **Non-casters**: 0

#### Mana Management
- **Spend Mana Button**: Decrements Current Mana by amount specified
- **Rest Button**: Resets Current Mana to Max Mana
- **Display Format**: "Current: [input] / Max: [calculated] (Casting Max: [calculated])"

---

## Dice Rolling System

### Roll Mechanics

#### Favor/Hinder Popup
For ALL rolls (Skills, Saves, Attacks):
1. Display modal/popup when roll button clicked
2. Three options:
   - **Favor** (adds +d6 to roll)
   - **Normal** (just d20)
   - **Hinder** (subtracts d6 from roll)

#### Roll Calculation
1. Roll d20
2. If Favor: Roll d6 and add to d20
3. If Hinder: Roll d6 and subtract from d20
4. Check for Critical (unmodified d20 = 20, before Favor/Hinder)
5. Compare total to difficulty
6. Determine result: Critical / Pass / Fail

#### Result Determination
- **Critical**: Unmodified d20 roll = 20
  - For attacks: Roll damage + add stat damage
  - For saves: Character takes no effect and can use an Action
  - For skills: Success with exceptional outcome
- **Pass**: Total roll ≥ difficulty
- **Fail**: Total roll < difficulty

---

## Roll Output Format (Chat Templates)

### Skill Check Output
```
[Character Name]'s [Skill Name] Check
d20 Roll: [d20 result] [+ d6 Favor: [d6 result]] / [- d6 Hinder: [d6 result]]
Total: [total] vs Difficulty [difficulty]
Result: [CRITICAL! / PASS / FAIL]
```

### Save Output
```
[Character Name]'s [Save Name] Save
d20 Roll: [d20 result] [+ d6 Favor: [d6 result]] / [- d6 Hinder: [d6 result]]
Total: [total] vs Difficulty [difficulty]
Result: [CRITICAL! / PASS / FAIL]
```

### Attack Output
```
[Character Name]'s [Weapon Name] ([Attack Type]) Attack
Attack Roll: [d20 result] [+ d6 Favor: [d6 result]] / [- d6 Hinder: [d6 result]]
Total: [total] vs Difficulty [difficulty]
Result: [CRITICAL! / PASS / FAIL]

[If Pass or Critical]
Damage: [damage dice result] [+ [stat] (stat bonus)] = [total damage] damage
```

**Example Attack Output:**
```
Gareth's Longsword (Melee) Attack
Attack Roll: 15 + Favor d6: 4
Total: 19 vs Difficulty 16
Result: PASS

Damage: d8: 6 = 6 damage
```

**Example Critical Attack Output:**
```
Gareth's Longsword (Melee) Attack
Attack Roll: 20 (CRITICAL!)
Total: 20 vs Difficulty 16
Result: CRITICAL!

Damage: d8: 6 + 4 (Might) = 10 damage
```

---

## Auto-Calculation Sheet Workers

### Required Sheet Workers

1. **Stats Change → Update Derived Values**
   - Might changes → Update Max HP, Inventory Slots, Endure save, Melee/Brawl difficulties
   - Dexterity changes → Update Speed values, Reflex save, Finesse/Sneak difficulties
   - Awareness changes → Update Reflex save, Ranged difficulties, Mysticism/Detect/Survival difficulties
   - Reason changes → Update Will save, Arcana/Craft/Medicine difficulties, Casting Max for Arcana casters
   - Presence changes → Update Will save, Influence/Leadership/Performance difficulties, Casting Max for Influence/Leadership casters
   - Luck changes → Update Max Luck

2. **Level Change → Update Derived Values**
   - Update Max HP
   - Update Max Mana
   - Update Casting Max

3. **Class Change → Update Mana**
   - Update Max Mana based on class multiplier
   - Update Casting Max based on class type

4. **Skill Training Toggle → Update Difficulty**
   - When checkbox toggled, recalculate skill difficulty

5. **Attack Training Toggle → Update Difficulty**
   - When checkbox toggled, recalculate attack difficulty

6. **Inventory Slot Changes → Update Total Used**
   - When item slots change, recalculate total slots used

7. **Character Creation Helper**
   - Update points remaining display when any stat changes
   - Show validation messages

---

## Responsive Design Breakpoints

### Desktop Layout (>768px)
```
┌─────────────────────────────────────────────────────────┐
│ VAGABOND - PULP FANTASY ROLEPLAYING GAME               │
├──────────┬──────────────────────────┬────────────────────┤
│  STATS   │  CHARACTER INFO          │   INVENTORY        │
│  ------  │  ---------------         │   -----------      │
│  MIGHT   │  Name, Ancestry, Class   │   Item  | Slots   │
│  [  2 ]  │  Level, XP, Size, Type   │   ------|------   │
│          │                          │   ...             │
│  DEXTER  │  HIT POINTS & ARMOR      │                    │
│  [  2 ]  │  -------------------     │   Occupied: X/Y    │
│          │  Current: [ ] / Max: [ ] │                    │
│  AWARE   │  Armor: [ ]              │   ---------------  │
│  [  2 ]  │  Fatigue: [ ][ ][ ][ ][X]│                    │
│          │                          │   WEALTH           │
│  REASON  │  SPEED                   │   ------           │
│  [  2 ]  │  -----                   │   G:[ ] S:[ ] C:[ ]│
│          │  Speed: 25 ft + [ ] ft   │                    │
│  PRES    │  Crawl: 75 ft            │   ---------------  │
│  [  2 ]  │  Travel: 5 miles         │                    │
│          │                          │   ABILITIES        │
│  LUCK    │  CURRENT LUCK            │   ---------        │
│  [  2 ]  │  ------------            │   [+] Add Ability  │
│          │  Current: [ ] / Max: 2   │                    │
│          │                          │   [Ability 1]      │
│          │  SAVES                   │   [Ability 2]      │
│          │  -----                   │   ...              │
│          │  Reflex: 16   [Roll]     │                    │
│          │  Endure: 16   [Roll]     │   ---------------  │
│          │  Will: 16     [Roll]     │                    │
│          │                          │   MAGIC            │
│          │  SKILLS                  │   -----            │
│          │  ------                  │   Current: [ ]     │
│          │  [ ] Arcana      16      │   Max: 2           │
│          │  [ ] Brawl       18      │   Casting Max: 3   │
│          │  ...                     │                    │
│          │                          │                    │
│          │  ATTACKS                 │                    │
│          │  -------                 │                    │
│          │  Weapon | Type | Damage │                    │
│          │  --------|------|--------|                    │
│          │  ...                     │                    │
└──────────┴──────────────────────────┴────────────────────┘
```

### Mobile Layout (≤768px)
Stack all sections vertically in order:
1. Header (Character Info)
2. Stats
3. Hit Points & Armor
4. Speed
5. Current Luck
6. Saves
7. Skills
8. Attacks
9. Inventory
10. Wealth
11. Abilities
12. Magic

---

## Implementation Notes

### Roll20 Specific Requirements

1. **Attribute Naming Convention**
   - Use `attr_` prefix for all attributes
   - Use `repeating_sectionname_` for repeating sections
   - Example: `attr_might`, `repeating_attacks_weaponname`

2. **Sheet Workers**
   - Use `on()` event listeners for attribute changes
   - Use `getAttrs()` and `setAttrs()` for calculations
   - Implement in `<script type="text/worker">` tags

3. **Roll Buttons**
   - Use Roll20 button syntax: `<button type="roll" name="roll_skillname">`
   - Include roll template calls
   - Use `@{character_name}` for character reference

4. **Roll Templates**
   - Create custom roll templates for:
     - Skill checks
     - Save checks
     - Attack rolls
     - Damage rolls
     - Ability posts

5. **Input Types**
   - Number inputs: `<input type="number" name="attr_might" value="2" min="2" max="7">`
   - Text inputs: `<input type="text" name="attr_charactername">`
   - Checkboxes: `<input type="checkbox" name="attr_skill_trained" value="1">`
   - Textareas: `<textarea name="attr_ability_description"></textarea>`
   - Selects: `<select name="attr_class">...</select>`

6. **Repeating Sections**
   - Use Roll20's repeating fieldset syntax
   - Include add/remove item buttons
   - Implement sheet workers for repeating section calculations

---

## Validation & Error Handling

### Character Creation
- Prevent stats from being set below 2 or above 7
- Warn if creation points are not fully allocated
- Prevent negative Current HP, Current Mana, Current Luck

### Inventory
- Warn (red text) when slots used exceeds max slots
- Prevent negative slot values

### Mana & Luck
- Current values cannot exceed max values
- Current values cannot be negative

---

## Accessibility Considerations

1. **Labels**: All form inputs should have associated labels
2. **Keyboard Navigation**: All interactive elements should be keyboard accessible
3. **Screen Readers**: Use semantic HTML and ARIA labels where appropriate
4. **High Contrast**: Ensure text is readable against backgrounds
5. **Focus Indicators**: Clear visual focus states for all interactive elements

---

## Testing Checklist

### Functionality Tests
- [ ] All stats update derived values correctly
- [ ] Skill difficulties calculate correctly (trained vs untrained)
- [ ] Attack difficulties calculate correctly based on type and training
- [ ] Save difficulties calculate correctly
- [ ] HP, Mana, and Luck max values calculate correctly
- [ ] Inventory slot totals calculate correctly
- [ ] Speed values calculate correctly based on Dexterity
- [ ] Character creation point allocation works
- [ ] Class selection updates mana calculations

### Roll Tests
- [ ] Skill rolls work with favor/hinder
- [ ] Save rolls work with favor/hinder
- [ ] Attack rolls work with favor/hinder
- [ ] Critical detection works (unmodified 20)
- [ ] Damage rolls work
- [ ] Critical damage adds stat bonus
- [ ] Roll output appears correctly in chat

### UI Tests
- [ ] Responsive layout works at 768px breakpoint
- [ ] All buttons are clickable
- [ ] All inputs accept appropriate values
- [ ] Repeating sections (inventory, attacks, abilities) add/remove correctly
- [ ] Character creation helper toggles correctly
- [ ] All visual elements render correctly

### Edge Cases
- [ ] Stats at minimum (2) and maximum (7)
- [ ] Level 1 and Level 10 characters
- [ ] All classes tested for mana calculation
- [ ] Inventory at max capacity
- [ ] Zero and negative current values handled
- [ ] Empty repeating sections

---

## Files to Deliver

1. **sheet.html** - Main character sheet HTML structure
2. **sheet.css** - Styling for the character sheet
3. **sheet.json** - Roll20 sheet configuration (if needed)
4. **README.md** - Instructions for installing and using the sheet

---

## Future Enhancements (Not Required for Initial Version)

- Spell list tracking (repeating section for known spells)
- Perk tracking (repeating section for perks)
- Quest tracker integration
- NPC/monster stat block variant
- Dark mode toggle
- Export character data
- Import character data
- Automated level-up assistant
- Equipment/armor database with auto-fill

---

## Reference Materials

- Official Vagabond Hero Record PDF (provided)
- Vagabond Core Rulebook PDF (provided for rules reference)
- Roll20 Character Sheet Documentation: https://wiki.roll20.net/Building_Character_Sheets

---

## Class-Specific Mana Implementation Details

### Complete Class Casting Stat Reference

| Class | Max Mana Multiplier | Casting Stat | Casting Max Formula |
|-------|---------------------|--------------|---------------------|
| Alchemist | 2 | Reason | RSN + ⌈Level/2⌉ |
| Barbarian | 0 | N/A | 0 |
| Bard | 2 | Reason | RSN + ⌈Level/2⌉ |
| Dancer | 2 | Reason | RSN + ⌈Level/2⌉ |
| Druid | 2 | Awareness | AWR + ⌈Level/2⌉ |
| Fighter | 0 | N/A | 0 |
| Gunslinger | 0 | N/A | 0 |
| Hunter | 0 | N/A | 0 |
| Luminary | 2 | Awareness | AWR + ⌈Level/2⌉ |
| Magus | 2 | Reason | RSN + ⌈Level/2⌉ |
| Merchant | 0 | N/A | 0 |
| Pugilist | 0 | N/A | 0 |
| Revelator | 2 | Presence | PRS + ⌈Level/2⌉ |
| Rogue | 0 | N/A | 0 |
| Sorcerer | 2 | Presence | PRS + ⌈Level/2⌉ |
| Vanguard | 0 | N/A | 0 |
| Witch | 2 | Awareness | AWR + ⌈Level/2⌉ |
| Wizard | 2 | Reason | RSN + ⌈Level/2⌉ |

Note: ⌈Level/2⌉ means "Level divided by 2, rounded up"

---

## Success Criteria

The character sheet is complete when:
1. All fields are present and functional
2. All calculations work correctly
3. All roll buttons produce correct output in Roll20 chat
4. Responsive layout works at both breakpoints
5. Character creation helper functions correctly
6. No console errors in Roll20
7. Sheet passes all functionality tests
8. Visual design approximates official PDF

---

## End of Specification

This specification provides complete requirements for implementing a fully functional Vagabond character sheet for Roll20. All mechanics, calculations, and UI elements are defined to enable Claude Code to create the necessary HTML, CSS, and JavaScript files.
