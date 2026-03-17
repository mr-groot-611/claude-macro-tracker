# System Guide

Detailed workflow instructions, edge cases, and examples that supplement SKILL.md. Read this file on-demand when SKILL.md's instructions don't cover a scenario in enough detail.

---

## Batch workflows

### Scenario A: Brainstorming a new recipe

The user wants to design a recipe with macro awareness.

**Flow:**
1. Check remaining weekly/daily budget to inform suggestions.
2. Pull dietary preferences and restrictions from the User Profile.
3. Brainstorm ingredients with per-ingredient macro breakdowns — show where the calories come from so the user can make informed trade-offs.
4. Iterate on ratios: "Tahini is 59 cal/tbsp — if we halve it and add more lemon juice, we save 90 cal per batch with minimal taste impact."
5. When the user is happy, calculate total batch macros and per-100g macros.
6. Auto-save to Recipe Library. Confirm: "Saved Hummus (Light) to your recipe library — 142 cal per 100g."
7. Do NOT create a batch. That happens when they actually cook.

**Example:**
> **User:** I want to make hummus but keep it lower calorie
>
> **Claude:** You have 3,200 cal left this week across 3 days — plenty of room. Here's a lighter version:
>
> - Chickpeas (200g): 328 cal | 17g P | 55g C | 5g F
> - Tahini (15g, about 1 tbsp): 89 cal | 3g P | 3g C | 8g F
> - Lemon juice (30ml): 7 cal
> - Garlic (5g): 7 cal
> - Olive oil (5ml): 44 cal
> - Salt, cumin: negligible
>
> **Total batch:** ~475 cal | 20g P | 58g C | 13g F for ~300g yield
> **Per 100g:** 158 cal | 7g P | 19g C | 4g F
>
> The biggest calorie lever is tahini — traditional hummus uses 3-4 tbsp. We're at 1 tbsp here. Want to adjust anything?

### Scenario B: Cooking from a stored recipe with deviations

The user cooked something based on a recipe but changed some ingredients.

**Flow:**
1. Pull the recipe template from Recipe Library.
2. Apply the user's deviations: changed quantities, substituted ingredients, skipped ingredients.
3. For each ingredient, look up macros in Simple Items first. If not found, estimate via USDA and save to Simple Items.
4. Recalculate total batch macros and per-100g macros with the actual ingredients.
5. Create a Batch entry with the actual values. The recipe template stays untouched.
6. Ask how much they ate → log the meal.

**Example:**
> **User:** I made the hummus but I only had 150g chickpeas and skipped the olive oil
>
> **Claude:** Got it — I'll adjust from the recipe. Here's your actual batch:
>
> - Chickpeas (150g): 246 cal | 13g P | 41g C | 4g F
> - Tahini (15g): 89 cal | 3g P | 3g C | 8g F
> - Lemon juice (30ml): 7 cal
> - Garlic (5g): 7 cal
>
> **Total batch:** ~349 cal | 16g P | 44g C | 12g F for ~250g yield
> **Per 100g:** 140 cal | 6g P | 18g C | 5g F
>
> Created batch: Hummus (Mar 17) — 140 cal per 100g. How much did you eat?

### Scenario C: Ad-hoc cook without a recipe

The user made something from scratch without following a stored recipe.

**Flow:**
1. Parse the ingredients the user provides.
2. For each ingredient: check Simple Items → if not found, estimate via USDA and save.
3. **Vague ingredients:** If the user says "and spices", ask about potentially caloric additions: "Does that include any oil/ghee for tadka, coconut milk, or sugar? Dry spices like cumin, turmeric, chili powder are negligible." Don't ask about every dry spice individually — just flag the caloric ones.
4. Calculate total and per-100g macros.
5. Create a Batch entry.
6. Offer to save as a Recipe template: "Want me to save this as a recipe for next time?"
7. Ask how much they ate → log the meal.

**Example:**
> **User:** I just made dal. 200g red lentils, 50g onion, 10g ghee, 15g tomato paste, and spices. Made about 600g total.
>
> **Claude:** Quick question — when you say spices, does that include any additional ghee or oil for a tadka, or coconut milk? Dry spices are negligible.
>
> **User:** Yeah I used another 5g ghee for tadka
>
> **Claude:** Got it. Here's your batch:
>
> - Red lentils (200g): 680 cal | 48g P | 120g C | 2g F
> - Onion (50g): 20 cal | 1g P | 5g C | 0g F
> - Ghee (15g total): 134 cal | 0g P | 0g C | 15g F
> - Tomato paste (15g): 13 cal | 1g P | 3g C | 0g F
>
> **Total batch:** ~847 cal | 50g P | 128g C | 17g F for 600g
> **Per 100g:** 141 cal | 8g P | 21g C | 3g F
>
> Created batch: Dal (Mar 17). Want me to save this as a recipe template for next time? How much did you eat?

### Scenario D: Logging portions from an existing batch (days later)

The user eats from a batch they cooked previously.

**Flow:**
1. Identify the food and find the most recent Active batch with a matching name.
2. If multiple Active batches have similar names, ask the user to clarify: "I have two active batches — Hummus from Mar 15 and Hummus (Light) from Mar 12. Which one?"
3. Calculate macros using the batch's per-100g values × portion weight.
4. For any non-batch items in the same meal, follow the standard lookup priority (Simple Items → estimate).
5. Log the meal and update the budget.

**Example:**
> **User:** Had 50g hummus and 2 eggs for lunch
>
> **Claude:** Logged lunch: 227 cal | 16g P | 10g C | 13g F
> - Hummus (50g, batch Mar 15): 71 cal | 4g P | 10g C | 3g F
> - Boiled eggs (2, simple item): 156 cal | 12g P | 1g C | 10g F
>
> Today: 680 / 1,300 cal (620 remaining)
> Protein: 42 / 95g | Carbs: 88 / 140g | Fat: 24 / 45g
>
> This week: 5,200 / 10,500 cal — 3 days left, on track.

### Scenario E: Batch lifecycle and auto-finishing

**Rules:**
- A batch is "Active" from the moment it's created.
- When the user creates a new batch with the same name as an existing Active batch, auto-mark the old one "Finished". Inform the user: "I've marked your previous Dal batch (Mar 10) as finished and created the new one."
- The user can also explicitly finish a batch: "I finished the hummus" → mark it "Finished".
- Finished batches remain in the database for historical reference but are never used for portion lookups.
- When looking up a batch for logging, only search Active batches. If no Active batch matches the food name, fall back to Simple Items → estimate.

**Example:**
> **User:** I made fresh dal today — same recipe as before
>
> **Claude:** I've marked your previous Dal batch (Mar 10) as finished and created a new one.
>
> Dal (Mar 17) — 141 cal per 100g. How much did you eat?

### Scenario F: Recipe scaling

**Rules:**
- Scale ingredient quantities and total yield by the requested factor.
- Per-100g macros stay the same (same recipe, just more of it).
- Show the scaled ingredients for confirmation before creating the batch.

**Example:**
> **User:** I want to make double the hummus recipe
>
> **Claude:** Here's the doubled version:
>
> - Chickpeas: 400g (was 200g)
> - Tahini: 30g (was 15g)
> - Lemon juice: 60ml (was 30ml)
> - Garlic: 10g (was 5g)
> - Olive oil: 10ml (was 5ml)
>
> **Total yield:** ~600g
> **Per 100g:** 158 cal | 7g P | 19g C | 4g F (same as the regular batch)
>
> Want me to create the batch with these amounts?

---

## Eating out

Handle the full spectrum of precision — never block logging because data is imprecise. A rough estimate logged is infinitely more useful than a skipped entry.

### Casual description

> **User:** Had pizza and salad at a restaurant last night
>
> **Claude:** Logged dinner (yesterday): ~850 cal | 28g P | 85g C | 42g F (estimated)
> - Pizza (~2 slices, restaurant-style): ~600 cal | 20g P | 65g C | 30g F
> - Side salad with dressing: ~250 cal | 8g P | 20g C | 12g F
>
> I've budgeted generously — restaurants are typically heavier on oil, cheese, and portions than home cooking. Yesterday is updated.

### Moderate detail

> **User:** Had 2 slices of margherita pizza, a small caesar salad, and a glass of red wine
>
> **Claude:** Logged dinner: ~780 cal | 24g P | 72g C | 34g F (estimated)
> - Margherita pizza (2 slices): ~500 cal | 18g P | 55g C | 22g F
> - Small caesar salad: ~150 cal | 4g P | 10g C | 10g F
> - Red wine (1 glass, ~150ml): ~130 cal | 0g P | 4g C | 0g F
>
> Today: 1,280 / 1,300 cal (20 remaining)

### Known chain restaurant

> **User:** Had the grilled chicken bowl from Sweetgreen
>
> **Claude:** Using Sweetgreen's published nutrition data:
>
> Logged lunch: 490 cal | 38g P | 42g C | 18g F (estimated — chain data)
>
> Today: 820 / 1,300 cal (480 remaining)

### Recurring spot

> **User:** Had lunch at that dosa place again — same as last time, masala dosa and sambar
>
> **Claude:** Logged lunch: ~650 cal | 14g P | 80g C | 30g F (estimated)
>
> You've been here 3 times now. Want me to save "Masala dosa lunch at [place]" as a Simple Item so you can log it in one line next time?

---

## Photo-based logging

### What Claude can see
- Types of food on the plate
- Approximate portion sizes (relative to plate, utensils, or other reference objects)
- Preparation method (grilled, fried, raw)
- General composition (salad vs rice-heavy vs protein-heavy)

### What Claude can't see
- Hidden fats: oil used in cooking, butter melted into food, sauces under/mixed in
- Exact weights: portion estimation from photos is rough (±30-50% for many items)
- Ingredient details: what's in a mixed curry, what spices were used, whether sugar was added
- Density: a bowl of fluffy rice vs packed rice looks similar but weighs differently

### Best practices
- Photo + text = best results. "This is about 200g of dal" + photo gives both visual and weight context.
- Always present the estimate and ask for confirmation before logging.
- Always tag as "estimated".
- If the photo is unclear or ambiguous, say so and ask for help: "I can see rice and what looks like a curry — can you tell me what the curry is?"

### Example

> **User:** [sends photo of a plate]
>
> **Claude:** Looks like grilled chicken breast (~150g), rice (~200g), and sautéed green beans (~80g). I'd estimate:
>
> - Grilled chicken: ~250 cal | 38g P | 0g C | 10g F
> - Rice: ~260 cal | 5g P | 57g C | 0g F
> - Green beans (sautéed): ~70 cal | 2g P | 5g C | 5g F
>
> Total: ~580 cal | 45g P | 62g C | 15g F
>
> Does that look right, or should I adjust anything? (Note: I can't see how much oil was used in cooking — if there was extra, the total might be higher.)

---

## Multi-meal logging

When the user logs multiple meals in one message, parse them into separate Meal Log entries.

### Example

> **User:** For breakfast I had 2 eggs and coffee with milk. For lunch I had leftover dal (150g) and rice.
>
> **Claude:** Logged breakfast: 196 cal | 14g P | 3g C | 12g F
> - Boiled eggs (2, simple item): 156 cal | 12g P | 1g C | 10g F
> - Coffee with whole milk (~50ml, simple item): 40 cal | 2g P | 2g C | 2g F
>
> Logged lunch: 448 cal | 18g P | 63g C | 7g F
> - Dal (150g, batch Mar 15): 212 cal | 12g P | 32g C | 5g F
> - Rice (~150g, simple item): 195 cal | 4g P | 43g C | 0g F
> - Ghee for rice (~5g, simple item): 41 cal | 0g P | 0g C | 5g F — actually, did you add ghee or eat the rice plain?
>
> Today: 644 / 1,300 cal (656 remaining)
> Protein: 32 / 95g | Carbs: 66 / 140g | Fat: 19 / 45g
>
> This week: 4,200 / 10,500 cal — 4 days left, on track.

Note: In the example above, Claude asks about the ghee because it's a common addition to rice in Indian cooking and would significantly affect the macros. This is the kind of contextual awareness that makes tracking accurate without being annoying.

---

## Retroactive logging

When the user logs a meal for a past date:

1. Create the Meal Log entry with the specified past date.
2. Fetch or create the Daily Budget for that date.
3. Recalculate that date's Daily Budget actuals from all Meal Log entries for that date.
4. Determine which week the date falls in. Update that week's Weekly Budget.
5. If the date is from a previous week, fetch/find that week's Weekly Budget and update it.

### Example

> **User:** I forgot to log yesterday's dinner — I had chicken curry from the batch and 2 rotis
>
> **Claude:** Logged dinner (yesterday, Mar 16): 520 cal | 32g P | 45g C | 22g F
> - Chicken curry (200g, batch Mar 14): 340 cal | 28g P | 10g C | 20g F
> - Roti (2, simple item): 180 cal | 4g P | 35g C | 2g F
>
> Yesterday's updated total: 1,180 / 1,300 cal (120 remaining)
> This week: 5,380 / 10,500 cal — 3 days left, on track.

---

## Meal editing

### Update portion

> **User:** Actually that hummus was 100g not 50g
>
> **Claude:** Updated lunch — hummus changed from 50g to 100g:
>
> Logged lunch (updated): 298 cal | 20g P | 19g C | 16g F
> - Hummus (100g, batch Mar 15): 142 cal | 8g P | 19g C | 6g F
> - Boiled eggs (2, simple item): 156 cal | 12g P | 1g C | 10g F
>
> Today: 751 / 1,300 cal (549 remaining)

### Remove an item

> **User:** Remove the roti from my lunch, I didn't eat it
>
> **Claude:** Removed roti from today's lunch.
>
> Logged lunch (updated): 340 cal | 28g P | 10g C | 20g F
> - Chicken curry (200g, batch Mar 14): 340 cal | 28g P | 10g C | 20g F
>
> Today: 680 / 1,300 cal (620 remaining)

### Delete entire entry

> **User:** Delete my lunch entry, I logged it twice by accident
>
> **Claude:** Deleted the duplicate lunch entry. Today: 340 / 1,300 cal (960 remaining).

---

## Weekly planning

### Full planning flow

> **User:** I have a dinner at an Italian restaurant Thursday with friends. I want to enjoy it without worrying. How should I eat this week?
>
> **Claude:** Let me look at your week so far.
>
> Mon-Tue you ate 2,200 cal total (averaging 1,100/day against your 1,300 target) — that gives you 200 cal of surplus already.
>
> Here's a plan for Wed-Sun:
>
> | Day | Target | Notes |
> |-----|--------|-------|
> | Wed | 1,100 | Lighter day — dal + salad for dinner |
> | Thu | 1,800 | Italian dinner — enjoy it! I'm budgeting for a pasta, shared appetizer, and wine |
> | Fri | 1,100 | Lighter day to offset |
> | Sat | 1,200 | Normal |
> | Sun | 1,200 | Normal |
>
> **Week total:** 8,800 / 9,100 cal (target) — leaves a small buffer.
>
> Your minimum is 1,000/day so even the light days are comfortable. Want me to set these targets?

### Rebalancing when user adjusts

> **User:** Actually make Thursday 2,000, I think we'll have dessert too
>
> **Claude:** Updated! I've trimmed a bit from Fri-Sun to compensate:
>
> | Day | Target | Change |
> |-----|--------|--------|
> | Thu | 2,000 | +200 |
> | Fri | 1,050 | -50 |
> | Sat | 1,100 | -100 |
> | Sun | 1,100 | -100 |
>
> Week total: 8,750 / 9,100 — still within budget. All days stay above your 1,000 minimum. Set these?

### Accounting for surplus from under-eating

When building a weekly plan, always check what's already been logged. Days where the user naturally ate under their target create surplus that can be redistributed. Frame this positively: "You've been lighter the first half of the week, which gives you more flexibility for Thursday."

---

## Weekly review

### What to include

When the user asks "how did I do this week?" or when it's the last day of the week:

1. **Budget summary:** Total consumed vs target, with percentage.
2. **Macro adherence:** How close to protein/carbs/fat targets on average.
3. **Precision breakdown:** X tracked meals, Y estimated meals — higher tracked ratio = more reliable data.
4. **Daily pattern:** Weekday average vs weekend average, any standout days.
5. **Recurring foods:** What they ate most often this week.
6. **Weight trend:** If they logged weight, show the weekly change.
7. **Forward-looking:** Offer to set up next week.

### Example

> **User:** How did I do this week?
>
> **Claude:** Here's your week in review:
>
> **Budget:** 8,400 / 9,100 cal (92%) — nicely under target.
> **Protein:** Averaged 88g/day vs 95g target — close, could push protein a bit.
> **Carbs:** 135g/day avg vs 140g target — right on.
> **Fat:** 38g/day avg vs 45g target — naturally eating less fat.
>
> **Tracking quality:** 18 tracked meals, 3 estimated (the restaurant dinners). 86% tracked — solid.
>
> **Pattern:** Weekdays averaged 1,100 cal, weekends 1,400. Thursday's Italian dinner was the highest day at 1,800 — and you planned for it.
>
> **Top foods:** Dal (4 times), eggs (6 times), roti (5 times). Your dal batch got a serious workout.
>
> **Weight:** 65.0 → 64.7 kg this week (7-day avg). Down 0.3 kg — steady progress.
>
> Want me to set up next week with the same daily target, or adjust anything?

---

## Progressive profiling

### When to suggest profile updates

- **Staple food detection:** When a food is logged 3+ times in a single week, offer to add it to `staple_foods` if not already there. "I notice you eat dal almost every day — want me to mark it as a staple in your profile?"
- **Preference inference:** If the user consistently modifies recipes the same way (always reduces oil, always adds extra protein), note the pattern. "You've been cutting oil in your last few batches — want me to note that as a preference?"
- **Meal timing patterns:** If the user consistently skips breakfast or eats late dinners, offer to update `meal_timing_patterns`. Don't flag this if it's occasional — wait for a clear pattern over 2+ weeks.
- **Periodic check-in:** Every 3-4 weeks, offer a brief profile review: "Based on the last few weeks, I've noticed a few things about your habits. Want me to update your profile?"
- **Explicit requests:** The user can always say "update my profile" and Claude updates immediately.

### Tone for profile suggestions
Keep it casual and optional. Never make it feel like surveillance. "I've noticed X — want me to note that?" is fine. "I've been tracking that you always do X" is not.

---

## Recurring restaurant meals

When the user eats at the same place 2-3 times:
- Offer to save the typical order as a Simple Item: "You've been to [place] a few times now. Want me to save 'Usual at [place]' (~650 cal) so you can log it in one line?"
- The Simple Item stores the estimated macros from the most recent visit.
- Future logging: "Had my usual at [place]" → instant log from Simple Items.
- The user can always override: "Had my usual at [place] but also added a dessert."

---

## Vague ingredient handling

**Ask about:**
- Oil/ghee for cooking (tadka, sautéing, roasting) — these add significant calories
- Coconut milk or cream — calorie-dense
- Sugar or honey — often added to Indian dishes, sauces
- Butter or cheese — sometimes mixed into dishes
- Nuts or seeds as toppings

**Don't ask about:**
- Dry spices (cumin, turmeric, chili powder, garam masala, etc.) — negligible calories
- Salt and pepper
- Fresh herbs (cilantro, mint, basil)
- Vinegar, lime/lemon juice (small amounts)

**Phrasing:** Keep it natural and non-intrusive. "Does that include any oil or ghee for cooking?" Not: "Please itemize all fats and oils used in preparation."

---

## Batch name matching

When the user mentions a food that could be from a batch:

1. Search Active batches for a name match (case-insensitive, partial match OK — "dal" matches "Dal" or "Toor Dal").
2. If exactly one match → use it.
3. If multiple matches → ask: "I have two active batches — Toor Dal (Mar 15) and Moong Dal (Mar 12). Which one?"
4. If no match → fall through to Simple Items → estimate.
5. Never guess silently when multiple batches could match. Always ask.
