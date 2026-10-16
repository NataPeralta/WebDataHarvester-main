## Cursor Cloud specific instructions

This Cloud VM is Ubuntu 24.04 (not CachyOS). Native reverse-engineering tooling is already installed for the `ubuntu` user; it is not part of the git tree.

### Tooling locations

| Tool | Location |
|---|---|
| Ghidra 12.1.4 PUBLIC | `/home/ubuntu/tools/ghidra_12.1.4_PUBLIC` (symlink `/home/ubuntu/tools/ghidra`) |
| Headless launcher | `analyzeHeadless` on PATH via `~/.local/bin` |
| JDK | `/usr/lib/jvm/java-21-openjdk-amd64` (`JAVA_HOME`) |
| REA CLI | `~/.local/bin/rea` (`rea-agents` 6.5.0 under `~/.local`) |
| REA skill | `~/.agents/skills/reverse-engineer-anything` (also copied to `~/.cursor/skills/reverse-engineer-anything`) |
| Cursor MCP registration | `~/.cursor/mcp.json` (`rea mcp`, with `GHIDRA_INSTALL_DIR` and `JAVA_HOME`) |

### Shell prerequisites

Node for REA must be **22.23.3+** from nvm (the default `/exec-daemon/node` is 22.14 and is too old for `rea-agents`). Prefer:

```bash
export NVM_DIR="$HOME/.nvm"
. "$NVM_DIR/nvm.sh"
nvm use 22.23.3
export PATH="$HOME/.nvm/versions/node/v22.23.3/bin:$HOME/.local/bin:$PATH"
export GHIDRA_INSTALL_DIR="$HOME/tools/ghidra_12.1.4_PUBLIC"
export JAVA_HOME="/usr/lib/jvm/java-21-openjdk-amd64"
```

These exports are also appended to `~/.bashrc`.

### Checks

```bash
rea doctor --provider ghidra --client cursor --json
rea analyze /bin/true --provider ghidra --format json
```

Ghidra smoke without REA: `analyzeHeadless /tmp/proj name -import /path/to/binary -deleteProject -readOnly`.

### MCP note

After registering REA, restart or reconnect the agent so Cursor loads `~/.cursor/mcp.json`. Until then, use the `rea` CLI. REA does not install Ghidra or Java; both are already present here.

### Scope / policy

Use REA and Ghidra only on binaries or apps you are authorized to analyze. Prefer the installed REA skill workflow (`reverse-engineer-anything`) for investigation structure and Evidence handling.
