Territorial Mobs Installation Guide

Territorial Mobs requires Minecraft 26.1.2, NeoForge 26.1.2.74, and the Territorial Mobs mod JAR.
Local AI dialogue is optional. Without it, villagers still speak using built-in fallback dialogue.
1. Install Minecraft and NeoForge
Install and launch Minecraft Java Edition 26.1.2 once.
Close Minecraft.
Download the NeoForge installer from NeoForge.
Select NeoForge version 26.1.2.74.
Run the installer and choose Install client.
Open the Minecraft Launcher.
Select the new NeoForge installation and launch it once.
Close Minecraft again.
If the installer will not open, install a 64-bit Java 25 runtime and retry.
2. Install Territorial Mobs
Download territorialmobs-1.0.0.jar.
Press Windows + R.
Enter:
%appdata%\.minecraft
Open the mods folder. Create it if it does not exist.
Place territorialmobs-1.0.0.jar inside mods.
Do not extract the JAR.
Launch Minecraft using the NeoForge profile.
Open the Mods menu and confirm that Territorial Mobs appears.
Everyone joining a multiplayer server must have the same mod version installed.
3. Install the optional local AI
The AI integration is inside the mod, but Ollama and its language model must be installed separately.
Windows
Download Ollama from Ollama for Windows.
Run the installer.
Open PowerShell.
Download the recommended model:
ollama pull llama3.2:1b
Test it:
ollama run llama3.2:1b
When the prompt appears, enter:
Reply with only: ready
After it responds, press Ctrl+D or enter /bye.
Leave Ollama running in the background.
Launch Minecraft.
The first response after starting Ollama may take longer while the model loads.
4. Verify villager dialogue
Enter a world.
Right-click an unrecruited villager to recruit them.
Stand within 12 blocks of the recruit.
Write something in chat, such as:
How are you feeling today?
The villager should begin thinking and then answer.
You can address a particular recruit using their first or full name.
Spoken orders include:
follow me
stay here
mine this tree
build here
stand down
Orders still require the correct role and renown level.
5. Playing without Ollama
Ollama is not required for the mod to load.
If Ollama is missing, disabled, busy, or too slow, villagers use contextual fallback dialogue. Settlement systems, combat, recruitment, orders, memories, renown, and world generation continue working.
To disable AI generation deliberately:
Launch the game once.
Close Minecraft.
Open:
.minecraft\config\territorialmobs-common.toml
Change:
llm.enabled = true
to:
llm.enabled = false
6. Dedicated-server installation
Server owner
Install Minecraft 26.1.2 with NeoForge 26.1.2.74 on the server.
Place territorialmobs-1.0.0.jar in the server’s mods folder.
Start the server once, then stop it.
Install Ollama on the server machine.
Run:
ollama pull llama3.2:1b
Ensure Ollama is running.
Check:
config\territorialmobs-common.toml
Confirm these settings:
llm.enabled = true
llm.endpoint = "http://127.0.0.1:11434/api/chat"
llm.model = "llama3.2:1b"
llm.timeoutSeconds = 60
llm.playerCooldownSeconds = 3
Restart the Minecraft server.
Only the dedicated-server machine needs Ollama. Players connecting to it only need Minecraft, NeoForge, and the mod JAR.
Keep Ollama bound to 127.0.0.1. Do not expose port 11434 publicly.
7. Troubleshooting
The mod does not appear
Confirm that:
You launched the NeoForge profile.
Minecraft is version 26.1.2.
NeoForge is version 26.1.2.74.
The JAR is directly inside mods.
The JAR was not extracted.
Villagers only use fallback dialogue
Open PowerShell and run:
ollama list
Confirm that llama3.2:1b is listed. Then test Ollama:
ollama run llama3.2:1b
If Ollama is not running automatically, try:
ollama serve
Keep that window open and restart Minecraft.
Responses take too long
Close memory-heavy applications. The model runs locally and uses the host computer’s RAM and processor or GPU.
You can reduce the timeout in territorialmobs-common.toml, but shorter timeouts will cause fallback dialogue more often.
Multiplayer dialogue does not work
Ollama must be installed on the server, not each client. The endpoint must be reachable from the Minecraft server process.
The game reports incompatible mods
Make sure the server and every player use exactly the same:
Minecraft version
NeoForge version
Territorial Mobs JAR
