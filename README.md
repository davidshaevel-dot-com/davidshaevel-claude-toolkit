# davidshaevel-claude-toolkit

Personal Claude Code plugin providing development conventions, skills, and project templates.

## What This Plugin Provides

- **Development conventions** injected automatically at session start via hook
- **Skills** for code review resolution, session handoff (cross-agent memory), and project bootstrapping
- **Templates** for initializing new projects with standard structure

## Installation

```bash
# Add as a marketplace
/plugin marketplace add davidshaevel-dot-com/davidshaevel-claude-toolkit

# Install the plugin
/plugin install davidshaevel-claude-toolkit@davidshaevel-claude-toolkit
```

## Skills

| Skill | Description |
|-------|-------------|
| `resolve-code-review` | Read PR feedback, fix or decline each item, reply in threads, post summary |
| `session-handoff` | Read/write SESSION_LOG.md for cross-agent memory persistence |
| `bootstrap-project` | Initialize new projects with CLAUDE.md, .cursorrules, CLAUDE.local.md, SESSION_LOG.md |

## Commands

| Command | Description |
|---------|-------------|
| `/resolve-code-review` | Invoke the resolve-code-review skill |
| `/bootstrap-project` | Invoke the bootstrap-project skill |

## Updating the Plugin

After pushing a new version to the repository, three locations must be updated for Claude Code to load the new version.

### 1. Marketplace directory

The marketplace directory is a git clone that needs to be pulled:

```bash
git -C ~/.claude/plugins/marketplaces/davidshaevel-claude-toolkit pull origin main
```

### 2. Plugin cache

The cache stores versioned clones at `~/.claude/plugins/cache/davidshaevel-claude-toolkit/davidshaevel-claude-toolkit/<version>/`. Create a new directory for the new version:

```bash
# Clone the new version tag into the cache
git clone --branch v<NEW_VERSION> --depth 1 \
  git@github.com:davidshaevel-dot-com/davidshaevel-claude-toolkit.git \
  ~/.claude/plugins/cache/davidshaevel-claude-toolkit/davidshaevel-claude-toolkit/<NEW_VERSION>

# Optionally recreate the old version directory from its tag (keeps it clean)
rm -rf ~/.claude/plugins/cache/davidshaevel-claude-toolkit/davidshaevel-claude-toolkit/<OLD_VERSION>
git clone --branch v<OLD_VERSION> --depth 1 \
  git@github.com:davidshaevel-dot-com/davidshaevel-claude-toolkit.git \
  ~/.claude/plugins/cache/davidshaevel-claude-toolkit/davidshaevel-claude-toolkit/<OLD_VERSION>
```

### 3. Installed plugins registry

Update `~/.claude/plugins/installed_plugins.json` to point to the new version:

- `installPath` → update the version in the path
- `version` → new version string
- `gitCommitSha` → commit SHA of the new version
- `lastUpdated` → current ISO timestamp

### 4. Restart Claude Code

Permission changes and plugin updates require a session restart to take effect.

## Convention Change Propagation

- **Claude Code:** Follow the update steps above, then restart the session
- **Cursor:** Re-run `/bootstrap-project` to regenerate `.cursorrules`

## License

MIT
