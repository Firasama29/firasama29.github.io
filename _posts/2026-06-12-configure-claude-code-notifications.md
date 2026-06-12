---
title: Configuring Claude Code hooks to Receive Notifications
date: 2026-06-12
categories: [AI, Claude-code, software development]
tags: [Claude-code]
---

I've been using claude code for development on side projects for a while and I started experimenting with claude's mobile app.
There are two ways I've followed to develop with claude code from the mobile app:
- **mobile-only sessions**: this is where I only worked from the mobile app. By default, sessions initiating from the mobile app itself are not accessable from the CLI but they are shared with the desktop app. There seems to be a way to pull mobile sessions into the CLI but that is a different topic.

- **CLI sessions**: sessions started from the terminal can be accessed outside in two ways:
    - `/teleport` allows access to claude.ai/code
    - `/remote-control` allows access to the mobile app

At times I've had multiple active sessions on different projects simulatneously, from the terminal and mobile app. It became difficult to track progress of each task and managing them became a challenge, especially because I do not know when one session completes a task or another requests my input. This is what drove me to find a solution or a way to minimize the clutter.

Notifications or alerts seemed to be one obvious way to make multi-session work more manageable by alerting me whenever a task reached a certain point and I have to take action. They are a key component in any workflow. The problem is that by default, Claude Code does not really have a general notification system across environments. In the terminal, there's a visual cue (the star on the terminal tab, signaling that a session requires some attention), but with a load of active tasks, this doesn't cut it, so additional hook configurations are often used to ensure events such as task completion or attention requests are not missed.

I solved the notification issue with the help of Claude in two ways, which I will discuss in the next sections.

## Terminal Notifications
I enabled notification sounds in the terminal. This is specific to the terminal itself, so not applicable to mobile and desktop apps. This involved finding the appropriate system sounds on Linux (I picked `paplay` sound) and adding them as hooks in `~/.claude/settings.json`:

```json
    "hooks": {
        "Stop": [
            {
                "hooks": [
                    {
                        "type": "command",
                        "command": "paplay /usr/share/sounds/freedesktop/stereo/bell.oga 2>/dev/null || true"
                    }
                ]
            }
        ],
        "PermissionRequest": [
            {
                "hooks": [
                    {
                        "type": "command",
                        "command": "paplay /usr/share/sounds/freedesktop/stereo/window-attention.oga 2>/dev/null || true"
                    }
                ]
            }
        ]
  }
```

I've added two Hook events, which execute the defined commands when triggered:
- `Stop`: triggers when Claude finishes a response/task.
- `PermissionRequest`: triggers just before a permission dialog is shown to the user, asking user to allow/deny it.

Hooks are simply user-defined shell commands that execute at specific points of Claude Code's lifecycle, so they work well for this purpose.
But there's another problem now. **What if I'm away from the terminal entirely?**
I would need something more, a setup to extend the workflow beyond the terminal, and the only obvious solution is enable notifications on my mobile. 


## Push Notification
There's a mobile app called `ntfy` that Claude suggested, useful for this use case and it can be added in Hooks to activate push notifications functionality on mobile. 
`ntfy` is a lightweight, HTTP-based pub-sub notification service. It works by sending push notifications to mobile via scripts, integrates cleanly with Claude hooks and requires no account to work. It's a clean and simple approach. Here's how it's configured:

- install `ntfy` from the app store
- add a _subscription_ (click on the + sign at the top right) and choose a unique name (e.g. claude-alerts)
- add this to `~/.claude/settings.json`:

```json
    {
        "hooks": {
            "Notification": [
                {
                    "hooks": [
                        {
                            "type": "command",
                            "command": "curl -d \"Claude needs your attention\" ntfy.sh/YOUR_TOPIC_NAME"
                        }
                    ]
                }
            ]
        }
    }
```
The above `Notification` hook can be configured to respond to specific notification events, such as permission requests or idle states.

To receive notifications when Claude finishes a task, I've added it to the `Stop` hook alongside `paplay`, so that it plays a sound and sends a mobile notification:

```json
    {
        "hooks": {
            "Stop": [
                {
                    "hooks": [
                        {
                            "type": "command",
                            "command": "paplay /usr/share/sounds/freedesktop/stereo/bell.oga 2>/dev/null || true && curl -s -d \"Claude task done in $(basename $(pwd))\" ntfy.sh/my-claude-alerts"
                        }
                    ]
                }
            ]
        }
    }
```

That's it. Integrating `ntfy` with Claude hooks is a small but meaningful improvement to my multi-session workflow - no more guessing if a task finishes or needs my input. It's not a complete solution to managing multiple sessions, but definitely a solid start.


telegram

--this is after installing the plugin and creating the bot and other steps
 Current status:
  - Token: set ✓
  - Access policy: not configured yet → defaults to pairing with an empty allowlist
  - Allowed senders: none yet
  - Pending pairings: none

  Next steps:
  1. Run /reload-plugins (or restart the session) so the server picks up the new token.
  2. DM your bot on Telegram — it'll reply with a pairing code. 
  3. Approve yourself with /telegram:access pair <code>.
  4. Once you're in (and anyone else who needs access), we should lock the policy down with /telegram:access policy allowlist so no one else can trigger pairing codes.
