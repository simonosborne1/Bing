---
name: bing
description: Toggle notification sounds for Claude Code. Plays a Windows chime whenever Claude finishes a response and is waiting for input, plus on permission prompts and auth events. Invoke with /bing on or /bing off, or just /bing to toggle.
disable-model-invocation: true
allowed-tools: Read, Edit, Write
---

Toggle notification sounds for Claude Code on this Windows machine.

## Instructions

1. Read `C:\Users\sosbo\.claude\settings.json` to get the current content.

2. Check `$ARGUMENTS`:
   - If "on" → add the hooks (even if already present, ensure they're there)
   - If "off" → remove the hooks
   - If empty → toggle: add if absent, remove if present

3. The `PermissionRequest` hook fires when a tool approval dialog appears:

```json
"PermissionRequest": [
  {
    "hooks": [{ "type": "command", "command": "powershell -Command \"[System.Media.SystemSounds]::Asterisk.Play(); Start-Sleep -Milliseconds 1200\"" }]
  }
]
```

4. The `Stop` hook fires every time Claude finishes a response and waits for user input:

```json
"Stop": [
  {
    "hooks": [{ "type": "command", "command": "powershell -Command \"[System.Media.SystemSounds]::Asterisk.Play(); Start-Sleep -Milliseconds 1200\"" }]
  }
]
```

4. The `Notification` hooks fire on permission prompts and auth events:

```json
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

5. Write the updated JSON back to `C:\Users\sosbo\.claude\settings.json`, preserving all other settings. Keep it valid, pretty-printed JSON.

6. Reply with a single short line:
   - "Notification sounds ON — you'll hear a chime whenever Claude is waiting for input."
   - "Notification sounds OFF."
