# Installing Superpowers for OpenCode

## Prerequisites

- [OpenCode.ai](https://opencode.ai) installed

## Installation

OpenCode installs Superpowers from the `plugin` array in `opencode.json`.

### Manual install

Edit your OpenCode config file:

- Windows: `%USERPROFILE%\.config\opencode\opencode.json`
- macOS/Linux: `~/.config/opencode/opencode.json`

Add or update the top-level `plugin` array:

```json
{
  "plugin": ["superpowers@git+https://github.com/evepupil/superpowers-yeton-ver.git"]
}
```

If you already have the official Superpowers plugin installed, replace its entry:

```json
{
  "plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]
}
```

becomes:

```json
{
  "plugin": ["superpowers@git+https://github.com/evepupil/superpowers-yeton-ver.git"]
}
```

Keep only one `superpowers` plugin entry enabled.

### Prompt install

Paste this into OpenCode and let it update its own config:

```text
请帮我安装 Superpowers Yeton 版 OpenCode 插件。

要求：
1. 找到或创建 OpenCode 全局配置文件：
   - Windows: %USERPROFILE%\.config\opencode\opencode.json
   - macOS/Linux: ~/.config/opencode/opencode.json
2. 保留配置文件里已有的其他配置。
3. 在顶层 plugin 数组中安装：
   superpowers@git+https://github.com/evepupil/superpowers-yeton-ver.git
4. 如果已经存在官方 superpowers 插件源，例如 github.com/obra/superpowers，请替换成上面的 Yeton 版源。
5. 确保最终只保留一个 superpowers 插件条目。
6. 修改完成后告诉我需要重启 OpenCode，并说明如何验证是否安装成功。
```

Restart OpenCode. The plugin installs through OpenCode's plugin manager and
registers all skills.

Verify by asking OpenCode to list skills and checking for fork-specific skills
such as `creating-agents-guidance`, `qa-testing-workflow`, or `qa-risk-review`.

OpenCode uses its own plugin install. If you also use Claude Code, Codex, or
another harness, install Superpowers separately for each one.

## Migrating from the old symlink-based install

If you previously installed superpowers using `git clone` and symlinks, remove the old setup:

```bash
# Remove old symlinks
rm -f ~/.config/opencode/plugins/superpowers.js
rm -rf ~/.config/opencode/skills/superpowers

# Optionally remove the cloned repo
rm -rf ~/.config/opencode/superpowers

# Remove skills.paths from opencode.json if you added one for superpowers
```

Then follow the installation steps above.

## Usage

Use OpenCode's native `skill` tool:

```
use skill tool to list skills
use skill tool to load superpowers/brainstorming
```

## Updating

OpenCode installs Superpowers through a git-backed package spec. Some OpenCode
and Bun versions pin that resolved git dependency in a lockfile or cache, so a
restart may not pick up the newest Superpowers commit. If updates do not appear,
clear OpenCode's package cache or reinstall the plugin.

To pin a specific version:

```json
{
  "plugin": ["superpowers@git+https://github.com/evepupil/superpowers-yeton-ver.git#v5.1.0"]
}
```

## Troubleshooting

### Plugin not loading

1. Check logs: `opencode run --print-logs "hello" 2>&1 | grep -i superpowers`
2. Verify the plugin line in your `opencode.json`
3. Make sure you're running a recent version of OpenCode

### Windows install issues

Some Windows OpenCode builds have upstream installer issues with git-backed
plugin specs, including cache paths for `git+https` URLs and Bun not finding
`git.exe` even when it works in a normal terminal. If OpenCode cannot install
the plugin, try installing with system npm and pointing OpenCode at the local
package:

```powershell
npm install superpowers@git+https://github.com/evepupil/superpowers-yeton-ver.git --prefix "$HOME\.config\opencode"
```

Then use the installed package path in `opencode.json`:

```json
{
  "plugin": ["~/.config/opencode/node_modules/superpowers"]
}
```

### Skills not found

1. Use `skill` tool to list what's discovered
2. Check that the plugin is loading (see above)

### Tool mapping

When skills reference Claude Code tools:
- `TodoWrite` → `todowrite`
- `Task` with subagents → `@mention` syntax
- `Skill` tool → OpenCode's native `skill` tool
- File operations → your native tools

## Getting Help

- Report issues: https://github.com/evepupil/superpowers-yeton-ver/issues
- Full documentation: https://github.com/evepupil/superpowers-yeton-ver/blob/main/docs/README.opencode.md
