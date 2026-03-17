# Example: Meal Logging Workflows

This example shows a realistic single-conversation meal logging session. The user is a few days into their week (today is Wednesday), with a daily target of 1,500 cal and a weekly target of 10,500 cal. Several active batches are available (Dal, Hummus, Rice), and simple items are referenced for common foods.

---

## Conversation transcript

**User:** Had 100g dal for lunch.

**Claude:** Logged lunch: 141 cal | 7g P | 20g C | 3g F
- Dal (100g, active batch): 141 | 7g P | 20g C | 3g F

Today: 141 / 1,500 cal (1,359 remaining)
Protein: 7 / 50g | Carbs: 20 / 150g | Fat: 3 / 50g

This week: 3,248 / 10,500 cal — 4 days left, on track.

---

**User:** Had 2 eggs and coffee with milk for breakfast.

**Claude:** Logged breakfast: 217 cal | 14g P | 2g C | 18g F
- Boiled egg, large (2, Simple Items): 156 | 12g P | 1g C | 12g F
- Whole milk, 50ml (0.5 cup, Simple Items): 32 | 2g P | 2g C | 2g F
- Black coffee, brewed (8oz, Simple Items): 2 | 0g P | 0g C | 0g F

Today: 358 / 1,500 cal (1,142 remaining)
Protein: 23 / 50g | Carbs: 22 / 150g | Fat: 21 / 50g

This week: 3,365 / 10,500 cal — 4 days left, on track.

---

**User:** For dinner I had about 150g rice with the dal, and as a snack earlier I ate some hummus with a few crackers.

**Claude:** Logged snack: 284 cal | 8g P | 28g C | 14g F
- Hummus (80g, active batch): 227 | 7g P | 18g C | 12g F
- Whole wheat crackers (10 crackers, Simple Items): 57 | 2g P | 10g C | 2g F

Logged dinner: 318 cal | 7g P | 66g C | 2g F
- Dal (150g, active batch): 212 | 10g P | 30g C | 4g F
- White rice, cooked (150g, Simple Items): 195 | 4g P | 43g C | 0g F

Today: 960 / 1,500 cal (540 remaining)
Protein: 40 / 50g | Carbs: 116 / 150g | Fat: 37 / 50g

This week: 4,747 / 10,500 cal — 4 days left, on track.

---

**User:** Actually, that dal at dinner was 150g not the 100g I logged at lunch. I need to fix that.

**Claude:** Got it — let me update the lunch dal from 100g to 150g.

Updated lunch: 212 cal | 10g P | 30g C | 4g F
- Dal (150g, active batch): 212 | 10g P | 30g C | 4g F

Today: 1,020 / 1,500 cal (480 remaining)
Protein: 43 / 50g | Carbs: 136 / 150g | Fat: 37 / 50g

This week: 4,807 / 10,500 cal — 4 days left, on track.

---

**User:** Can I have a snack before bed? I've got 480 calories left.

**Claude:** You have 480 cal remaining, so you have flexibility. A few quick options from your active batches and Simple Items:

- **Hummus + crackers** (if you haven't finished the portion from earlier): another 80–100g hummus with ~10 crackers = ~310 cal, hits protein well
- **Apple with peanut butter**: 1 medium apple (~95 cal) + 2 tbsp peanut butter (~190 cal) = ~285 cal, keeps you satisfied before bed
- **Yogurt with granola** (if you have these): 150ml plain yogurt (~65 cal) + 40g granola (~180 cal) = ~245 cal
- **Banana with almonds**: 1 medium banana (~105 cal) + 23 almonds (~160 cal) = ~265 cal

What sounds good?

---

**User:** I'll have an apple with a bit of peanut butter, maybe 1.5 tbsp.

**Claude:** Logged snack: 259 cal | 7g P | 20g C | 17g F
- Apple, medium (1, Simple Items): 95 | 0g P | 25g C | 0g F
- Peanut butter (1.5 tbsp, Simple Items): 165 | 7g P | 6g C | 14g F

Today: 1,279 / 1,500 cal (221 remaining)
Protein: 50 / 50g | Carbs: 156 / 150g | Fat: 54 / 50g

This week: 5,066 / 10,500 cal — 4 days left, on track.

**Note:** You've hit your protein target exactly and are 6g over on carbs and 4g over on fat — very balanced day. You have 221 cal remaining if you want anything else, but you're in great shape.

---

## Key patterns shown

1. **Batch item lookup** (dal): User says "had 100g dal" → Claude finds the active Dal batch, applies per-100g macros (141 cal/100g), logs the entry with full macro breakdown.

2. **Simple items** (eggs, milk, crackers, apple, peanut butter): Common, pre-stored items with serving sizes. User specifies quantity → Claude multiplies by quantity.

3. **Multi-meal message** (snack + dinner in one message): Claude creates two separate Meal Log entries with their own meal types.

4. **Correction workflow** (dal quantity update): User provides corrected portion size. Claude refetches the Meal Log entry, recalculates, and shows updated totals for the day and week.

5. **Response format consistency**: Every log includes the standard template — meal type, total cals/macros, itemized breakdown with sources, daily status, and weekly status with days remaining.

6. **Budget awareness**: Claude offers suggestions based on remaining calories and shows how each logged meal affects remaining budget.

7. **Macro precision tracking**: Shows both remaining and total values so the user can see if they're hitting targets. When user goes slightly over, framing is forward-looking and non-judgmental.
