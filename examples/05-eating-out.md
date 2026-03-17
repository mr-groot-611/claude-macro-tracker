# Example: Eating Out & Photo-Based Logging

A realistic conversation showing how the meal tracker handles restaurant meals via photo, text estimates, and chain nutrition data. Daily target: 1,500 cal. Weekly budget: 10,500 cal.

---

## Scenario 1: Photo-Based Logging with Confirmation

**User:**
I grabbed lunch at a new Indian place near work. Let me send you a photo.

[User sends a photo of a plate with grilled paneer cubes, white rice, and a dal curry on the side]

**Claude:**
Got it! I can see:
- **Grilled paneer** — looks like about 150g, lightly char-marked
- **White rice** — roughly 1 cup cooked
- **Dal curry** — about 1.5 cups

Before I log this, a couple of quick checks:
- Was the paneer just grilled, or is there a sauce/gravy on it?
- Does the dal look oily to you, or more on the lighter side?

This helps me dial in the macros more accurately.

**User:**
Yeah, the paneer actually came in a cream gravy. And the dal is pretty buttery.

**Claude:**
Perfect — that changes things. Updating:
- **Paneer in cream gravy** — 150g paneer + ~3 tbsp cream gravy
- **White rice** — 1 cup cooked
- **Dal (buttery)** — 1.5 cups

**Estimated macros:**
- Calories: **620**
- Protein: **22g**
- Carbs: **58g**
- Fat: **28g**

*Tagged as estimated — photo-based portions are ±10% accurate. If you weighed the paneer or rice, feel free to correct me later.*

**Logging to Meal Log as "Paneer cream gravy, rice, dal" — estimated**

Look good?

**User:**
Yes, that's right.

**Claude:**
Logged! ✓

---

**Daily Summary**
- Lunch: 620 cal
- **Today so far: 620 / 1,500** (41% of daily target)
- **Week so far: 6,840 / 10,500** (65% of weekly budget — on track)

---

## Scenario 2: Quick Text Estimate for Casual Takeout

**User:**
Grabbed a burger and fries from the place on Main Street for lunch today.

**Claude:**
Got it — I'll estimate a typical burger + fries from a casual spot:
- **Burger** (1/4 lb patty, cheese, bun, mayo): ~500 cal
- **Fries** (medium): ~350 cal

Restaurants use more oil than home cooking, so I'm going generous here.

**Estimated macros:**
- Calories: **850**
- Protein: **28g**
- Carbs: **72g**
- Fat: **45g**

*Tagged as estimated.*

If you know it was smaller/larger or remember toppings, just let me know.

**User:**
That sounds right.

**Claude:**
Logged! ✓

**Heads up:** I see you've logged meals from the burger place on Main Street three times now. Would you like me to save it as a **Simple Item** ("Main St burger & fries — ~850 cal") so next time you can just say "had the usual" and I'll grab it in one step?

**User:**
Yeah, that'd be helpful.

**Claude:**
Done! Saved as a Simple Item. Next time, you can just say "had the burger & fries from Main St" or even "the usual from Main St" and I'll log it instantly — still marked as estimated, but faster.

---

**Daily Summary**
- Lunch: 620 cal
- Dinner: 850 cal
- **Today so far: 1,470 / 1,500** (98% of daily target)
- **Week so far: 8,190 / 10,500** (78% of weekly budget)

---

## Scenario 3: Chain Restaurant with Published Data

**User:**
Grabbed a grande latte from Starbucks this morning.

**Claude:**
Starbucks publishes their nutrition data, so I'm pulling the exact numbers:
- **Grande latte** (2% milk): **190 cal**
- Protein: **13g**
- Carbs: **18g**
- Fat: **7g**

*Logged as measured (Starbucks data).*

**User:**
Perfect.

**Claude:**
Logged! ✓

---

**Daily Summary**
- Breakfast (latte): 190 cal
- **Today so far: 190 / 1,500** (13% of daily target)
- **Week so far: 8,380 / 10,500** (80% of weekly budget)

---

## Key Behaviors Demonstrated

1. **Photo logging with confirmation:**
   - Claude describes what they see and asks clarifying questions (sauce? oil?) before finalizing.
   - Estimates are tagged "estimated" with honest accuracy ±10%.
   - Adjustments happen *before* logging, not after.

2. **Text estimates for casual spots:**
   - Claude goes generous (restaurants use more fat/oil).
   - Tags everything "estimated" unless it's a chain with published data.
   - Offers to save recurring spots as Simple Items for speed.

3. **Published chain data:**
   - Starbucks, McDonald's, Chipotle, etc. use exact published nutrition info.
   - Tagged "measured" for confidence.

4. **Simple Item suggestion:**
   - After 3+ logs from the same spot, Claude proactively offers to save it.
   - Next-time logging is instant, still honest about "estimated" tag.

5. **Realistic macro values:**
   - USDA-aligned estimates for home recipes and estimates.
   - Restaurant meals account for higher fat/oil content.
   - Photo-based portions are acknowledged as approximate.

6. **Daily/weekly status:**
   - Shown after every log.
   - Weekly budget tracked in context.
   - Clear percentage breakdowns.

---

## Limitations Acknowledged in Photo Logging

- **Portion estimates from photos are ±10% accurate** — Claude doesn't claim precision beyond what's possible.
- **Can't detect hidden ingredients** — e.g., oil, butter, cream in sauces — Claude asks about these.
- **Works best with clear plating** — mixed dishes or blended sauces are harder to parse.
- **User confirmation required** — Claude always shows estimates and waits for "yes" before logging.
- **Estimated vs. measured tags** — User sees which macros are guesses vs. published data.
