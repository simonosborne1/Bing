# bing

A Claude Code skill + settings hook that plays a Windows chime whenever Claude finishes a response and is waiting for your input — so you don't have to watch the screen.

## What it does

- Plays the Windows **Asterisk** system sound every time Claude stops and waits for input (`Stop` hook)
- Also chimes on permission prompts and auth events (`Notification` hook)

## Installation

1. Copy `SKILL.md` to `~/.claude/skills/bing/SKILL.md`

2. Add the following to `~/.claude/settings.json` under `"hooks"`:

```json
"Stop": [
  {
    "hooks": [{ "type": "command", "command": "powershell -Command \"[System.Media.SystemSounds]::Asterisk.Play(); Start-Sleep -Milliseconds 1200\"" }]
  }
],
"Notification": [
  {
    "matcher": "permission_prompt",
    "hooks": [{ "type": "command", "command": "[System.Media.SystemSounds]::Asterisk.Play(); Start-Sleep -Milliseconds 1200" }]
  },
  {
    "matcher": "auth_success",
    "hooks": [{ "type": "command", "command": "[System.Media.SystemSounds]::Asterisk.Play(); Start-Sleep -Milliseconds 1200" }]
  },
  {
    "matcher": "elicitation_dialog",
    "hooks": [{ "type": "command", "command": "[System.Media.SystemSounds]::Asterisk.Play(); Start-Sleep -Milliseconds 1200" }]
  }
]
```

## Usage

Once installed, use `/bing` to toggle sounds on or off:

```
/bing        # toggle
/bing on     # enable
/bing off    # disable
```

## Requirements

- Windows (uses `System.Media.SystemSounds`)
- Claude Code CLI
