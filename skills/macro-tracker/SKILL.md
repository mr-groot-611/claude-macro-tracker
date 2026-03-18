---
name: macro-tracker
description: >
  This skill should be used when the user mentions "food", "meal", "calorie",
  "macro", "recipe", "weight", "budget", "track", "log", "diet", "protein",
  "carbs", "fat", "TDEE", "deficit", "batch", "what should I eat",
  "how am I doing", "snack", "breakfast", "lunch", "dinner", or "cook".
  It handles meal and macro tracking backed by Notion databases, including
  recipe management, batch cooking, meal logging, daily/weekly budgets,
  weight tracking, and goal management.
version: 1.0.0
---

# macro-tracker

A meal and macro tracking system backed by Notion databases. Handles recipe management, batch cooking, meal logging, daily/weekly budgets, weight tracking, and goal management — all through conversation.

**Trigger keywords:** food, meal, calorie, macro, recipe, weight, budget, track, log, diet, protein, carbs, fat, TDEE, deficit, batch, "what should I eat", "how am I doing", snack, breakfast, lunch, dinner, cook

---

## Database schema reference

All databases are in the user's Notion workspace, prefixed "Macro Tracker - ". The User Profile stores a `database_ids` field (JSON map) containing the Notion IDs of all other databases — making every lookup after startup ID-based. Only the User Profile is ever searched by name.

### 1. User Profile (single row)
| Field | Type | Notes |
|-------|------|-------|
| height, age, sex | number/text | Body stats |
| current_weight, target_weight | number | In user's preferred units |
| activity_level | select | Sedentary / Lightly active / Moderately active / Very active |
| tdee | number | Calculated or from Apple Health |
| preferred_units | select | metric / imperial |
| target_daily_calories | number | Baseline daily target |
| target_protein, target_carbs, target_fat | number | Grams per day |
| deficit_strategy | text | e.g., "500 cal deficit from TDEE of 2000" |
| min_calories | number | Hard floor (e.g., 1200) |
| max_calories | number | Hard ceiling |
| goal_type | select | weight loss / maintenance / muscle gain |
| week_start_day | select | Default: Monday |
| dietary_type | select | omnivore / vegetarian / vegan / pescatarian / other |
| allergies_restrictions | text | e.g., "lactose intolerant, no nuts" |
| foods_to_avoid | text | e.g., "processed sugar, deep fried" |
| cuisine_preferences | text | e.g., "Indian, Mediterranean" |
| cooking_frequency | text | e.g., "batch cooks on Sundays" |
| staple_foods | text | Discovered from usage patterns |
| favorite_recipes | text | Frequently used recipes |
| meal_timing_patterns | text | e.g., "usually skips breakfast" |
| notes | text | Free-form preferences Claude picks up |
| apple_health_connected | checkbox | |
| apple_health_uses | text | e.g., "TDEE, weight sync" |
| onboarding_completed | checkbox | |
| last_updated | date | |
| schema_version | number | For forward compatibility |
| database_ids | text | JSON map of all other DB IDs |

### 2. Recipe Library
| Field | Notes |
|-------|-------|
| recipe_name | e.g., "Hummus (Light)" |
| ingredients_breakdown | Structured rich text: each ingredient with name, grams, cal, P, C, F |
| total_yield_grams | Total batch yield |
| cal_per_100g, protein_per_100g, carbs_per_100g, fat_per_100g | Computed macros |
| preparation_notes | Method/instructions |
| tags | e.g., "high-protein", "snack", "meal-prep" |
| created_date | |
| source_notes | e.g., "modified from online recipe to reduce tahini" |

### 3. Batch Log
| Field | Notes |
|-------|-------|
| batch_name | e.g., "Hummus", "Dal" |
| date_cooked | |
| linked_recipe | Relation to Recipe Library (optional for ad-hoc cooks) |
| actual_ingredients | Structured rich text: same format as Recipe Library |
| total_yield_grams | |
| cal_per_100g, protein_per_100g, carbs_per_100g, fat_per_100g | What portion logging uses |
| status | Active / Finished |
| notes | e.g., "used less tahini than recipe" |

### 4. Simple Items
| Field | Notes |
|-------|-------|
| item_name | e.g., "Boiled egg (1 large)", "Whole milk (100ml)" |
| serving_description | Standard serving size |
| calories_per_serving | |
| protein_per_serving, carbs_per_serving, fat_per_serving | Grams |
| source | "USDA FoodData Central" / "user-specified" / "brand: Amul" |
| created_date | |

### 5. Meal Log
| Field | Notes |
|-------|-------|
| date | |
| meal_type | breakfast / lunch / dinner / snack |
| description | Human-readable summary |
| items_breakdown | Structured rich text: each item with name, qty, source, cal, P, C, F |
| total_calories, total_protein, total_carbs, total_fat | Summed from items |
| precision_tag | tracked / estimated / mixed |
| notes | Optional context |

### 6. Daily Budget
| Field | Notes |
|-------|-------|
| date | |
| planned_calories | Set during weekly planning |
| planned_protein, planned_carbs, planned_fat | Day-specific targets |
| actual_calories, actual_protein, actual_carbs, actual_fat | Computed from Meal Log on each fetch |
| remaining_calories, remaining_protein, remaining_carbs, remaining_fat | Computed: planned minus actual |
| special_notes | e.g., "dinner out tonight — reserved 800 cal" |
| status | under-budget / on-track / over-budget (computed) |
| weekly_budget | Relation to Weekly Budget |

### 7. Weekly Budget
| Field | Notes |
|-------|-------|
| week_start_date | Per user's week_start_day setting |
| weekly_calorie_target | From goals, can be overridden |
| weekly_protein_target, weekly_carbs_target, weekly_fat_target | |
| total_consumed_calories, total_consumed_protein, total_consumed_carbs, total_consumed_fat | Computed from Daily Budgets |
| remaining_calories, remaining_protein, remaining_carbs, remaining_fat | Computed |
| special_events | e.g., "Thursday: dinner out with friends" |
| status | on-track / over / under (computed) |

### 8. Weight Log
| Field | Notes |
|-------|-------|
| date | |
| weight | In user's preferred units |
| moving_avg_7day | Computed by Claude when logging |
| weekly_change | Difference from 7 days ago |
| notes | Optional |
| source | manual / apple-health |

---

## Startup flow

Run this sequence every time the skill triggers:

1. **Find User Profile.** Search Notion for a database named "Macro Tracker - User Profile". This is the only name-based search the system ever does.

2. **Not found → onboard.** Read `onboarding.md` from this skill's directory and run the onboarding workflow. Stop here until onboarding completes.

3. **Found → check status.** Read the single row from the User Profile database.
   - If `onboarding_completed` is false → read `onboarding.md` and continue onboarding from where it left off.
   - If `onboarding_completed` is true → proceed to step 4.

4. **Extract database IDs.** Parse the `database_ids` field (JSON map) to get Notion IDs for all other databases. All subsequent queries use these IDs directly — no name-based searches.

5. **Fetch core context** using the stored IDs:
   - **Today's Daily Budget:** Filter by today's date. If no entry exists, create one using the baseline `target_daily_calories` and macro targets from the User Profile. Use `YYYY/MM/DD (Day)` format for the title (e.g., "2026/03/18 (Tue)") — this sorts correctly in Notion.
   - **This week's Weekly Budget:** Determine the current week's start date using the profile's `week_start_day`. Filter by that date. If no entry exists, create one with target = daily target × 7. Also create Daily Budget entries for each day of the week with baseline targets, using the same date title format.
   - **Today's Meal Log entries:** Filter by today's date.

6. **Recalculate actuals.** Sum today's Meal Log entries to compute actual calories and macros for today's Daily Budget. Never trust stored actual values — always recompute. This handles manual Notion edits gracefully.

7. **Lead with status when relevant.** If the user's message is about logging food or checking their budget, include a brief daily status in the response. If they're asking about recipes or something unrelated to today's budget, skip the status.

---

## Lazy loading rules

Only fetch these when the conversation needs them — not on startup:

| Data | Trigger to fetch |
|------|-----------------|
| Active Batches | User mentions a food that could be a batch item, or asks "what's in my fridge" / "what do I have" |
| Simple Items | User logs a non-batch item (eggs, fruit, coffee, etc.) |
| Recipe Library | User mentions recipes, wants to brainstorm, or asks "what recipes do I have" |
| Weight Log | User logs weight or asks about trends/progress/timeline |
| Historical Meal Log / Daily Budget | User asks about a past date |
| `system-guide.md` (local file) | You encounter an edge case or complex scenario not covered in enough detail here |

---

## Meal logging

This is the most common interaction — handle it well.

### Input parsing
Accept text, photo, or both. Handle single meals and multi-meal messages ("for breakfast I had X, for lunch I had Y" → two separate Meal Log entries with their own meal types).

### Lookup priority (system rule — always follow this order)
1. **Active Batch** — Find the most recent Active batch matching the food name. Use its stored per-100g macros × portion weight.
2. **Simple Items** — Find an exact match in the Simple Items database. Use stored per-serving macros × quantity.
3. **Claude estimate** — Estimate using USDA FoodData Central reference values. For new simple items, save to Simple Items and tell the user: "Based on USDA data, 1 large boiled egg is ~78 cal. I've saved this to your Simple Items for future reference."

### Batch items
Use the batch's stored per-100g macros × portion weight in grams. Example: 50g hummus from a batch at 142 cal/100g = 71 cal.

### Simple items
Use stored per-serving macros × quantity. Example: 2 boiled eggs at 78 cal each = 156 cal.

### Eating out
Estimate generously — restaurants use more oil, butter, and cream than home cooking. Tag as "estimated". Use published chain nutrition data when the user names a specific chain. For recurring spots, offer to save as a Simple Item for quick future logging.

### Photo input
When the user sends a photo of their meal:
- Identify foods and estimate portion sizes visually.
- Present the estimate and ask for confirmation before logging: "Looks like grilled chicken (~150g), rice (~200g), and sautéed vegetables. I'd estimate ~550 cal. Does that look right?"
- Always tag as "estimated" — photo estimates are rough.
- Be upfront about limitations: can't see hidden ingredients (oil, butter, sauces mixed in), portion estimation from photos is approximate.
- Photo + text together gives the best results: "this is dal, about 150g" + photo provides both visual and explicit context.

### Retroactive logging
User can log meals for past dates. Use the specified date for the Meal Log entry and update that date's Daily Budget (not today's). Update the Weekly Budget for whichever week contains that date.

### Logging to Notion
Each meal → one Meal Log row:
- **date** — the date the meal was eaten (not necessarily today)
- **meal_type** — breakfast / lunch / dinner / snack
- **description** — human-readable summary
- **items_breakdown** — structured: each item with name, quantity, source (batch ref / simple item / estimated), cal, P, C, F
- **total macros** — summed from items
- **precision_tag** — "tracked" (all items from batches/Simple Items), "estimated" (eating out, photo, guesses), "mixed" (meal has both)

### After logging
Recalculate the affected day's Daily Budget actuals by re-summing that day's Meal Log entries. Update the Weekly Budget totals.

### Response format
Use this format consistently after logging a meal:

```
Logged [meal type]: [total cal] | [P]g P | [C]g C | [F]g F
- [Item] ([qty], [source]): [cal] | [macros]
- [Item] ([qty], [source]): [cal] | [macros]

Today: [consumed] / [target] cal ([remaining] remaining)
Protein: [x] / [target]g | Carbs: [x] / [target]g | Fat: [x] / [target]g

This week: [consumed] / [target] cal — [days left] days left, [status].
```

---

## Batch creation

### From a stored recipe
Pull the template from the Recipe Library. Apply the user's deviations ("I only had 150g chickpeas instead of 200g"). Create a Batch with actual ingredients and recalculated per-100g macros. The recipe template is never modified.

### Ad-hoc (no recipe)
User provides ingredients + total yield. Calculate per-ingredient macros and per-100g totals. Create a Batch directly. Offer to save as a Recipe template for next time.

### Ingredient macros
For each ingredient, check Simple Items first. If found, use stored macros. If not, estimate via USDA FoodData Central, save to Simple Items. This makes Simple Items the single source of truth for ingredient macros — used both in direct logging and batch calculations.

### Vague ingredients
When the user says "and spices" — ask about potentially caloric additions (oil for tadka, coconut milk, sugar). Dry spices are negligible and don't need clarification.

### Batch lifecycle
- New batches are "Active" from creation.
- If a previous Active batch with the same name exists, auto-mark it "Finished".
- User can explicitly finish a batch: "I finished the hummus."
- Finished batches stay for history but are not used for portion lookups.

---

## Recipe brainstorming

When the user wants to explore or create a recipe:
- Brainstorm with awareness of their current budget remaining and dietary preferences from the profile.
- Show per-ingredient macro breakdowns so they can see where calories come from.
- Suggest ingredient substitutions to hit macro targets.
- Handle scaling: "make it double" → scale ingredient quantities and yield; per-100g macros stay the same.
- When finalized: auto-save to Recipe Library with total yield and per-100g macros.
- Do NOT create a batch — that happens when they actually cook.

---

## Meal editing & corrections

- Find the Meal Log entry by date + meal type (ask user to clarify if ambiguous).
- Support: update portion/quantity, remove an item, delete an entire entry, add a forgotten meal to a past date.
- After any change: recalculate the affected date's Daily Budget and the Weekly Budget.
- Confirm with updated numbers.

---

## Weekly planning

When the user describes upcoming situations ("I have a dinner at an Italian restaurant Thursday with friends, I want to enjoy it"):

1. Check already-logged days this week — surplus from naturally under-eating creates flexibility.
2. Reserve a generous allocation for special events (budget 20-30% above a typical home-cooked equivalent for restaurant meals).
3. Distribute remaining budget across other days, respecting min/max daily calories from profile (hard floor and ceiling).
4. Consider active batches for practical meal suggestions on lighter days.
5. Present a day-by-day plan with reasoning.
6. Create/update Daily Budget entries in Notion for each planned day.
7. Let the user adjust — rebalance when they do.

---

## Goal management

Help the user calculate or recalculate TDEE, deficit/surplus, and macro splits:
- TDEE: use Mifflin-St Jeor equation from their body stats (or Apple Health data if connected).
- Show the math transparently. Let the user adjust.
- Save updated targets to User Profile — all future conversations inherit them.

---

## Weight tracking

When the user logs weight or asks about trends:
- Log to Weight Log: date, weight, source (manual or apple-health).
- Compute and show: today's weight, 7-day moving average, change from last week.
- On request: trend analysis, projected timeline to goal weight.
- If Apple Health is connected, weight can auto-sync.

---

## Weekly review

When the user asks "how did I do this week?" or at the end of the week:
- Summarize: total calories vs target, macro adherence, tracked vs estimated ratio, days over/under.
- Highlight patterns: recurring foods, meal timing, weekday vs weekend differences.
- Offer to set up next week's budget.

---

## Meal suggestions

When the user asks "what should I eat?" or is approaching their daily limit:
- Combine: remaining budget + active batches + recipe library + dietary preferences.
- Suggest specific options with macros, prioritizing what's already cooked (active batches).

---

## Progressive profiling

Periodically notice usage patterns (staple foods logged 3+ times per week, consistent recipe modifications, meal timing patterns) and offer to update the user's profile. Read `system-guide.md` for detailed triggers.

---

## Behavioral rules

### Data honesty
Always state the source of every macro number: "from batch", "from Simple Items", "estimated (USDA)". Never fabricate data. If no batch or simple item match is found, say so and offer to estimate.

### USDA as default estimation source
Use USDA FoodData Central reference values for all macro estimates.

### Over-budget — no guilt
Never show negative remaining. Frame as forward-looking: "You've had X against a Y target. You have Z days left with W remaining." No guilt language, no "unfortunately." If consistently over for several days, suggest adjusting targets — not shaming.

### Under-eating guardrails
- After 2 consecutive days below min calories: gentle check-in. "You've been under [min] cal the past couple of days. Just checking in — is that intentional, or have you been forgetting to log?"
- After 4+ days: more direct concern. Suggest revisiting targets. Never diagnose or label behavior.

### Rest days
Respect "not tracking today" / "cheat day" without judgment. Mark as "untracked" in Daily Budget.

### Encouragement
Brief, factual, occasional (once every week or two). "You've hit your protein target consistently for 2 weeks." Not patronizing.

### Units
Accept any input units (cups, tablespoons, oz, grams). Convert to grams internally. Display in user's preferred system from profile.

### Schema compatibility
Handle missing database fields gracefully. If a field doesn't exist, work with what's available. Offer to add missing fields.

---

## When to read system-guide.md

Read `system-guide.md` from this skill's directory when you encounter:
- An edge case the workflows above don't cover in enough detail
- A complex batch scenario (recipe deviations, ingredient substitution chains)
- A user asking about a workflow that needs more nuance than provided here
- Photo logging combined with unusual foods or ambiguous portions
- Multi-week trend analysis or complex rebalancing scenarios
