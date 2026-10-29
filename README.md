# AgenticSimulationBasedInference

## Query the paper library

The local RAGFlow dataset **project80** is available to OpenCode through the
project-local `papers` MCP connection in `opencode.jsonc`. Papers and their search
index remain in RAGFlow; this repository stores only connection configuration and
research instructions.

- RAGFlow: <http://127.0.0.1>
- MCP endpoint: `http://127.0.0.1:9380/api/v1/mcp`
- Dataset ID: `56ea658fbc7441f1ac30407256869d20`

With RAGFlow running and the credential configured, work in this repository and
ask OpenCode, for example:

> Search project80: what roles do predictability, computability, and stability
> play in veridical data science? Cite the supporting paper passages.

Or, once the Giles paper has finished parsing:

> What assumptions underlie Giles's MLMC complexity bounds, and which might fail
> in reservoir forecasting? Separate the paper's claims from your interpretation.

`AGENTS.md` scopes paper searches to this dataset and requires source citations.
Only successfully parsed, indexed documents can contribute search results.

### Local credential

The MCP configuration reads a RAGFlow API key from
`.opencode/ragflow-api-key`. This file is Git-ignored and should have owner-only
permissions (`600`). It contains just the key, without quotes or a `Bearer`
prefix. No environment-variable export is needed.

On a fresh checkout, acquire a key from RAGFlow's API settings and save it without
putting it in shell history or chat:

```sh
mkdir -p .opencode
python3 - <<'PY'
import getpass
import os

key = getpass.getpass("RAGFlow API key: ").strip()
if not key:
    raise SystemExit("API key must not be empty")
fd = os.open(".opencode/ragflow-api-key", os.O_WRONLY | os.O_CREAT | os.O_TRUNC, 0o600)
os.fchmod(fd, 0o600)
with os.fdopen(fd, "w") as credential:
    credential.write(key)
PY
```

Check the connection with `opencode mcp list`; `papers` should be connected.
OpenCode reloads configuration automatically. If you replace the key, use
`/reload` to reload its contents. Do not commit the key or paste it into chat.

The RAGFlow API key may authorize access beyond `project80`; the explicit dataset
filter is not a security boundary. This configuration uses localhost and is for
the local machine. Another RAGFlow instance requires its own endpoint, key, and
dataset ID.
