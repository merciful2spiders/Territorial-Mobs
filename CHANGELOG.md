# Changelog

- Fixed campsite garrisons disappearing after players traveled away: campsite pillagers are now persistent, and bounded placement retries reliably attempt the full configured guard count.
- Added rare deterministic pillager campsites in newly explored Overworld terrain, with a tall-smoke campfire, weathered ground, seats, lean-to and bedrolls, supplies, target practice, a named captain, configurable guards, settlement pressure/rumors, terrain safeguards, and bounded one-candidate-per-second generation.
- Fixed the first live war-party test: newly registered settlements now refresh population immediately, eligible villages can muster a bounded share of residents while retaining home defense, the first patrol need not wait three days, materialization is announced, and party entity lookup uses stable UUIDs instead of large repeated AABB scans.
- Reduced exploration stalls by delaying and shrinking ruined-portal structure searches, preserving the sparse search deadline across chunk movement instead of rescheduling a synchronous lookup every few seconds.
- War-party chat questions now use authoritative saved party data; invalid two-word echoes such as `war party` are rejected.

- Added the first functional settlement war-party release: versioned world persistence, named abstract members, border patrol/threat response/retaliation missions, conservative settlement-derived creation, bounded hybrid materialization, local grievance/disposition, staged deterministic warnings, direct-attack escalation, bounded pursuit/retreat, casualties and settlement outcomes, explicit restitution/surrender, war ledger data, player/admin commands, configuration, and manual tests.

- Gold blocks now count as protected Nether gold: breaking one alerts visible zombified piglins and other piglin defenders within 32 blocks and feeds the existing conflict/corruption-pressure path.

- Fixed stale dialogue-topic inheritance observed with Wren: a new personal question no longer inherits the previous settlement topic, while genuine pronoun-based follow-ups still retain context.

- Fixed villager buy offers that still paid emeralds. Emerald result stacks now pass through the same configurable nugget/ingot conversion as emerald costs while retaining offer uses, demand, discounts, stock, XP, and restocking behavior.

- Fixed Wren's observed chat flow: discussing walls no longer implies a construction order, pronoun-based follow-ups retain the settlement topic, a recruit's bare name gets an acknowledgment, and "come here" now uses the authoritative follow order.

- Fixed settlement chat intent routing so wall proposals, yes/no follow-ups, and material questions no longer repeat the generic settlement status report; replies now use the actual active project, builder count, and 20 timber/10 stone project threshold.

Notable changes to Territorial Mobs are recorded here. The project uses semantic versioning where practical.

## Unreleased - Phase A: Settlement Ledger

### Added

- Added the server-safe `/mil ledger` interface with Company, Settlement, Chronicle, Projects, Territory, and War Council sections.
- Added bounded settlement score breakdowns for security, morale, and prosperity.
- Added paginated chronicle filters and persistent pinning with `/mil ledger pin <newest-index>`.
- Added `ledger.enabled` and `ledger.pageSize` common configuration options.

### Persistence and compatibility

- Settlement SavedData is now version 4. Existing chronicle entries infer a safe category and default to unpinned; unknown categories fall back to `GENERAL`.
- Chronicle storage remains bounded at 24 entries and prefers retaining pinned entries when trimming.

### Known limitations

- This first slice uses chat components and commands instead of a custom client screen. Unloaded recruits remain counted but detailed live health, equipment, location, and inventory appear only while their entity is loaded.
- Medicine, tools, gold, knowledge, project damage, known-site intelligence, campaigns, missions, and captives are labeled as untracked rather than presented as implemented.

## Unreleased - Relationship-driven recruit dialogue

### Added

- Every recruit now has one deterministic, persistent motivation and a concise motivation summary used by dialogue.
- Added derived relationship states from friendship, respect, grievance, temperament, flaw, and trust friction.
- Added a bounded per-player conversation summary containing the last topic, tone, unresolved thread, recent count, social action, cooldown, and daily effect totals.
- Added conservative deterministic interpretation for greetings, thanks, praise, apologies, insults, threats, promises, work questions, safety concerns, travel invitations, help requests, and authoritative orders.
- Dialogue can apply small explainable relationship changes with a configurable cooldown, separate positive/negative daily caps, and repeated-action suppression.
- Ollama prompts now receive motivation, relationship state, continuity, social interpretation/effect, settlement state, player standing, and up to three grounded relationship references.
- Fallback dialogue now varies by relationship state and handles resentment, distrust, apology, insult, thanks, and reluctant accepted orders without Ollama.
- Added operator-only `/mil dialogue-debug` structured state display and deterministic policy checks.

### Fixed

- Generated schema/keyword labels such as `farmer_farming_producing_crops_farm_food` are now rejected and replaced by relationship-aware fallback dialogue.
- Low-confidence follow-up messages no longer erase the prior conversation topic and emotional tone.
- Travel phrases such as `saddle up` and `you're coming` retain travel context, and tense recruits cannot abruptly claim eager compliance unless an authoritative order was accepted.
- Work-question fallback dialogue now reflects farmer, builder, blacksmith, guard, and militia roles when Ollama returns malformed JSON.
- Single schema labels such as `greeting` are rejected as generated speech.
- Crop questions are classified as supplies discussions and fall back to truthful checks of the recruit's carried inventory.
- Work fallback recognizes villager profession as well as assigned settlement role.
- Structured Ollama output allowance increased from 32 to 64 tokens to prevent truncated JSON responses.
- Generated replies containing only the recruit's own name are rejected, while ordinary sentences may still mention names.
- Inventory and seed-diet questions now bypass generated dialogue and use authoritative carried-inventory or planting answers.
- Unsupported claims that crop supplies `just arrived` or that villagers eat seeds as meals are rejected.
- Factual work, settlement, resource, and other-recruit questions now use authoritative identity, inventory, relationship, and SettlementData answers instead of generated guesses.
- One-word labels, perspective-flipped player promises, and replies longer than 40 words are rejected.
- Low-confidence follow-up questions inherit the existing topic, while likely gibberish receives a clear request to rephrase.

### Persistence and compatibility

- Villager identity NBT is now version 7. Version 6 and older recruits load with a deterministic motivation and empty per-player conversation state.
- Conversation records are capped at 16 players per recruit; summaries are capped at 120 characters and recent counts/effect totals are clamped.

### Deferred

- Structured promises and fulfillment are deferred until each promise has a reliable authoritative gameplay hook.
- Unsolicited villager initiative is deferred rather than adding a recurring scan or disruptive login chatter.
- Relevant villagers are currently represented by grounded UUID/relationship summaries; names are not invented when the related entity is unloaded.

## [1.0.0] - 2026-06-20

### Added

- Villager trade costs now use configurable gold nuggets or gold ingots instead of emeralds, preserving vanilla offers, outputs, discounts, demand, stock, restocking, and completed-trade reputation rewards.
- Players at their current follower limit can deliberately offer configurable gold payments for a small number of extra persistent recruits.
- Renown tier thresholds are lower, making follow, hold, labor, and structure orders arrive earlier.
- Added `/mil transfer` and matching spoken transfer phrases so recruits can be rehomed into player-owned or sworn-protector settlements.
- Nether gold ore and gilded blackstone extraction now feed the existing Nether conflict/corruption-pressure path without scanning player inventories or punishing ordinary gold spending.
- Persistent recruited-villager identities with names, traits, competencies, flaws, morale, experience, relationships, memories, and corruption responses.
- Villager inspection, recruitment, militia formation, role assignment, follower directions, work orders, tree felling, and builder-only construction.
- Renown titles, follower limits, settlement trust, protection status, persistent chronicles, and right-click-air status reporting.
- Event-driven renown gains for hostile mobs defeated near recorded or vanilla villages.
- Settlement roles, daily production, security, prosperity, stores, faction pressure, autonomous projects, bounded offline catch-up, rumors, leadership, and succession.
- Veteran service records, veteran titles, hero memorials, portal opinions, and personality-aware dialogue.
- Optional asynchronous Ollama dialogue using `llama3.2:1b`, with sanitization, cooldowns, timeouts, name targeting, contextual prompts, and varied fallback responses.
- Pillager retaliation and separate Nether retaliation escalation.
- More frequent pillager outposts and ruined portals in newly generated chunks.
- Ruined-portal incursion sites with horizontal proximity activation, Nether mob spawning, terrain corruption, and recurring cooldowns.
- Destructive Nether terrain spread with sparse Nether gold ore and zombified-piglin deposit defense.
- Safe first-login placement near a living village and initial respawn assignment.

### Persistence and compatibility

- Villager identity and work state use versioned persistent entity NBT.
- Renown uses server-owned player persistent data.
- Settlement state uses versioned dimension `SavedData` with bounded histories.
- Missing fields and older recruited villagers load with safe defaults.

### Known limitations

- Named raid commanders, rival warlords, capture and rescue, and player-mediated settlement disputes remain planned future slices.
- World-generation frequency changes only affect newly generated chunks.
- Ollama and its language model must be installed separately for generated dialogue.
- Model-suggested intentions and memories are descriptive; the model cannot directly execute commands or mutate the world.
