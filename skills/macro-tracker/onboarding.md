# Onboarding

This file is read when the User Profile database doesn't exist or when `onboarding_completed` is false. It runs exactly once per user.

---

## Phase 1: Database creation

Create all 8 Notion databases using the Notion MCP's create-database tool. Use these exact names:

1. **Macro Tracker - User Profile**
2. **Macro Tracker - Recipe Library**
3. **Macro Tracker - Batch Log**
4. **Macro Tracker - Simple Items**
5. **Macro Tracker - Meal Log**
6. **Macro Tracker - Daily Budget**
7. **Macro Tracker - Weekly Budget**
8. **Macro Tracker - Weight Log**

### Field definitions per database

Create each database with the fields specified in SKILL.md's schema reference. Use these Notion property types:

**User Profile:**
| Field | Notion Type |
|-------|-------------|
| height | number |
| age | number |
| sex | select (Male / Female / Other) |
| current_weight | number |
| target_weight | number |
| activity_level | select (Sedentary / Lightly active / Moderately active / Very active) |
| tdee | number |
| preferred_units | select (metric / imperial) |
| target_daily_calories | number |
| target_protein | number |
| target_carbs | number |
| target_fat | number |
| deficit_strategy | rich_text |
| min_calories | number |
| max_calories | number |
| goal_type | select (weight loss / maintenance / muscle gain) |
| week_start_day | select (Monday / Tuesday / Wednesday / Thursday / Friday / Saturday / Sunday) |
| dietary_type | select (omnivore / vegetarian / vegan / pescatarian / other) |
| allergies_restrictions | rich_text |
| foods_to_avoid | rich_text |
| cuisine_preferences | rich_text |
| cooking_frequency | rich_text |
| staple_foods | rich_text |
| favorite_recipes | rich_text |
| meal_timing_patterns | rich_text |
| notes | rich_text |
| apple_health_connected | checkbox |
| apple_health_uses | rich_text |
| onboarding_completed | checkbox |
| last_updated | date |
| schema_version | number |
| database_ids | rich_text |

**Recipe Library:**
| Field | Notion Type |
|-------|-------------|
| recipe_name | title |
| ingredients_breakdown | rich_text |
| total_yield_grams | number |
| cal_per_100g | number |
| protein_per_100g | number |
| carbs_per_100g | number |
| fat_per_100g | number |
| preparation_notes | rich_text |
| tags | multi_select |
| created_date | date |
| source_notes | rich_text |

**Batch Log:**
| Field | Notion Type |
|-------|-------------|
| batch_name | title |
| date_cooked | date |
| linked_recipe | relation → Recipe Library |
| actual_ingredients | rich_text |
| total_yield_grams | number |
| cal_per_100g | number |
| protein_per_100g | number |
| carbs_per_100g | number |
| fat_per_100g | number |
| status | select (Active / Finished) |
| notes | rich_text |

**Simple Items:**
| Field | Notion Type |
|-------|-------------|
| item_name | title |
| serving_description | rich_text |
| calories_per_serving | number |
| protein_per_serving | number |
| carbs_per_serving | number |
| fat_per_serving | number |
| source | select (USDA FoodData Central / user-specified / brand) |
| created_date | date |

**Meal Log:**
| Field | Notion Type |
|-------|-------------|
| date | date |
| meal_type | select (breakfast / lunch / dinner / snack) |
| description | title |
| items_breakdown | rich_text |
| total_calories | number |
| total_protein | number |
| total_carbs | number |
| total_fat | number |
| precision_tag | select (tracked / estimated / mixed) |
| notes | rich_text |

**Daily Budget:**
| Field | Notion Type |
|-------|-------------|
| date | date |
| planned_calories | number |
| planned_protein | number |
| planned_carbs | number |
| planned_fat | number |
| actual_calories | number |
| actual_protein | number |
| actual_carbs | number |
| actual_fat | number |
| remaining_calories | number |
| remaining_protein | number |
| remaining_carbs | number |
| remaining_fat | number |
| special_notes | rich_text |
| status | select (under-budget / on-track / over-budget) |
| weekly_budget | relation → Weekly Budget |

**Weekly Budget:**
| Field | Notion Type |
|-------|-------------|
| week_start_date | date |
| weekly_calorie_target | number |
| weekly_protein_target | number |
| weekly_carbs_target | number |
| weekly_fat_target | number |
| total_consumed_calories | number |
| total_consumed_protein | number |
| total_consumed_carbs | number |
| total_consumed_fat | number |
| remaining_calories | number |
| remaining_protein | number |
| remaining_carbs | number |
| remaining_fat | number |
| special_events | rich_text |
| status | select (on-track / over / under) |

**Weight Log:**
| Field | Notion Type |
|-------|-------------|
| date | date |
| weight | number |
| moving_avg_7day | number |
| weekly_change | number |
| notes | rich_text |
| source | select (manual / apple-health) |

### Store database IDs

After creating all 8 databases, collect their Notion IDs. Store them in the User Profile's `database_ids` field as a JSON map:

```json
{
  "recipe_library": "<notion-id>",
  "batch_log": "<notion-id>",
  "simple_items": "<notion-id>",
  "meal_log": "<notion-id>",
  "daily_budget": "<notion-id>",
  "weekly_budget": "<notion-id>",
  "weight_log": "<notion-id>"
}
```

The User Profile's own ID is not stored — it's the anchor found by name on every startup.

### Re-onboarding

If the databases already exist (user re-runs onboarding):
- Connect to the existing databases and read their IDs.
- Check `schema_version` in the User Profile.
- Offer to add any missing fields from the current schema.
- Update the `database_ids` field with current IDs.
- Skip to Phase 2 if the profile already has data, or continue from where it left off.

---

## Phase 2: Profile interview

Run this as a conversation, not a form. Ask in natural groups. If the user volunteers information early (e.g., mentions their weight while describing their goal), absorb it without re-asking.

### Group 1: Goal
"What are you looking to do — lose weight, maintain, or build muscle?"

### Group 2: Body stats
"Tell me a bit about yourself — your age, height, current weight, and target weight. Also, how active would you say you are day-to-day? (Sedentary, lightly active, moderately active, or very active)"

### Group 3: Units
"Do you prefer metric (kg, cm) or imperial (lbs, feet/inches)?"

### Group 4: Dietary restrictions
"Any dietary restrictions I should know about? Things like vegetarian/vegan, allergies, or foods you avoid?"

### Group 5: Optional deep dive
"That's enough to get started! Want to tell me more about your eating habits — things like what cuisines you prefer, how often you cook, staple foods you eat regularly — or should we jump in?"

If they share more, capture:
- Cuisine preferences → `cuisine_preferences`
- Cooking habits → `cooking_frequency`
- Meal patterns → `meal_timing_patterns`
- Staple foods → `staple_foods`
- Common simple items → pre-populate the Simple Items database

### TDEE calculation

Calculate using the Mifflin-St Jeor equation:
- Male: BMR = (10 × weight in kg) + (6.25 × height in cm) − (5 × age) + 5
- Female: BMR = (10 × weight in kg) + (6.25 × height in cm) − (5 × age) − 161
- TDEE = BMR × activity multiplier (Sedentary: 1.2, Lightly active: 1.375, Moderately active: 1.55, Very active: 1.725)

Show the math transparently: "Based on your stats, your estimated BMR is ~1,450 cal and with moderate activity, your TDEE is ~2,250 cal/day."

Let the user adjust if they have a reason (e.g., they know from experience their maintenance is higher/lower).

### Target setting

- **Daily calorie target:** For weight loss, suggest TDEE minus 300-500 cal. For maintenance, suggest TDEE. For muscle gain, suggest TDEE plus 200-300 cal. Let the user choose.
- **Macro split:** Suggest a starting split based on goal:
  - Weight loss: ~40% protein / 30% carbs / 30% fat
  - Maintenance: ~30% protein / 40% carbs / 30% fat
  - Muscle gain: ~30% protein / 45% carbs / 25% fat
  - Convert percentages to grams from the calorie target. Let the user adjust.
- **Min calories:** Suggest 1,200 for women / 1,500 for men as starting defaults. Adjustable.
- **Max calories:** TDEE or slightly above as default.
- **Week start day:** Default to Monday, let the user change.

### Apple Health

If the conversation platform supports Apple Health:
"I can connect to Apple Health for more accurate TDEE based on your actual activity, and to auto-sync your weight if you log it on your phone or watch. Want to set that up?"

If connected, note it in the profile.

### Save profile

Write all collected data to the single User Profile row. Set `last_updated` to today.

---

## Phase 3: First week setup

1. **Create this week's Weekly Budget:**
   - `week_start_date` = the most recent occurrence of the user's `week_start_day` (if today is Wednesday and week starts Monday, use this past Monday)
   - `weekly_calorie_target` = daily target × number of remaining days in the week (including today)
   - Set macro targets proportionally

2. **Create Daily Budget entries** for each remaining day this week (including today):
   - `planned_calories` = the baseline daily target from the profile
   - `planned_protein`, `planned_carbs`, `planned_fat` = from the profile's macro targets
   - `actual_*` fields = 0
   - Link each to the Weekly Budget

3. **Pre-populate Simple Items** if the user mentioned staple foods or common items during the interview. For each mentioned item, estimate macros using USDA FoodData Central values and create an entry.

---

## Wrap-up

1. Set `onboarding_completed` to true in the User Profile.
2. Set `schema_version` to 1.
3. Summarize: "You're all set! Your target is [X] cal/day ([Y]/week). I've set up your databases and this week's budget. Just tell me what you eat and I'll handle the rest."
4. The user is now ready to start logging meals. Return to SKILL.md's normal flow.
