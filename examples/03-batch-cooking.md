# Batch Cooking Workflows

Example conversations showing how the meal tracker handles batch cooking: stored recipes with deviations and ad-hoc cooking with new batch creation.

---

## Scenario 1: Cooking from a stored recipe with deviations

**User:** I made the hummus but only had 150g chickpeas instead of 200g and skipped olive oil.

**Claude:** Got it. Let me pull your stored Hummus recipe and adjust for what you actually cooked.

Looking up stored recipe...
- **Hummus (stored recipe)**
  - Chickpeas: 200g
  - Tahini: 50g
  - Lemon juice: 30mL
  - Garlic: 3g
  - Olive oil: 15g
  - Water: 30mL
  - Salt: 1g

You made it with **150g chickpeas** (instead of 200g) and **no olive oil** (instead of 15g). I'm creating a new batch with your actual ingredients:

**Hummus Batch (2026-03-17)**
- Chickpeas: 150g | 413 cal | 22g protein | 44g carbs | 8g fat
- Tahini: 50g | 302 cal | 9g protein | 10g carbs | 28g fat
- Lemon juice: 30mL | 9 cal | 0g protein | 3g carbs | 0g fat
- Garlic: 3g | 4 cal | 0g protein | 1g carbs | 0g fat
- Water: 30mL | 0 cal | — | — | —
- Salt: 1g | 0 cal | — | — | —

**Batch total: 728g | 728 cal | 31g protein | 58g carbs | 36g fat**
**Per 100g: 100 cal | 4.3g protein | 8g carbs | 4.9g fat**

Note: The original recipe per-100g was 101 cal (with oil). This batch is slightly lower because the olive oil wasn't used. I'm updating your per-100g macros to reflect what you actually made.

How much did you eat?

**User:** I had 80g.

**Claude:** Logging 80g of Hummus Batch (2026-03-17)...

**Hummus — 80g | 80 cal | 3.4g protein | 6.4g carbs | 3.9g fat**

---

**Daily Summary (2026-03-17)**
| Meal | Calories | Protein | Carbs | Fat |
|------|----------|---------|-------|-----|
| Breakfast | 420 | 18g | 48g | 12g |
| Snack (Hummus) | 80 | 3.4g | 6.4g | 3.9g |
| **Today (so far)** | **500** | **21.4g** | **54.4g** | **15.9g** |
| Daily target | 1,500 | 90g | 150g | 50g |
| Remaining | 1,000 | 68.6g | 95.6g | 34.1g |

---

## Scenario 2: Ad-hoc cook with no recipe

**User:** I just made chana masala — 250g chickpeas, 100g onion, 50g tomato, 10g oil, and spices. About 700g total.

**Claude:** Nice! Let me check on those spices — did you add any tadka oil, ghee, or butter to the spices when you bloomed them, or just dry spices (cumin, coriander, etc.)?

**User:** Just dry spices, no extra oil.

**Claude:** Perfect. Creating your batch now:

**Chana Masala Batch (2026-03-17)**
- Chickpeas: 250g | 688 cal | 37g protein | 73g carbs | 13g fat
- Onion: 100g | 40 cal | 1.1g protein | 9g carbs | 0.1g fat
- Tomato: 50g | 9 cal | 0.4g protein | 2g carbs | 0.1g fat
- Oil (cooking): 10g | 90 cal | 0g protein | 0g carbs | 10g fat
- Dry spices (cumin, coriander, ginger, turmeric): 2g | 6 cal | 0.3g protein | 1g carbs | 0.2g fat

**Batch total: 700g | 833 cal | 39.8g protein | 85g carbs | 23.4g fat**
**Per 100g: 119 cal | 5.7g protein | 12.1g carbs | 3.3g fat**

Would you like me to save this as a recipe template called "Chana Masala" so you can use it next time?

**User:** Yes, save it.

**Claude:** Saved! "Chana Masala" is now in your Recipe Templates. Next time you make it, I can adjust for any ingredient swaps.

How much did you have for dinner?

**User:** 200g.

**Claude:** Logging 200g of Chana Masala Batch (2026-03-17)...

**Chana Masala — 200g | 238 cal | 11.4g protein | 24.2g carbs | 6.6g fat**

---

**Daily Summary (2026-03-17)**
| Meal | Calories | Protein | Carbs | Fat |
|------|----------|---------|-------|-----|
| Breakfast | 420 | 18g | 48g | 12g |
| Lunch | 350 | 16g | 42g | 10g |
| Dinner (Chana Masala) | 238 | 11.4g | 24.2g | 6.6g |
| **Today (so far)** | **1,008** | **45.4g** | **114.2g** | **28.6g** |
| Daily target | 1,500 | 90g | 150g | 50g |
| Remaining | 492 | 44.6g | 35.8g | 21.4g |

---

**Weekly Summary (Mar 11–17, 2026)**
| Day | Calories | Budget | ↓ Remaining |
|-----|----------|--------|------------|
| Mar 11 | 1,520 | 1,500 | -20 |
| Mar 12 | 1,480 | 1,500 | 20 |
| Mar 13 | 1,510 | 1,500 | -10 |
| Mar 14 | 1,490 | 1,500 | 10 |
| Mar 15 | 1,505 | 1,500 | -5 |
| Mar 16 | 1,495 | 1,500 | 5 |
| **Mar 17 (today)** | **1,008** | **1,500** | **492** |
| **Weekly total** | **10,508** | **10,500** | **8** |

You're tracking well — very close to your weekly budget so far.

---

## Key Flow Notes

- **Deviation handling**: Recipe templates stay unchanged; batches capture what was actually cooked
- **Macro lookups**: Claude retrieves macros for chickpeas, onion, tomato, and oil from the Simple Items database (locked in on first use per user profile)
- **Spice clarification**: When fat-bearing spices (tadka, ghee) are possible, Claude asks to confirm caloric additions
- **Saved recipes**: User can opt-in to save ad-hoc batches as templates for future use
- **Per-100g transparency**: Both scenarios show ingredient breakdown and per-100g macros so user can estimate future portions visually
- **Context efficiency**: Batch creation doesn't require profile reload; macros come from Simple Items (cached locally in user context)
