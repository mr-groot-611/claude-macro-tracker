# claude-macro-tracker

A Claude-powered meal and macro tracking system backed by Notion databases. Track your meals, recipes, batches, and weekly calorie budgets — all through conversation.

## What this is

A custom Claude skill that turns Claude into a persistent meal tracker. Tell Claude what you ate, and it logs everything to Notion with full macro breakdowns, tracks your daily and weekly calorie budgets, manages your recipes and batch cooks, and learns your habits over time. No app to install — the conversation is the interface, Notion is your data layer.

## Key capabilities

- **Recipe & batch management** — Design recipes collaboratively with per-ingredient macro breakdowns. When you cook, create a batch with actual ingredients. Log portions against batches for precise tracking.
- **Smart meal logging** — Text or photo input. Automatic macro lookup from your batches and saved items. USDA-aligned estimates for everything else.
- **Flexible budgeting** — Daily targets that flex within a weekly envelope. Plan ahead for restaurant dinners, adjust on the fly, and let natural under-eating create room.
- **Weight tracking** — Daily logs with 7-day moving averages and projected timelines to your goal weight.
- **Eating-out estimation** — Generous restaurant estimates tagged by precision level. Save recurring spots for quick logging.
- **Progressive personalization** — Claude learns your staple foods, cooking patterns, and preferences over time.

## How it works

The system uses a three-layer recipe model: **Recipe templates** (for planning) → **Batches** (what you actually cooked, with real macros per 100g) → **Meal Log** (portions eaten, referencing batches). This means when you log "50g hummus" on Tuesday, the macros come from the specific hummus you cooked on Sunday — not a generic estimate.

Eight Notion databases store everything: User Profile, Recipe Library, Batch Log, Simple Items, Meal Log, Daily Budget, Weekly Budget, and Weight Log. Claude reads and writes to these via the Notion MCP.


## Prerequisites

- **Claude account** (Pro recommended for higher usage limits; Free works)
- **Notion account** (free plan works fine)
- **Notion MCP** connected to Claude ([setup guide](https://claude.com/connectors/notion))

## Setup

1. Connect Notion to Claude ([setup guide](https://claude.com/connectors/notion)) — the skill needs this to store your data
2. Download `macro-tracker-skill.zip` from [Releases](https://github.com/varun-atul/claude-macro-tracker/releases)
   > **Note:** If your browser automatically unzips the file (common in Safari), right-click the download link and choose **"Download Linked File"** instead.
3. In Claude Desktop, go to **Customize → Skills → + → Upload a skill** and select the zip file
4. Start a conversation on desktop or mobile — say something like "I want to start tracking my meals" and Claude handles onboarding

## What happens on first run

Claude runs a ~5 minute onboarding:
1. Creates all 8 Notion databases in your workspace automatically
2. Interviews you conversationally: goals, body stats, dietary preferences
3. Calculates your TDEE and suggests daily calorie/macro targets
4. Sets up your first weekly budget

After that, just tell Claude what you eat and it handles the rest.

## Example

> **You:** I had 2 eggs and toast with butter for breakfast
>
> **Claude:** Logged breakfast: 350 cal | 18g P | 25g C | 20g F
> - Boiled eggs (2, simple item): 156 cal | 12g P | 1g C | 10g F
> - Toast with butter (1 slice + 10g butter, simple item): 194 cal | 4g P | 24g C | 10g F
>
> Today: 350 / 1,300 cal (950 remaining)
> Protein: 18 / 95g | Carbs: 25 / 140g | Fat: 20 / 45g
>
> This week: 4,550 / 9,100 cal — 3 days left, on track.

More examples in the [examples/](examples/) folder.

## License

MIT — see [LICENSE](LICENSE).
