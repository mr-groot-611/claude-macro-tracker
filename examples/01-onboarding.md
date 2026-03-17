# Example 1: First-Run Onboarding

A realistic first conversation where Claude sets up the entire system and interviews the user to build their profile. The user is a 28F looking to lose weight.

---

## Conversation

**User:**
Hey! I want to lose some weight and I've heard this meal tracking thing can help. Where do I start?

**Claude:**
Hey, I'm excited to help! I'll set everything up for you — it's pretty straightforward. First, I'm going to create some Notion databases that'll store your recipes, meals, and budget info. Nothing fancy, you'll be able to browse and edit everything in Notion if you want.

Let me get those set up real quick...

*[Creating 8 Notion databases: User Profile, Recipe Library, Batch Log, Simple Items, Meal Log, Daily Budget, Weekly Budget, Weight Log]*

Perfect — all set up! Now I'd like to learn a bit about you so I can personalize your targets.

Let's start simple: **What's your goal?** Are you looking to lose weight, build muscle, or just maintain where you are?

**User:**
Definitely lose weight. I want to get down to about 60 kg. Right now I'm 68 kg.

**Claude:**
Nice, that's a solid goal — 8 kg is totally achievable. Before I calculate your calorie targets, I need a few more details to estimate your TDEE (how many calories you burn per day).

**Tell me:**
1. **Age?**
2. **Height?** (and do you prefer metric or imperial for tracking?)
3. **Activity level** — are you sedentary, lightly active, moderately active, very active? (e.g., office job vs. office job + regular gym vs. manual labor)

**User:**
I'm 28, 165 cm (so yeah, metric for me), and I'd say moderately active — I don't have a gym routine but I walk to work and move around a lot during the day.

**Claude:**
Perfect, that's helpful. Let me calculate your TDEE.

Using the Mifflin-St Jeor equation:
- **BMR** = (10 × 68) + (6.25 × 165) − (5 × 28) − 161 = **1,370 calories/day** (what you burn at rest)
- **Moderately active multiplier** = 1.55
- **TDEE** ≈ 1,370 × 1.55 = **2,124 calories/day**

So that's your maintenance baseline. To lose weight, we create a deficit. A common approach is 500 cal/day deficit, which gives you ~0.5 kg/week (steady and sustainable).

**Your target would be:**
- **1,500 cal/day** (500 deficit from your TDEE of 2,124)
- **~10,500 cal/week** (same deficit across 7 days for simplicity)

Does that feel reasonable to you?

**User:**
Yeah, that sounds good. So 1,500 a day — that's what I aim for?

**Claude:**
Exactly. 1,500/day is your daily target. It can flex a bit within the week though — maybe 1,400 one day, 1,600 another — but 1,500 is your baseline.

One more thing: **how should I split your macros?** A balanced starting point is:
- **40% protein** = 150g/day (helps with fullness and muscle retention during weight loss)
- **30% carbs** = 113g/day
- **30% fat** = 50g/day

Does that work for you, or do you have a preference?

**User:**
That sounds good. I've heard protein is important for not losing muscle, so yeah, let's go with that.

**Claude:**
Great choice. Now, any dietary preferences or restrictions I should know about? Vegetarian, vegan, allergies, foods you absolutely hate?

**User:**
I'm vegetarian, so no meat. No allergies though.

**Claude:**
Got it. And just to clarify — do you eat eggs and dairy? (chicken eggs, milk, yogurt, paneer, cheese, etc.)

**User:**
Yeah, I eat eggs and dairy. Vegetarian but not vegan.

**Claude:**
Perfect. That opens up a lot of protein options — eggs, yogurt, paneer, lentils, chickpeas, etc.

Last question: **Do you prefer any particular cuisines or have foods you absolutely don't want to eat?** (e.g., "I love Indian food" or "I can't stand mushrooms")

**User:**
Oh, I love Indian food — that's most of what I cook. And yeah, I eat a lot of eggs and dal. Like, almost every day. I don't really have foods I hate, just... keep it simple, you know?

**Claude:**
Dal and eggs — perfect, those are super macro-friendly and easy to track. And I love that you're into Indian food — plenty of room for creative, filling meals there.

Actually, let me ask one more thing since you mentioned it: **Do you batch cook?** Like, do you make a big pot of dal on Sunday and eat it through the week?

**User:**
Yeah! I batch cook on Sundays. I usually make dal, sometimes paneer curry, sometimes chickpea stuff. It makes the week so much easier.

**Claude:**
That's ideal for this system. I'll set it up so when you tell me "I made 2 cups of dal," I calculate the exact macros and lock them in for the whole week. Then whenever you log "I had 150g dal," the macros are consistent.

Alright, I have everything I need! Let me set up your profile...

*[Saving to User Profile]*

**Here's what I've set up:**
- **Goal:** Weight loss (68 kg → 60 kg)
- **Daily target:** 1,500 calories
- **Macro split:** 150g protein / 113g carbs / 50g fat
- **Dietary:** Vegetarian (eggs & dairy OK)
- **Preferences:** Indian food, batch cooks on Sundays, eats dal & eggs regularly
- **Unit:** Metric
- **Activity level:** Moderately active
- **Min calories per day:** 1,200 (floor — you shouldn't go below this)
- **Week starts:** Monday

I've also pre-populated your Simple Items database with eggs and a baseline for dal so they're ready to go. You can adjust those anytime.

Now I'm going to create your first week's budget:
- **Week of March 17–23:** 10,500 calories to split across 7 days (10,500 ÷ 7 = 1,500/day)

**You're all set!** 🎯

Here's how it works from here:
1. **Log meals** — Just tell me what you ate (text, photo, or both) and I'll log it to Notion and show you what's left for the day
2. **Cook batches** — Tell me what you made on Sunday and I'll calculate the macros once
3. **Plan ahead** — If you have a special meal coming up, let me know and I'll adjust your daily targets for the week
4. **Track progress** — Weigh in whenever you want and I'll show you trends

Any questions before we jump in?

**User:**
This is awesome, actually. I think I'm ready to go. What do I do first?

**Claude:**
Nice! A few options:

1. **Log what you ate today** — Tell me breakfast, lunch, dinner, snacks and I'll get today's budget set up
2. **Tell me about your batch plan for Sunday** — If you're thinking about what to cook, we can brainstorm and I'll help with macros
3. **Just start fresh tomorrow** — Sleep on it and jump in tomorrow with a clean slate

What feels right?

**User:**
I've already eaten today, so logging might be a mess. Let's just start fresh tomorrow. I'll make my batch on Sunday and log as I go.

**Claude:**
Perfect plan. You'll find everything in your Notion workspace — User Profile, Recipe Library, all the empty logs ready to go.

When you're ready tomorrow (or whenever you eat), just come back and tell me what you had. I'll handle the rest.

One last thing: **would it help if I walked you through an example?** Like, "Here's what it looks like when you log breakfast and I show you your remaining budget for the day?" Or are you good to figure it out as you go?

**User:**
Nah, I think I can figure it out. But I'll come back if I get stuck!

**Claude:**
Perfect. Good luck, and I'll be here whenever you need me. Let's get you to 60 kg! 💪

---

## What Happened in This Conversation

1. **Database creation:** All 8 Notion databases created automatically
2. **Conversational interview:** Covered goal, body stats (age, height, weight, target, activity), unit preference, dietary restrictions
3. **User volunteered extra context naturally:** Batch cooking on Sundays, eats dal and eggs regularly, loves Indian food
4. **TDEE calculated transparently:** Mifflin-St Jeor formula shown step-by-step (BMR = 1,370, TDEE ≈ 2,124 at 1.55 multiplier)
5. **Targets set:** 1,500 cal/day, 40/30/30 macro split (150P/113C/50F)
6. **User Profile populated:** All preferences, stats, and learned context saved to Notion
7. **Simple Items pre-populated:** Eggs and dal baseline added (user can adjust later)
8. **First week budget created:** March 17–23, 10,500 cal/week (1,500/day)
9. **Min calorie floor set:** 1,200 (health guardrail, prevents dangerous under-eating)
10. **Week start configured:** Monday (user didn't specify, so default applied)
11. **Warm sign-off:** User ready to start logging tomorrow

The onboarding took ~5 minutes of conversation (essentials only, no unnecessary details) and set up the entire system. The user learned about their TDEE, understood their targets, and knew what to expect next. Progressive profiling happens naturally as they log meals and cook batches.
