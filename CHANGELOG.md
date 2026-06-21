# Changelog

Notable changes to Territorial Mobs are recorded here. The project uses semantic versioning where practical.

## [1.0.0] - 2026-06-20

### Added

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
