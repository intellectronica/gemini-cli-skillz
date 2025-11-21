# Gemini CLI Skills Extension

Run Anthropic-style Agent Skills in Gemini CLI using the [skillz MCP server](https://github.com/intellectronica/skillz).

## Installation

```bash
gemini extensions install https://github.com/intellectronica/gemini-cli-skillz
```

**Note**: If the GitHub release download fails with a 415 error, answer "Y" when prompted to install via git clone instead.

## Setup

1. **Create skills directory** (if using default):
   ```bash
   mkdir -p ~/.skillz
   ```

2. **Add skills** to the directory. Each skill is a folder with a SKILL.md file.

3. **Restart Gemini CLI** to load the skills.

## Example: Installing Anthropic Skills

```bash
cd ~/.skillz

# Clone specific skills from Anthropic's repository
git clone --depth 1 --filter=blob:none --sparse \
  https://github.com/anthropics/skills.git temp
cd temp
git sparse-checkout set document-skills/pdf
mv document-skills/pdf ../
cd ..
rm -rf temp
```

## Using Skills

Skills are invoked automatically based on your task. Example:

```
> Extract form fields from this PDF
[Gemini invokes the pdf skill automatically]
```

## Configuration

The extension uses `~/.skillz` as the default skills directory (skillz's built-in default).

### Using a Custom Skills Directory

To use a different skills directory, edit the extension configuration:

**Edit `~/.gemini/extensions/skillz/gemini-extension.json`** and add your custom path to the args array:
```json
{
  "mcpServers": {
    "skillz": {
      "command": "uvx",
      "args": [
        "skillz@latest",
        "/absolute/path/to/your/skills",
        "--verbose"
      ]
    }
  }
}
```

**Important notes:**
- Use an absolute path (e.g., `/Users/you/my-skills`)
- Tilde (`~`) won't expand - use full path like `/Users/yourusername/.skillz`
- Or use `$HOME/.custom-skills` if your shell expands it before Gemini CLI sees it

Then restart Gemini CLI.

## Requirements

- **uvx** — Installed automatically with uv package manager
- **Gemini CLI** — Latest version recommended

## Troubleshooting

**Skills not loading**:
- Check your skills directory exists (default: `~/.skillz`)
- Verify SKILL.md files have valid YAML frontmatter
- Run `/mcp list` to see if skillz server connected

**Skills not recognized**:
- Restart Gemini CLI after adding skills
- Check skillz server logs with Gemini CLI debug mode

**uvx command not found**:
- Install uv: `curl -LsSf https://astral.sh/uv/install.sh | sh`

**Custom skills directory not working**:
- Verify you're using an absolute path (no `~` tilde)
- Check that you edited gemini-extension.json correctly (see Configuration section)
- Ensure the path exists: `ls -la /your/custom/path`

## About

This extension packages the [skillz MCP server](https://github.com/intellectronica/skillz) for Gemini CLI, enabling the same Agent Skills format used in Claude.ai and Claude Code.

Repository: https://github.com/intellectronica/gemini-cli-skillz

Skills are loaded from PyPI via `uvx skillz@latest`, ensuring you always have the latest version.

## License

Same as skillz — see repository LICENSE file.
