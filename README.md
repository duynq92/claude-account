# claude-account

Bind a Claude Code account to a project directory, and switch per project.

Each account is an isolated `CLAUDE_CONFIG_DIR` under `~/.claude-accounts/<name>`. Claude Code stores a separate keychain credential per config dir, so logging in to one account never logs out another. Your `CLAUDE.md`, `settings.json`, skills, plugins, agents, commands, hooks and keybindings are symlinked from `~/.claude`, so every account shares the same setup.

## Install

Requires `bash`, `jq` and `claude` on your `PATH`.

```bash
ln -s "$PWD/claude-account" ~/.local/bin/claude-account
echo 'command -v claude-account >/dev/null && eval "$(claude-account shell-init)"' >> ~/.zshrc
```

The second line wraps `claude` so it uses the account bound to the current project.

## Usage

```bash
claude-account add personal      # create an account profile
claude-account login personal    # launch Claude Code to sign in (/login)
claude-account bind personal     # bind the current directory
claude-account which             # show the account used here
claude-account list              # list accounts (* = active here)
claude-account use work          # run claude once with another account
claude-account unbind            # remove the binding
```

`default` is reserved and means your normal `~/.claude`.

## How an account is picked

1. `CLAUDE_ACCOUNT` environment variable
2. The nearest `.claude-account` file, walking up from the current directory
3. `default`

`bind` writes `.claude-account` and adds it to the repo's `.git/info/exclude`, so it isn't committed.

## Limitations

- Only affects `claude` started from a shell. The Claude desktop app always uses the default account.
- User-scoped MCP servers live in each account's `.claude.json` and are not shared; add them per account.
