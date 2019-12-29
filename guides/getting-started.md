---
description: The definitive guide.
---

# Getting Started

Natsuko can be confusing for people sometimes, so this getting started guide is for teaching you how the bot works, what you can do with it, how to set it up, and finally, how to wield its power.

## Configuration

First off, Natsuko needs to be configured before all features are available. To do so, you need `n;config`.  
Setting options is as simple as:   
`n;config set <option> <value>`   
And you can always view the options with:  
`n;config show`  
As of yet, only the following options are available

```text
modrole <role>: Set the role required to use Natsuko moderation commands. Ex: @Mod
adminrole <role>: Set the role required to modify Natsuko settings. Ex: @Admin
mutedrole <role>: Set the role used with n;mute and n;unmute. Ex: @Muted
modlog <channel>: Set the Mod-Log channel. Ex: #modlog. 
strikes.kickthreshold <number>: Set the strikes threshold required to kick. Default: 2
strikes.banthreshold <number>: Set the strikes threshold required to ban. Default: 3
strikes.bantime <time>: Set the time for strike-autoban expiry. Ex: 10m. Default: -1m
                        Available time units: (m)inutes, (h)ours, (d)ays, (w)eeks, 
automod.antispam <on|off>: Set the status of anti-spam. Default: off.
automod.antispam.mpslimit <number>: Set the number of messages-per-second to trigger antispam on. Default: 3.
automod.antispam.threshold <number>: Set the threshold of messages-per-second limit hits to act upon. Default: 3.
```

Configuration is optional, However, Features will be unavailable if not configured.

## Modlog



