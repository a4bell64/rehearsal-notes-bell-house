# #4 The floodlight that wouldn't turn off

**The problem:** floodlights left on long after they were needed, and one floodlight that latched on at dusk and ignored every "off".

**The cause:** two different things. Lights that Home Assistant turns on don't get the floodlight's own 15-second shutoff. And one unit started hanging, and only a power cut cleared it.

**The fix:** a backstop that turns off any floodlight left on with no motion, and if a light refuses twice, restarts the PoE switch port that powers it.

Full story: Rehearsal Notes #4 on the Home Assistant forum (link coming)

## The setup

Eight UniFi floodlights, each powered over PoE from a UniFi switch and each paired with a UniFi camera, in "motion when dark" mode. Everything the fix needs comes from two local integrations:

- **UniFi Protect:** each floodlight's light, its motion sensor, its "is dark" reading, and its paired camera's motion sensor.
- **UniFi Network:** a "power cycle" button for every PoE switch port.

## How it works

| Piece | File | What it does |
|---|---|---|
| Backstop | [floodlight_backstop.yaml](floodlight_backstop.yaml) | Finds lights on 15+ minutes with no recent motion, sends "off" twice, restarts the port if it still refuses, sends one alert |

Five details that matter:

1. **Motion means hands off.** A floodlight held on by its own motion sensor doesn't obey "off", and it shouldn't. Our first version tried anyway at 2:26 AM and sent a false "STILL ON" alert. Now a light whose own sensor *or* its paired camera saw motion in the last 60 seconds is left alone.
2. **A 5-minute sweep, not just a 15-minute trigger.** A light skipped for motion has to be looked at again, or a light that saw a passing car at minute 15 would stay on all day. The sweep also catches lights still on after a Home Assistant restart.
3. **Two "offs" before any restart.** The port restart is the last resort, not the first move.
4. **At most one restart per port every 2 hours.** The rule reads the button's own last-pressed time, so a truly broken light can't cause a restart loop.
5. **One alert, then wait.** If a light is still on after everything, the run waits up to 30 minutes for it to go off. With `mode: single`, that stops an alert every 5 minutes about the same light.

A missing or unavailable motion sensor counts as "no motion", so a dead camera can't block the backstop.

## How it has held up

- **Built October 11, 2026**, after two floodlights sat at full brightness for 17 minutes after sunrise.
- **Revised October 12** after the 2:26 AM false alarm (the "motion means hands off" rule).
- **Port restart added October 14.** The first automatic port restart came on October 15 at dusk: the light went dark, came back about 20 seconds later, and stayed off.
- **Since Friday, October 16:** the backstop has cleared that one light nine times with no one touching anything. The other seven floodlights haven't needed it once.
- The one misbehaving unit is being replaced under warranty. The backstop covers it until then.

## The general rule

When a device won't take a command, cutting its power is often the most reliable reset there is. If it's on PoE, you already have a remote power switch for it. Use it, but with a limit and an alert, so it fixes the rare hang without hiding a real failure.

## Known limits and open questions

- The light-to-camera pairing and the light-to-port map are written out by hand. Move a light to another port or pair it with another camera, and you have to update the YAML.
- While one run waits on a refusing light (up to 30 minutes), other lights wait too. We accepted that because refusing lights should be rare.
- Would you rather get the alert and decide yourself, instead of an automatic port restart? Suggestions welcome.

*The YAML here is simplified from what runs at Bell House, with example entity names (two lights instead of eight). The real version also stands down while our alarm system is holding the floodlights on.*
