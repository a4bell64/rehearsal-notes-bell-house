# #3 Lights coming on in empty rooms

**The problem:** room lights switched on with nobody there, at the same moments every day.
**The cause:** the blinds. A moving blind is a large moving object, and presence sensors reported it correctly.
**The fix:** tell the house when the blinds are moving, and have the light rules wait. No sensor was replaced.

Full story: [Rehearsal Notes #3 on the Home Assistant forum](https://community.home-assistant.io/t/rehearsal-notes-from-bell-house-3-lights-coming-on-in-empty-rooms/1029340)

## What we measured

- **Several sensor types tripped on the same blinds:** an Everything Presence Pro, an Everything Presence Lite and a Sonoff SNZB-06P24 (all mmWave radar), and a Third Reality R2 (an ordinary PIR motion sensor), which turned the lights on. On one evening close, the EP Pro tripped 0.9 s after the blind started moving and the EP Lite 1.5 s after.
- **One type didn't:** the Aqara FP300 has not tripped on a moving blind in any of our tests.
- **Guest room, one day:** the EP Lite turned on 1 to 7 seconds after six of the six blind movements. An Aqara FP300 in the same room ignored all six. (One room, one day. Interesting, not proof.)
- **Dining room, one evening:** a new hot-afternoon rule moved the blinds up to four times an hour. The radar tripped about 3 s after each close and the lights came on at full brightness, three times in one hour, in an empty room.

## How it works

| Piece | File | What it does |
|---|---|---|
| Helper | (create in Settings → Helpers) | A toggle, `input_boolean.blinds_moving` |
| Watcher | [blinds_moving_watcher.yaml](blinds_moving_watcher.yaml) | On while any listed blind moves; off 90 s after the last movement |
| Scheduled sweep | [scheduled_sweep_example.yaml](scheduled_sweep_example.yaml) | Raises the flag *before* a scheduled open or close starts |
| Room lights | [room_light_example.yaml](room_light_example.yaml) | Turn-on waits while the flag is on; turn-off is never blocked |
| Safety net | [safety_net.yaml](safety_net.yaml) | Clears a flag stuck on for 10 minutes, or left on at startup, and tells you |

Four details that matter:

1. **The watcher is its own automation.** A blinds automation in `mode: restart` cuts short any delay inside it, which can leave the flag stuck on.
2. **Only the turn-ON branch checks the flag.** A flag that blocked turn-off could hold a light on.
3. **The check is written as "not on".** If the helper is ever unavailable, the lights still work and only the old phantom comes back. That's the safe way to fail.
4. **The flag turning off is a trigger.** Someone who walks in during the 90 s window still gets their light when it clears.

Why 90 seconds: it covers blind travel plus the longest sensor hold we measured (about 73 s on an EP Lite).

## How it has held up

- In use since September 2026, now in six rooms. One bedroom is deliberately left without it so we can compare.
- **One week (Oct 11–18, 2026):** the flag went up 52 times and came down on its own every time, within about two minutes. The safety net never had to step in.
- **The day after the dining room fix:** two heat closes; the radar tripped about 3 s later both times; the light stayed off both times.

## The general rule

When a machine you control is what's moving (a blind, a garage door, a robot vacuum), tell the house it's moving. Don't replace the sensor.

## Known limits and open questions

- A person who walks in during the 90 s window waits up to 90 s for light, unless the room also uses a door sensor or another quick trigger.
- Blinds moved by a physical remote are only covered if the watcher lists those blinds.
- Is a fixed 90 s best, or should the flag wait for every listed blind to report "stopped"? Suggestions welcome.

*The YAML here is simplified from what runs at Bell House, with example entity names. The real versions carry extra house-specific conditions (sleep mode, guest mode, someone home).*
