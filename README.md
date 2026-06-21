# Territorial Mobs

Territorial Mobs is a NeoForge 26.1.2 mod about escalating faction conflict and the people caught inside it. Recruited villagers are persistent individuals: they have temperaments, competencies, optional flaws, relationships, memories, morale, experience, and distinct responses to Nether corruption.

Its long-term direction draws from the hard-lived settlement stories of Kenshi and the rising-warband arc of Mount & Blade: begin as an unknown wanderer, learn who lives in a village, recruit a small company, train them through work and danger, defend their home, earn renown and trust, grow settlements, hold territory, and let victories and losses become local history. Territorial Mobs remains an emergent settlement and faction-pressure mod—not a generic companion collection.

## Gameplay loop and first strategic slice

The first strategic vertical slice is now playable:

1. Find a living village and inspect its residents.
2. Recruit a company limited by your current renown tier.
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
- Right-click a recruited villager while holding a weapon to transfer and consume one item from that stack in every game mode. Their old weapon is returned to your inventory, and they join the defensive militia.
- Right-click a recruited villager without a weapon to trade normally.
- Sneak-right-click a recruited villager with an empty hand to hear contextual, personality-aware dialogue.
- Chat within 12 blocks of a recruited villager and the nearest recruit will answer what you said. Include a recruit's full name or first name to address that specific villager instead.
- Right-click the air to display nearby militia, faction status, and your current renown level and title.

### Optional commands

- `/mil recruit` recruits all villagers within 32 blocks and assigns an identity.
- `/mil inspect` opens a styled, server-safe inspection panel for the nearest recruit. It contains the identity header, trait icons, morale bar, corruption status, relationship tab, and memory timeline.
- `/mil form` turns armed villagers into militia and recruits any who do not already have identities.
- `/mil status` reports militia and faction escalation state.
- `/mil village` shows the nearest settlement's population, militia, morale, security, prosperity, stores, faction pressure, player trust, and protection status.
- `/mil chronicle` shows the latest eight persisted events for the nearest settlement.
- `/mil renown` shows the player's persistent renown title and current follower limit.
- `/mil role <role>` assigns the nearest recruit. Roles are `militia`, `farmer`, `builder`, `scout`, `medic`, `blacksmith`, `guard`, `laborer`, and `unassigned`.
- `/mil order follow`, `/mil order hold`, and `/mil order clear` direct the nearest recruit. Follow and hold unlock at **Local Defender** (25 renown).
- `/mil order mine` sends the nearest recruit to mine the single block the player is looking at within 16 blocks.
- `/mil order build` reserves one held block in the recruit's inventory and sends them to place it beside the looked-at face. Construction requires a recruit assigned as **Builder** and unlocks at **Militia Captain** (75 renown).
- `/mil order fell` discovers and harvests up to 64 connected log blocks from the targeted tree. Targeting a log with `/mil order mine` automatically creates the same whole-tree plan.
- `/mil order structure palisade` reserves 15 matching held blocks and builds a five-wide, three-high wall at **Militia Captain**.
- `/mil order structure watchtower` reserves 41 blocks and builds a framed tower and platform at **Protector** (175 renown).
- `/mil order structure shelter` reserves 98 blocks and builds a five-wide enclosed shell at **Warden** (350 renown).

All single-block and blueprint construction orders are restricted to the **Builder** role. Assign the nearest recruit first with `/mil role builder`. Mining and tree-felling remain available to other eligible recruits.

The same orders can be spoken to the addressed or nearest recruit with clear phrases such as `follow me`, `stay here`, `mine this tree`, `build here`, `build a palisade`, and `stand down`. Spoken orders use the same renown gates, targeting rules, and resource costs as commands.

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

The model receives the player's nearby chat plus a compact snapshot containing the recruit's traits, morale, corruption exposure, relationship with the player, important memory types, time, weather, held item, and nearby threat count. Responses use a strict JSON schema and are sanitized before appearing in chat. Model-suggested intentions and memories are currently descriptive only: the model cannot execute commands, mutate entities, or invent authoritative gameplay events.

Ruined portals use an eight-chunk village exclusion zone so newly generated portals cannot overlap village buildings. World-generation changes do not remove structures that were already generated.

Nether corruption now embeds sparse Nether gold ore among the replacing netherrack. The gold is deliberately dangerous bait: breaking Nether gold ore alerts visible zombified piglins within 32 blocks, causing them to defend the deposit and attack the miner. This response runs only on the block-break event and does not add a recurring AI scan. Ruined portals activate when a player comes within 64 horizontal blocks; their structure anchor height is deliberately ignored so surface ruins cannot fail activation because of locator Y coordinates.

Pillager outposts generate substantially more often than vanilla. Killing any hostile mob within 64 blocks of a recorded or vanilla village awards 1 renown, while pillagers award 5 and Nether threats award 4. The village lookup only occurs on the death event, so this progression route adds no recurring scan.

## Architecture overview

The implementation has five boundaries:

1. `identity/data` owns save-friendly domain objects and enums.
2. `identity/persistence` converts an identity to and from versioned entity NBT.
3. `identity/event` translates gameplay events into small immutable story events and applies them in batches once per second.
4. `identity/behavior` and `identity/ui` consume identity data without owning persistence.
5. `settlement` owns the world-level strategic ledger, chronicles, rumors, leadership, autonomous projects, territorial pressure, and once-per-day simulation; `renown` owns player progression.

Recruitment creates `VillagerIdentity`, stores it under `territorialmobs:villager_identity` in the villager's persistent data, and adds the `territorialmobs:recruited` entity tag. Because the NBT lives on the entity, normal chunk and world saves carry the identity automatically. A UUID cache avoids repeated NBT decoding while an entity is active.

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
