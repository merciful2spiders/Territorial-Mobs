# Territorial Mobs

## Pillager campsites

Rare pillager campsites can now appear while exploring newly generated Overworld chunks. Each terrain-safe camp has a tall-smoke campfire, worn coarse-dirt and podzol ground, log seats, a gray-wool lean-to with bedrolls, a supply barrel, an archery target, a warning torch, and a configurable garrison led by a named camp captain. Supply barrels contain modest food, arrows, metal scraps, and occasional gold nuggets or a crossbow.

Camps avoid recorded settlements at close range. A camp discovered farther outside a settlement can raise its pillager pressure and add a rumor and chronicle report, tying the encounter into the existing settlement system. Generation is deterministic per world seed, only evaluates brand-new chunks, keeps a bounded queue, and builds at most one candidate per second to avoid adding continuous exploration scans.

The `pillagerCamps` config section controls enablement, frequency, and minimum/maximum garrison size. The default chance is one candidate per 96 newly generated chunks; unsuitable terrain can reject a candidate. Existing explored chunks are not retrofitted. See [PILLAGER-CAMPSITE-TEST-CHECKLIST.md](PILLAGER-CAMPSITE-TEST-CHECKLIST.md).

## Settlement war parties

The first semi-hostile war-party release adds save-backed groups belonging to real recorded settlements. Missions are `BORDER_PATROL`, `THREAT_RESPONSE`, and `RETALIATION`. Parties exist as bounded abstract records while far from players, advance every five seconds rather than every tick, and materialize as named armed villagers only near a relevant player. Spawn searches are capped and require solid ground, two clear body blocks, no fluid, fire, portal, or exposed cliff edge. They dematerialize outside the configured radius when combat is inactive, retaining member identity, health, experience, casualties, mission state, and settlement ownership.

Newly recorded settlements now take an immediate observed population snapshot instead of waiting until the next Minecraft day. Ordinary villages may muster at most half their observed population as potential defenders, still respecting the configured defenders-left-home minimum. The first eligible patrol may form immediately; later ordinary patrols obey the configured multi-day interval.

War parties are not hostile on sight. Local settlement trust and settlement-specific grievance produce `FRIENDLY`, `NEUTRAL`, `SUSPICIOUS`, `HOSTILE`, or `RETALIATORY` disposition. Direct attacks and kills create grievance even for high-renown protectors. Suspicious or retaliatory encounters use deterministic challenge and final-warning stages. Withdrawing beyond the warning radius de-escalates without adding grievance. Ignoring a final warning or attacking a member permits combat. Creative and spectator players are ignored, retaliation does not cross dimensions, and pursuit is bounded by mission type.

Holding a sword, selecting a weapon, scrolling across a weapon, charging a bow, or merely walking near a party never creates grievance. The implementation does not poll hotbars or grow suspicion per tick. Direct damage is authoritative. Reliable server-side missed-swing and aimed-bow detection is intentionally deferred instead of using a false-positive approximation.

Use `/mil warparties`, `/mil warparty nearest`, `/mil grievance`, or `/mil grievance <settlement>` for known information. During a valid challenge, `/mil warparty pay` (or `I will pay`, `Take the gold`, or `I will make restitution`) consumes real gold and reduces grievance; severe offenses retain a minimum unresolved grievance. `/mil surrender` (or `I surrender`/`I yield`) stops immediate combat only if the player has not attacked recently, leaves grievance unresolved, and applies a temporary settlement ban. `/mil ledger war` shows known party state without revealing exact coordinates. Operators can use permission-gated `/mil warparty debug-create <border_patrol|threat_response|retaliation>`.

Common configuration is grouped under `warParties`: enablement, global/per-settlement caps, population and reserve-defense minimums, materialization radii, warning duration/radius, patrol interval, grievance thresholds/decay/history, pursuit limits, compensation, and debug logging. Ordinary patrols default to no more than one per eligible settlement every three Minecraft days. Retaliation remains consequence-driven. See [WAR-PARTIES.md](WAR-PARTIES.md) and [WAR-PARTY-TEST-CHECKLIST.md](WAR-PARTY-TEST-CHECKLIST.md).

## Relationship-driven recruit conversations

Recruit conversations now use persistent character motivations, friendship, respect, grievance, memories, opinions, settlement conditions, and compact conversation continuity. Villagers can remain wary, distrustful, resentful, respectful-but-distant, friendly, trusting, or loyal. Accepted orders remain server-authoritative, while a recruit may still obey reluctantly or disagree.

Direct thanks, praise, apologies, insults, threats, personal questions, and repeated denied orders can produce small trait-sensitive relationship effects. These effects have a per-recruit/player cooldown and separate daily caps so chat supplements deeds rather than replacing battles, work, raids, protection, and shared history.

Common configuration:

- `dialogue.relationshipEffectCooldownTicks` defaults to 1200 ticks.
- `dialogue.positiveRelationshipGainPerDay` defaults to 6 points.
- `dialogue.negativeRelationshipGainPerDay` defaults to 12 weighted points.

Operators can use `/mil dialogue-debug` near a recruit to inspect structured motivation, relationship state/values, conversation topic/tone, cooldown, daily totals, and deterministic policy-check results. Local Ollama remains optional; fallback dialogue uses the same relationship state and authoritative order results.

## Settlement Ledger

Phase A adds a dedicated-server-safe ledger through `/mil ledger`. It presents existing systems without requiring a custom client screen:

- `/mil ledger company [page]` lists owned recruits that are currently loaded, their role/order, health, morale, traits, corruption, service record, location, and ownership-safe command controls.
- `/mil ledger settlement` shows settlement stores, pressure, trust, protection, role counts, and explained security, morale, and prosperity modifiers.
- `/mil ledger chronicle [page] [filter]` provides bounded pagination and filters such as recruitment, battles, deaths, construction, corruption, territory, and trade.
- `/mil ledger pin <newest-index>` persists or removes a pin on an important chronicle entry.
- `/mil ledger projects`, `/mil ledger territory`, and `/mil ledger war` summarize currently authoritative data and explicitly mark fields that later phases have not implemented.

The existing `/mil inspect`, `/mil village`, `/mil chronicle`, `/mil territory`, and order commands remain available. Ledger pages resolve the persistent ownership roster only when opened; they add no recurring scans.

For player and server setup, see [INSTALLATION.md](INSTALLATION.md). Generated villager dialogue is optional and documented in [OPTIONAL-AI-SETUP.md](OPTIONAL-AI-SETUP.md). Release history is recorded in [CHANGELOG.md](CHANGELOG.md). For a single context document meant to paste into ChatGPT or another assistant, see [CHATGPT-MOD-CONTEXT.md](CHATGPT-MOD-CONTEXT.md).

Territorial Mobs is a NeoForge 26.1.2 mod about escalating faction conflict and the people caught inside it. Recruited villagers are persistent individuals: they have temperaments, competencies, optional flaws, relationships, memories, morale, experience, and distinct responses to Nether corruption.

Its long-term direction draws from the hard-lived settlement stories of Kenshi and the rising-warband arc of Mount & Blade: begin as an unknown wanderer, learn who lives in a village, recruit a small company, train them through work and danger, defend their home, earn renown and trust, grow settlements, hold territory, and let victories and losses become local history. Territorial Mobs remains an emergent settlement and faction-pressure mod—not a generic companion collection.

Players begin at retaliation tier 0 for both factions, so no hunting parties are sent at the start. Killing a pillager or Nether mob raises only that faction to tier 1; tiers 1–5 then increase party size, shorten the delay between hunts, and introduce stronger Nether compositions. Hunters remember their intended player through chunk reloads; creative and spectator players are ignored.

## Gameplay loop and first strategic slice

The first strategic vertical slice is now playable:

1. Find a living village and inspect its residents.
2. Recruit a company limited by your current renown tier. The roster is saved world-wide, so relogging or walking away from recruits does not restore spent follower slots.
3. Assign recruits as militia, farmers, builders, scouts, medics, blacksmiths, guards, or laborers.
4. Defend the area from pillagers and Nether mobs to earn renown, settlement trust, and reduce hostile pressure.
5. Let one low-frequency settlement update run each Minecraft day. Roles produce stores or improve morale, prosperity, security, and pressure control.
6. Read the village status and chronicle to see the strategic consequences of recruitment, deaths, portals, militia formation, protection, and new renown titles.

Renown titles are **Unknown Wanderer**, **Local Defender**, **Militia Captain**, **Protector**, and **Warden**. Each tier raises the nearby active-follower limit from 2 up to 20. Combat near a registered settlement awards renown; those deeds also build local trust. At 25 trust, the settlement records the player as its sworn protector.

## Phase 6: living history

Settlements now continue through a bounded abstract daily simulation while no player is nearby. Each settlement owns its own simulation cursor and catches up at most 30 detailed days when time advances, preventing unloaded villages from being mistaken for population zero. Last-observed farmers, builders, scouts, medics, blacksmiths, guards, laborers, militia, resources, morale, security, prosperity, and faction pressure drive the offline outcomes.

Autonomous days can produce and consume resources, advance a palisade/watchtower/clinic/storehouse/training-yard/smithy project, repel or lose raids, contain or suffer corruption, attract households, lose households during hardship, and generate scout reports. Results become persisted chronicle entries and bounded rumors rather than continuous AI simulation.

Loaded recruits accumulate service days, battles, raids, assisted kills, and completed construction plans. Milestones award visible titles such as **Veteran**, **Gatekeeper**, **Defender**, or **Builder of [settlement]**. Settlements fill persistent Captain, Head Builder, Chief Scout, Elder, and Protector offices from suitable experienced recruits; deaths vacate an office and immediately consider successors. Veteran and leader deaths receive memorial announcements and rumors.

Portal events now generate persistent personal opinions from temperament, flaws, and corruption response. Recruits may demand destruction, argue for study, seek resources, or embrace corruption. Opinions appear in inspection, chronicles, rumors, and dialogue context. Full player-choice voting, named raid commanders, rival warlords, and capture/rescue remain later Phase 6 slices.

## Player controls

On a player's first login to a world, the mod rejects abandoned/zombie village templates, finds a living village, and searches outward for a safe arrival point with solid, non-hazardous ground and two clear body blocks. That position becomes the player's death-respawn point until they deliberately replace it with a bed or respawn anchor. Initial village placement happens once per player; later logins do not teleport them again.

- Right-click an unrecruited villager to recruit them and generate their persistent identity.
- At your current follower limit, hold gold ingots or nuggets and right-click an unrecruited villager to offer paid recruitment above the cap. Without gold in hand, unrecruited villagers remain available for normal trading instead of consuming the interaction as a failed recruitment attempt.
- Right-click a recruited villager while holding a weapon to transfer and consume one item from that stack in every game mode. Their old weapon is returned to your inventory, and they join the defensive militia.
- Right-click a recruited villager without a weapon to trade normally.
- Sneak-right-click a recruited villager with an empty hand to hear contextual, personality-aware dialogue.
- Chat within 12 blocks of a recruited villager and the nearest recruit will answer what you said. Include a recruit's full name or first name to address that specific villager instead.
- Conversations remain focused on the most recently addressed recruit even when other recruits are closer. Calling another recruit by name transfers that conversation focus.
- Right-click the air to display nearby militia, faction status, and your current renown level and title.

### Optional commands

- `/mil recruit` recruits all villagers within 32 blocks and assigns an identity.
- `/mil inspect` opens a styled, server-safe inspection panel for the nearest recruit. It contains the identity header, trait icons, morale bar, corruption status, relationship tab, and memory timeline.
- `/mil form` turns armed villagers into militia and recruits any who do not already have identities.
- `/mil status` reports militia and faction escalation state.
- `/mil village` shows the nearest settlement's population, militia, morale, security, prosperity, stores, faction pressure, player trust, and protection status.
- `/mil chronicle` shows the latest eight persisted events for the nearest settlement.
- `/mil renown` shows the player's persistent renown title and current follower limit.
- `/mil reputation` shows today's capped economic reputation from villager trading.
- `/mil territory claim` claims the current 16x16 chunk. `/mil territory unclaim`, `info`, and `list` manage and inspect your save-backed claims.
- Placing any banner and right-clicking it claims that banner's chunk. A chunk can have only one owner; overlapping claims are rejected, and other players cannot break, place, or interact with blocks inside it.
- `/mil role <role>` assigns the nearest recruit. Roles are `militia`, `farmer`, `builder`, `scout`, `medic`, `blacksmith`, `guard`, `laborer`, and `unassigned`.
- `/mil order follow`, `/mil order hold`, and `/mil order clear` direct the nearest recruit. Follow and hold unlock at **Local Defender** (15 renown).
- `/mil order mine` sends the nearest recruit to mine the single block the player is looking at within 16 blocks.
- `/mil order build` reserves one held block in the recruit's inventory and sends them to place it beside the looked-at face. Construction requires a recruit assigned as **Builder** and unlocks at **Militia Captain** (50 renown).
- `/mil order fell` discovers and harvests up to 64 connected log blocks from the targeted tree. Targeting a log with `/mil order mine` automatically creates the same whole-tree plan.
- `/mil order hunt` sends the nearest recruit to hunt nearby untamed animals for food and experience. Add a target such as `/mil order hunt cow`, `/mil order hunt sheep`, or `/mil order hunt rabbit` to prefer that animal.
- `/mil order carrying` reports what the nearest recruit is carrying in inventory or visible hands.
- `/mil order drop all` makes the nearest recruit drop every carried item. `/mil order drop <item>` drops matching items such as `iron_sword`, `bread`, or `minecraft:oak_planks`.
- `/mil order structure palisade` reserves 15 matching held blocks and builds a five-wide, three-high wall at **Militia Captain** (50 renown).
- `/mil order structure watchtower` reserves 41 blocks and builds a framed tower and platform at **Protector** (125 renown).
- `/mil order structure shelter` reserves 98 blocks and builds a five-wide enclosed shell at **Warden** (250 renown).
- `/mil transfer` rehomes the nearest recruit into the player's current settlement, then orders them to hold that position. The player must stand in their own claimed territory or near a recorded settlement where they are sworn protector.

All single-block and blueprint construction orders are restricted to the **Builder** role. Assign the nearest recruit first with `/mil role builder`. Mining and tree-felling remain available to other eligible recruits.

The same orders can be spoken to the addressed or nearest recruit with clear phrases such as `follow me`, `stay here`, `guard here`, `go hunting`, `hunt for food`, `hunt cows`, `bring back meat`, `mine this tree`, `dig this`, `build here`, `build a palisade`, `make this your home`, `settle here`, `what are you carrying`, `drop all`, `drop bread`, `drop your weapon`, `drop what you're holding`, `give me your sword`, `cancel order`, and `stand down`. A drop request removes real stored inventory and equipment items, clears matching visible hands, and drops the item stacks into the world without a renown requirement. Other spoken orders use the same renown gates, targeting rules, and resource costs as commands. Recruits confirm accepted chat orders in character; if renown is too low, they refuse and explain the required title and renown instead of falsely promising to obey.

Idle recruits occasionally gather around nearby campfires or soul campfires. If they have food in their inventory, they visibly hold and eat it; otherwise they still idle around the fire with small social gestures.

Ask a recruit about `news`, `rumors`, or what `happened` to hear the latest persisted settlement rumor even when Ollama falls back to local scripted dialogue.

The inspection view uses chat components, so it works on dedicated servers without a custom client packet or menu. `[T]`, `[C]`, `[!]`, and `[N]` are the temperament, competency, flaw, and Nether-response icons.

## Local villager dialogue

Dialogue uses a local Ollama server and never blocks Minecraft's server thread. The default configuration expects `llama3.2:1b` at `http://127.0.0.1:11434/api/chat`, keeps the model warm for 30 minutes, allows one generation at a time, limits each player to one request every three seconds, and falls back to varied, message-aware personality lines when Ollama is disabled, busy, unavailable, or slow.

Install and test the default model before launching Minecraft:

```powershell
ollama pull llama3.2:1b
ollama run llama3.2:1b
```

The generated common configuration contains `llm.enabled`, `llm.endpoint`, `llm.model`, `llm.timeoutSeconds`, and `llm.playerCooldownSeconds`. For multiplayer, Ollama belongs on the dedicated-server machine; clients do not need it. Keep the endpoint bound to localhost unless the network is separately secured.

Dialogue prompts explicitly separate the recruited villager from the human player: first-person language belongs to the recruit, while second-person language addresses the player. Generated speech that simply echoes the player's chat is rejected server-side and replaced with a contextual fallback; follow requests have dedicated personality-aware replies.

While a recruit is actively considering or finishing a response, it paths back toward the speaking player at ten blocks, keeping the conversation inside a twelve-block radius. Only active conversations are tracked; there is no village-wide conversation scan.

Work orders persist with villager identity NBT as bounded plans of at most 128 steps. Combat has higher priority, unbreakable portals and block entities cannot be mined, tree discovery is capped at 64 connected logs, and every construction block is reserved from the player's real inventory before work begins. Obstructed blueprint positions are skipped and their unused material remains in the recruit's inventory.

Builder assignments complete placement steps faster; Laborers mine faster and receive a smaller construction bonus. Trait work-persistence modifiers also affect both tasks, keeping identity and settlement roles relevant to manual orders.

The model receives the player's nearby chat plus a compact snapshot containing the recruit's villager species, traits, morale, corruption exposure, relationship with the player, important memory types, time, weather, held item, and nearby threat count. Recruits know they are Minecraft villagers and generated denials of that identity are rejected. Responses use a strict JSON schema and are sanitized before appearing in chat. Model-suggested intentions and memories are currently descriptive only: the model cannot execute commands, mutate entities, or invent authoritative gameplay events.

Ruined portals use an eight-chunk village exclusion zone so newly generated portals cannot overlap village buildings. Their spacing is increased from the earlier aggressive setting to reduce exploration-time chunk generation spikes. World-generation changes do not remove structures that were already generated.

Nether corruption now embeds sparse Nether gold ore among the replacing netherrack. Gold is deliberately dangerous bait: breaking Nether gold ore, gilded blackstone, or a gold block alerts visible zombified piglins and other piglin defenders within 32 blocks, causing them to attack the miner. This response runs only on the block-break event and does not add a recurring AI scan. Ruined portals activate when a player comes within 64 horizontal blocks; their structure anchor height is deliberately ignored so surface ruins cannot fail activation because of locator Y coordinates.

Pillager outposts generate more often than vanilla, but with moderated spacing and frequency to keep exploration smoother. Killing any hostile mob within 64 blocks of a recorded or vanilla village awards 1 renown, while pillagers award 5 and Nether threats award 4. The village lookup only occurs on the death event, so this progression route adds no recurring scan.

Trading also builds a small amount of economic reputation when completed with villagers near a recorded or vanilla village. Each player can earn up to 6 settlement trust per Minecraft day from trade, plus up to 2 renown per day while below **Militia Captain** (50 renown). This lets peaceful investment help an unknown player become locally known, but later command authority still depends on defense, projects, and dangerous deeds.

Villager trade costs and rewards that use emeralds are converted to gold by default. Low-value emerald values become gold nuggets, while higher values become gold ingots using the common config values under `economy.goldTrades`. Non-currency outputs, villager profession, level, stock, restocking, demand, discounts, XP, and completed-trade reputation are preserved. Trades that require an item plus emeralds keep the original item and convert only the emerald portion. Spending, storing, or carrying gold does not anger Nether factions; aggression only comes from gold-extraction events such as mining Nether gold ore or breaking gilded blackstone or gold blocks.

Gold can also be offered for limited recruitment beyond the player's current renown follower cap. This is configured under `economy.goldRecruitment`: by default the first paid extra recruit costs 4 gold ingots, each additional paid recruit above the cap adds 2 ingots, and at most 3 paid extra recruits are allowed before the player must earn more renown. Paid recruits use the same persistent identity and ownership roster as normal recruits, so they survive relogs and still count toward future follower-cap checks.

## Architecture overview

The implementation has five boundaries:

1. `identity/data` owns save-friendly domain objects and enums.
2. `identity/persistence` converts an identity to and from versioned entity NBT.
3. `identity/event` translates gameplay events into small immutable story events and applies them in batches once per second.
4. `identity/behavior` and `identity/ui` consume identity data without owning persistence.
5. `settlement` owns the world-level strategic ledger, chronicles, rumors, leadership, autonomous projects, territorial pressure, and once-per-day simulation; `renown` owns player progression.

Recruitment creates `VillagerIdentity`, stores it under `territorialmobs:villager_identity` in the villager's persistent data, and adds the `territorialmobs:recruited` entity tag. Because the NBT lives on the entity, normal chunk and world saves carry the identity automatically. A world-wide saved recruitment roster records which player owns each follower for persistent renown-limit accounting; recruit deaths free their slot. A UUID cache avoids repeated NBT decoding while an entity is active.

```text
Minecraft/NeoForge events
        |
        v
IdentityEventHooks / NetherSpreadManager
        |
        v
Concurrent story-event queue -- capped at 128 events/batch
        |
        v
StoryEventSystem -- every 20 server ticks
        |
        +--> relationships, memories, morale, XP, evolution
        +--> corruption response behavior
        +--> VillagerIdentitySerializer --> entity persistent NBT
```

There is no per-tick relationship scan, memory loop, or identity AI loop. Spatial lookups occur only when a relevant event happens (portal creation/site activation or a nearby death).

## Folder structure

```text
src/main/java/com/merciful2spiders/territorialmobs/
|-- identity/
|   |-- IdentityManager.java
|   |-- behavior/
|   |   `-- TraitBehaviorPolicy.java
|   |-- data/
|   |   |-- VillagerIdentity.java
|   |   |-- Relationship.java
|   |   |-- VillagerMemory.java
|   |   |-- CoreTemperament.java
|   |   |-- Competency.java
|   |   |-- VillagerFlaw.java
|   |   |-- MemoryType.java
|   |   `-- CorruptionResponse.java
|   |-- event/
|   |   |-- StoryEvent.java
|   |   |-- StoryEventType.java
|   |   |-- StoryEventSystem.java
|   |   `-- IdentityEventHooks.java
|   |-- persistence/
|   |   `-- VillagerIdentitySerializer.java
|   `-- ui/
|       `-- VillagerInspectionView.java
|-- VillageCommandSystem.java
|-- NetherSpreadManager.java
|-- PillagerAggressionManager.java
`-- MobBehaviorFactory.java
```

## Identity data

Every recruit stores:

- UUID, display name, age stage, profession, recruitment date, and home coordinates
- experience, morale from 0-100, and corruption exposure from 0-100
- one core temperament, one competency, and a 55% chance of one flaw
- a corruption response selected deterministically from the UUID
- at most 16 relationship records and at most five important memories
- persistent work-plan steps, service records, veteran title, settlement office, and up to four recent opinions

Relationship values are clamped: friendship and respect are `-100..100`; grievance is `0..100`. Memories retain the five highest-importance entries, preferring newer entries when importance ties. Enum names are serialized instead of ordinals, so appending traits is save-compatible.

## Event system and existing mechanics

The story queue currently connects to:

- recruitment: adds the `RECRUITED` memory
- constructed and active ruined portals: adds `SAW_PORTAL_OPEN`, exposure, morale effects, and response behavior
- Nether mob and pillager deaths: nearby recruits gain shared-combat memories and reciprocal relationship changes
- recruited villager deaths: nearby recruits gain `LOST_FRIEND`, grievance, and morale loss
- militia formation: recruits villagers before trait-weighted combat movement is installed
- Nether terrain spread: nearby Resistant recruits reduce event-time spread attempts by 35%

`IdentityEventHooks.ateTogether`, `workedTogether`, and `survivedRaid` are public adapters for settlement job, meal, and raid-completion systems. They enqueue changes rather than applying them synchronously.

### Corruption responses

- **Resistant** takes half exposure and reduces nearby portal terrain spread.
- **Adaptable** suffers smaller morale penalties.
- **Enthralled** can path toward a newly active portal when its portal event is processed.
- **Symbiotic** gains experience at portal events and receives a work-persistence bonus after exposure.
- **Devoted** is at risk of a permanent, named Nether-devoted transformation at maximum exposure.

## Trait behavior and progression

`TraitBehaviorPolicy` turns traits into pathing tolerance, danger response, work persistence, and interaction-frequency multipliers. Militia pursuit already consumes pathing and danger modifiers. Settlement work and dialogue systems can consume the other two without reading enum values throughout the codebase.

Evolution is intentionally rare and event-driven. A villager needs at least 120 experience, four retained memories, an event of importance 60+, and a one-in-eight event roll. Successful evolution is announced to all players:

- Timid -> Resolute
- Curious -> Explorer
- Bitter -> Protective
- Greedy -> Generous

An evolution adds a maximum-importance `TRAIT_EVOLVED` memory.

## Save/load format

`VillagerIdentitySerializer` writes a `version` field plus primitive identity fields and nested compounds for relationships and memories. The reader supplies defaults for missing or unknown values, allowing new fields and enum values to be introduced without invalidating old villagers. All numeric values are clamped again by domain constructors/mutators while loading.

Villager identity and role assignment are written after recruitment and mutations under the existing versioned `territorialmobs:villager_identity` entity payload. Player renown is stored in server-owned player persistent NBT. Settlement status and bounded chronicles use a versioned `SavedData` ledger in each dimension. Vanilla saves all three through restarts; older recruited villagers load with the safe `UNASSIGNED` role default.

The strategic simulation does not scan villages every tick. A cheap day-bound guard runs from the server level event, and each registered settlement performs one bounded 64-block villager/role count only when a new Minecraft day begins. Combat, portal, death, recruitment, trust, and chronicle changes remain event-driven.

## Phased roadmap

- **Phase 1 — strategic foundation (implemented):** persistent renown, settlement status/resources, role assignments, daily production/security, territorial pressure, and chronicles.
- **Phase 2 — consequences:** incapacitation and severe injuries before some combat deaths, recovery states, medic/clinic effects, witness morale and relationship consequences, scars, and veteran history.
- **Phase 3 — settlement projects:** palisade, watchtower, clinic, storehouse, training yard, and smithy with material costs, builder/labor contribution, completion effects, and `/mil projects` controls.
- **Phase 4 — living conflict:** raid lifecycle integration, scouts and warnings, village defense objectives, project damage, faction campaigns, and stronger renown/trust command gates.
- **Phase 5 — territorial rule:** multiple protected villages, influence links, patrol routes, governors or wardens, strategic map presentation, diplomacy, and world-spanning chronicles.
- **Phase 6 — living history (first slice implemented):** autonomous catch-up, abstract projects/incidents, rumors, veterans, leadership succession, hero memorials, and portal opinions. Remaining slices include named raids, rival warlords, capture/rescue, and player-mediated disputes.

## Adding traits and events

To add a trait:

1. Append its stable name to the appropriate enum.
2. Add its behavioral mapping to `TraitBehaviorPolicy`.
3. Add event-specific outcomes in `StoryEventSystem` only when the trait changes an outcome.
4. Add an evolution branch if appropriate. Never reorder or rename a released serialized value without a migration.

To add an event:

1. Add a `StoryEventType` value.
2. Enqueue `StoryEvent` from a NeoForge hook or an existing gameplay system.
3. Handle it in `StoryEventSystem.apply` and save only the affected villagers.

Keep hooks event-driven. Prefer one bounded spatial query at the triggering event over recurring scans.

## Build

This project targets Java 25, Minecraft 26.1.2, and NeoForge 26.1.2.74.

```powershell
.\gradlew.bat build
```

For a development client:

```powershell
.\gradlew.bat runClient
```
