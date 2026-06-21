# Territorial Mobs Installation Guide

Territorial Mobs targets Minecraft Java Edition 26.1.2, NeoForge 26.1.2.74, and Java 25. Local AI dialogue is optional; all other gameplay systems work without it.

## Client installation

1. Install and launch Minecraft Java Edition 26.1.2 once, then close it.
2. Download the NeoForge installer from <https://neoforged.net/>.
3. Select NeoForge 26.1.2.74 and choose **Install client**.
4. Launch the new NeoForge profile once, then close Minecraft.
5. Press `Windows + R`, enter `%appdata%\.minecraft`, and press Enter.
6. Open the `mods` folder. Create it if necessary.
7. Copy `territorialmobs-1.0.0.jar` into `mods`. Do not extract the JAR.
8. Launch the NeoForge profile and confirm that Territorial Mobs appears in the Mods menu.

If the NeoForge installer does not open, install a 64-bit Java 25 runtime and try again.

## Optional local AI

For generated villager conversations, continue with [OPTIONAL-AI-SETUP.md](OPTIONAL-AI-SETUP.md). Without Ollama, villagers automatically use the mod's built-in contextual fallback dialogue.

## Dedicated server installation

1. Install a Minecraft 26.1.2 server with NeoForge 26.1.2.74.
2. Run the server once to generate its directories, then stop it.
3. Copy `territorialmobs-1.0.0.jar` into the server's `mods` folder.
4. Start the server and accept the generated configuration.
5. If AI dialogue is wanted, install Ollama and the model on the server machine by following [OPTIONAL-AI-SETUP.md](OPTIONAL-AI-SETUP.md).
6. Give every player the same Territorial Mobs JAR and require the same Minecraft and NeoForge versions.

Only the dedicated server needs Ollama. Connected clients do not.

## Confirm that the mod works

1. Enter a world and right-click an unrecruited villager.
2. Confirm that the villager receives a generated name and becomes recruited.
3. Right-click the air and confirm that militia, retaliation, and renown status appear.
4. Stand within 12 blocks of the recruit and write a message in chat.
5. The recruit should answer through Ollama or built-in fallback dialogue.

## Troubleshooting

### The mod is missing from the Mods menu

- Confirm that Minecraft is 26.1.2 and NeoForge is 26.1.2.74.
- Confirm that the JAR is directly inside `mods` and was not extracted.
- Confirm that the NeoForge launcher profile is selected.
- Remove older Territorial Mobs JARs so only one version is installed.

### The server rejects a connection

- The server and every player must use matching Minecraft, NeoForge, and Territorial Mobs versions.
- Check the server's `logs/latest.log` for the first reported missing or incompatible mod.

### Villagers use fallback dialogue

This is safe and does not disable recruitment, orders, identity, settlements, combat, or progression. Follow the checks in [OPTIONAL-AI-SETUP.md](OPTIONAL-AI-SETUP.md) if generated dialogue is expected.

## Updating

1. Back up the world.
2. Stop Minecraft or the dedicated server completely.
3. Remove the old Territorial Mobs JAR from `mods`.
4. Add the new JAR; do not keep both versions.
5. Start the game and retain the existing world save and configuration unless the release notes instruct otherwise.

Villager identities, renown, settlement records, rumors, and chronicles are stored in the world save. Always keep a backup before changing mod versions.
