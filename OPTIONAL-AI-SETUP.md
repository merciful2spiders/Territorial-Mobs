# Optional Local AI Setup

Territorial Mobs can use a local Ollama model for generated recruited-villager dialogue. The AI request runs asynchronously and never controls entities or changes authoritative gameplay state. When Ollama is unavailable, the mod uses contextual fallback dialogue.

## What gets installed

- Ollama runs the language model locally on the Minecraft host.
- The recommended model is `llama3.2:1b`.
- Single-player users run Ollama on their own computer.
- Dedicated-server owners run Ollama on the server machine; clients do not need it.

Ollama and its model are not bundled with the Territorial Mobs JAR.

## Windows setup

1. Download Ollama from <https://ollama.com/download/windows>.
2. Run the installer and allow it to start in the background.
3. Open PowerShell.
4. Download the recommended model:

```powershell
ollama pull llama3.2:1b
```

5. Test the model:

```powershell
ollama run llama3.2:1b
```

6. Enter `Reply with only: ready` and wait for a response.
7. Enter `/bye` or press `Ctrl+D` to leave the model prompt.
8. Leave Ollama running and launch Minecraft or the dedicated server.

If Ollama is installed but not running, open a separate PowerShell window and run:

```powershell
ollama serve
```

Keep that window open while playing.

## Verify the local service

List installed models:

```powershell
ollama list
```

Confirm that the API is listening:

```powershell
Invoke-RestMethod http://127.0.0.1:11434/api/tags
```

The second command should return model information rather than a connection error.

## Territorial Mobs configuration

Launch the game or server once to generate:

```text
config/territorialmobs-common.toml
```

The AI section should resemble:

```toml
[llm]
enabled = true
endpoint = "http://127.0.0.1:11434/api/chat"
model = "llama3.2:1b"
timeoutSeconds = 60
playerCooldownSeconds = 3
```

- `enabled` turns Ollama generation on or off.
- `endpoint` is the Ollama chat API used by the Minecraft host.
- `model` must exactly match a model shown by `ollama list`.
- `timeoutSeconds` controls how long the mod waits before falling back.
- `playerCooldownSeconds` limits how often one player can request dialogue.

Restart Minecraft or the server after editing this file.

## In-game test

1. Recruit a villager by right-clicking them.
2. Stand within 12 blocks.
3. Say `How are you feeling today?` in chat.
4. The villager should display a thinking message and then answer.
5. Ask about `news` or `rumors` to test settlement-aware dialogue.

The first request can be slower because the model needs to load. Later requests should benefit from the model remaining warm.

## Dedicated-server notes

- Install Ollama on the same machine that runs the Minecraft server.
- Keep the default endpoint on `127.0.0.1` whenever both programs share a machine.
- Do not expose Ollama port `11434` directly to the public internet.
- Clients only install the Territorial Mobs JAR; they do not install Ollama or download the model.

## Disabling generated dialogue

Change the generated configuration to:

```toml
[llm]
enabled = false
```

Villagers will immediately rely on built-in dialogue after the next restart. No world data is lost.

## Troubleshooting

### `ollama` is not recognized

Restart Windows after installing Ollama, or reopen PowerShell so the updated command path is loaded.

### The model is missing

Run:

```powershell
ollama pull llama3.2:1b
ollama list
```

Ensure the configured model name matches the installed name.

### The API refuses the connection

Start Ollama with `ollama serve`, then repeat the API verification command. Check whether security software is blocking the local process.

### Responses time out

- Close memory-heavy programs.
- Wait for the initial model load and try again.
- Keep `llama3.2:1b` as the low-memory default.
- Check `logs/debug.log` or `logs/latest.log` for `Local dialogue generation failed` and timeout messages.
- A timeout is nonfatal: the villager will use a fallback line.

### Multiplayer AI does not respond

Run `ollama list` and the API verification command on the server machine, not the player's computer. Confirm that the server's configuration uses the correct endpoint and model.
