# #5 Ruby and the window

**The problem:** a window check that sends a push when the glass moves but the window's open/closed sensor doesn't change. It works, but a dog bumping the glass sets it off too. Seven bumps in four minutes meant a stack of pushes.

**The cause:** the vibration sensor feels a shake. It can't tell a dog from a person, or a bump from a window being opened.

**The fix:** two small changes. At most one push every 10 minutes, and a "Pups Penned" switch that pauses the check on purpose when the dogs are gated, and turns itself off after 8 hours.

Full story: [Rehearsal Notes #5 on the Home Assistant forum](https://community.home-assistant.io/t/rehearsal-notes-from-bell-house-5-ruby-and-the-window/1029921)

## The setup

Each window has two sensors: a contact sensor (open or closed) and a vibration sensor on the glass. Contact sensors sometimes miss a close, so the glass sensor is used to double-check the contact sensor.

## How it works

| Piece | File | What it does |
|---|---|---|
| Window check | [window_glass_check.yaml](window_glass_check.yaml) | Glass shakes, contact doesn't change within 90 seconds: one push naming the window |
| Pups Penned | [pups_penned.yaml](pups_penned.yaml) | A switch that pauses the window check, turns itself off after 8 hours, and comes on by itself with Workers Mode |

Details that matter:

1. **Only while someone is home.** The push asks you to check the window, so it only makes sense when someone can.
2. **Skip if the contact already changed.** If the contact changed in the minute before the shake, it caught the move. Nothing to report.
3. **90 seconds to catch up.** Contact sensors can be slow. If it reports the change within 90 seconds, no push.
4. **One push per 10 minutes.** A burst of bumps becomes one message. The trade-off: a real missed window within 10 minutes of a bump goes unannounced.
5. **The pause is a switch a person sets.** It shows on the dashboard, says when it ends, and turns itself off after 8 hours, so it can't be forgotten on.

## The idea we rejected

The first suggestion was to alert only when someone's phone is in that room. That was turned down: an intruder working a window doesn't care whether you're in the room. A check that depends on where your phone is misses the one case it was built for. A pause should be on purpose, for a known reason, and it should expire.

## How it has held up

- **Built October 19, 2026.** A knock test that night (no window opened) sent the push about 90 seconds later, as expected.
- **October 21:** Ruby bumped a bedroom window 7 times in 4 minutes. The contact correctly read closed the whole time; the pushes were the glass sensor doing its job on the wrong cause.
- **Same morning:** the 10-minute limit and the Pups Penned switch were added.
- **Not proven yet:** the 10-minute limit hasn't had a real trigger since.

## Known limits and open questions

- The sensor can't tell a dog from a person. That's why the pause is a human decision, not a guess.
- Away-from-home protection is the alarm's job, not this check's. Our alarm build is still in progress.
- How do you keep pets from setting off glass or window sensors without weakening an alarm? Suggestions welcome.

*The YAML here is simplified from what runs at Bell House, with example entity names (two windows instead of thirteen).*

*README drafted with Claude from the house's own logs; the decisions and the story are mine.*
