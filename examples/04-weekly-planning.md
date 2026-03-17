# Weekly Planning: Adjusting for a Restaurant Dinner

This example shows how to use the meal tracker to plan around special meals and social events, balancing flexibility with your weekly budget.

---

## Setup

- **Weekly target:** 9,100 calories
- **Daily baseline:** 1,300 calories
- **Daily minimum:** 1,000 calories
- **Current date:** Wednesday, March 17
- **Upcoming event:** Thursday dinner at an Italian restaurant (Lucia's)
- **Context:** User has logged lightly Monday–Wednesday and has flexibility

---

## Conversation

**User:**
> Hey, I want to plan the rest of this week. There's a work dinner Thursday night at Lucia's—Italian place. I want to actually enjoy it without feeling weird about calories. Can you look at what I've eaten so far this week and figure out a plan?

**Claude:**
> I'll check your logs through today and see what we're working with.
>
> *[checks User Profile for week range; queries Monday–Wednesday meal logs]*
>
> Perfect timing. Here's where you are:
>
> | Day | Logged | Target | Difference |
> |-----|--------|--------|-----------|
> | Monday | 1,050 | 1,300 | –250 |
> | Tuesday | 1,180 | 1,300 | –120 |
> | Wednesday (today) | 1,100 | 1,300 | –200 |
> | **Mon–Wed total** | **3,330** | **3,900** | **–570** |
>
> You're sitting with a **570-calorie natural surplus**. That's great—it means you have real breathing room for Thursday without any scrambling or sacrifice. Here's what I'm thinking:
>
> - **Thursday dinner:** This is the event. You'll probably have an appetizer, main, maybe wine, and yes—dessert sounds natural. Let's give you **1,800 calories** for the day, which means you can enjoy it fully.
> - **Friday, Saturday, Sunday:** We'll dial back a bit on those to keep the week balanced. Since you're already ahead, we can do lighter days (around 1,150–1,200 each) and still land right at your weekly target.
>
> Here's my proposal:
>
> | Day | Daily Target | Notes |
> |-----|--------------|-------|
> | Thursday | 1,800 | Italian dinner—appetizer, main, dessert, drink |
> | Friday | 1,200 | Lighter day; you've got momentum from enjoying Thursday |
> | Saturday | 1,150 | Still comfortable; not restrictive |
> | Sunday | 1,200 | Slightly easier to hit with family cooking |
> | **Thu–Sun total** | **5,350** | *Keeps weekly total at ~8,680 (–420 surplus)* |
> | **Full week** | **9,080** | Within 20 of target |
>
> The idea: you have permission to genuinely enjoy Thursday. No guilt, no "I'll make it back"—just enjoy the dinner and eat lightly Friday without feeling like you're being punished.
>
> Does Thursday at 1,800 feel right, or do you want to shift it?

**User:**
> Actually, make it 2,000. We're probably doing dessert AND we always get bread at Lucia's. I don't want to feel constrained.

**Claude:**
> Absolutely—2,000 is smart. Bread, main, dessert, maybe a cocktail. You'll enjoy it without second-guessing bites.
>
> That means I'm rebalancing the rest. We'll go:
>
> | Day | Daily Target |
> |-----|--------------|
> | Thursday | 2,000 |
> | Friday | 1,150 |
> | Saturday | 1,100 |
> | Sunday | 1,150 |
> | **Total** | **9,100** ✓ |
>
> All four days stay above 1,000 minimum, so nothing feels tight. And Friday–Sunday, you still have your dal and hummus batch active—those both anchor easy, satisfying meals under 500 calories, which gives you tons of room for lunch and snacks.
>
> Ready for me to set these in Notion?

**User:**
> Yeah, do it.

**Claude:**
> *[queries Daily Budget database; updates Thursday, Friday, Saturday, Sunday entries]*
>
> Done. All four days are set. Your weekly view is now locked in—you can see the visual: a generous Thursday surrounded by balanced lighter days. No scramble, no guilt.
>
> And a concrete note: Friday and Saturday, your dal reheats in 3 minutes and pairs with rice or naan for ~450 calories. Your hummus is still fresh—that works as a snack with veggies or pita. Both give you easy, low-friction options for those lighter days.
>
> Enjoy Thursday. You've earned the space.

---

## What This Shows

1. **Reviewing history:** Claude checks actual logs (not targets) to see where the user *actually* is before proposing changes.

2. **Transparent math:** A clear table showing daily and weekly totals, plus the buffer created by earlier lighter days.

3. **No guilt framing:** "You have permission," "no scramble," "you've earned the space"—the tone is forward-looking and practical, not punitive.

4. **Flexibility within bounds:** The user adjusts Thursday up from 1,800 to 2,000; Claude rebalances immediately without drama, showing that all remaining days still stay safely above the minimum.

5. **Lazy context load:** Claude uses active batch data (dal, hummus) to suggest practical meal options for the lighter days—concrete and helpful without being unsolicited.

6. **Emotional design:** The plan centers the event as special and worth enjoying, while maintaining honesty about the weekly envelope. No "cheat day" language, no compensation mentality.

---

## Key Details

- **Weekly totals matter more than daily:** Individual days can be 1,000–2,000+ as long as the week balances. This removes daily anxiety.
- **Surplus → flexibility:** Natural undereating earlier in the week (Monday–Wednesday) is immediately converted into permission for the special meal, not guilt.
- **Active batches as scaffolding:** Mentioning dal and hummus isn't prescriptive; it's offering a resource the user already has, reducing friction for the lighter days that follow.
- **Time-bound planning:** This conversation happens mid-week (Wednesday), giving the user time to adjust if needed before Thursday.
