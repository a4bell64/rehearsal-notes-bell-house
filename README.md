# Rehearsal Notes from Bell House

Real problems from a real Home Assistant house, and the fixes that held up.

Bell House is a home in Georgia (USA) run on Home Assistant: presence sensors in nearly every room, motorized blinds, cameras, a weather station, and a few hundred automations. I'm a retired band director, not a programmer. I build this with an AI assistant (Claude), and we test everything against the house's own history before calling it done.

The name comes from band rehearsal. A rehearsal is where you find the wrong notes, figure out why, and fix them before the concert. Each note here is one of those: something the house got wrong, the wrong first guess, and the fix that worked.

## The notes

| # | Note | What it fixes | Code | Story |
|---|---|---|---|---|
| 3 | [Lights coming on in empty rooms](lights-in-empty-rooms/) | Moving blinds fooling presence sensors | Yes | [Forum post](https://community.home-assistant.io/t/rehearsal-notes-from-bell-house-3-lights-coming-on-in-empty-rooms/1029340) |
| 4 | [The floodlight that wouldn't turn off](floodlight-backstop/) | Floodlights left on, and one that hangs until its power is cut | Yes | [Forum post](https://community.home-assistant.io/t/rehearsal-notes-from-bell-house-4-the-floodlight-that-wouldnt-turn-off/1029512) |

Earlier notes (stories only, for now):

- #1 The mailbox: [forum post](https://community.home-assistant.io/t/rehearsal-notes-from-bell-house-1-the-mailbox/1028709)
- #2 Trash day: [forum post](https://community.home-assistant.io/t/rehearsal-notes-from-bell-house-2-trash-day/1028746)

## How each note is laid out

Every folder has a plain-English README (what went wrong, why, how the fix works, how it has held up) and the Home Assistant YAML. The YAML uses example names like `cover.dining_left`; swap in your own entities.

## Suggestions welcome

If you see a better way to do any of this, please say so. Open an [issue](../../issues) or start a [discussion](../../discussions). That's the whole point of a rehearsal.

## License

MIT. Copy, change and share freely.
