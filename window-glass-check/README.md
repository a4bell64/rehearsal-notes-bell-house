# #5 Ruby and the window

Full story with replies: [Rehearsal Notes #5 on the Home Assistant forum](https://community.home-assistant.io/t/rehearsal-notes-from-bell-house-5-ruby-and-the-window/1029921)

## The Setup
We have a total of 22 windows of those we have 16 that can be opened.  We have a simple door/window sensor on those that can open to show that a window is open or closed.  We also have, in addition to, a vibration sensor that shows movement or vibration. If the glass shakes but the open/closed sensor doesn't change within 90 seconds, the house sends a notification, because it may have missed the window moving.. (We added this after some sensors missed a "close").

## Wednesday morning
I had a crew/contractor coming in so I placed a babygate at one end of house for our two dogs Ruby and Ana.  The dogs have access to many of the windows including what we call the girls bedroom (even though both my girls have long moved out). Between 9:04 and 9:08 (4 minutes) there were 7 bumps (Window remained closed of course). I received several pushes (notifications). I also received notifications for my wife's Study (Study Angie). No more after 9:09 which is when I left. (the check only runs when someone is home)

## Two small fixes
Step 1, 9:15am, must have a "10 minute limit" or at most one push per 10 minutes.  I realize there is a trade-off: a real missed window bump would go unannounced.  Step 2, about 9:25 integrated a Pups Penned switch (on my main dashboard) had a cute Dog icon... which once activated pauses the check for all the windows and would turn itself off after 8 hours.  This was pressed along with Workers Mode. But then we changed it to where if I press the workers mode, it would automatically turn on pups penned.

## The idea considered, but rejected
Claude suggested only alerting when someones phone is in that room. I rejected it and said, "What if: An intruder working on a window doesn't care whether you are in the room".  I wanted a pause... that I knew was there that I know will stop (When card is pressed again to turn off Pups Penned or after 8 hours on its own).  Claude withdrew the idea... Away from home protection is the alarm's job, and we are still working on it (Alarmo)

## What I dont know yet
The 10 minute limit hasn't had a real test yet (hard to do).  The sensor cannot tell a dog from a person. (only movement). That is why the pause is a switch a person sets, not a guess...

## Question for Readers
Does anyone have any ideas keeping pets off glass or window sensors without weakening an alarm (Alarmo)?

## Files
| File | What it does |
|---|---|
| [window_glass_check.yaml](window_glass_check.yaml) | Glass shakes, open/closed sensor doesn't change within 90 seconds: one push naming the window. Only when someone is home, not while Pups Penned is on, at most once per 10 minutes. |
| [pups_penned.yaml](pups_penned.yaml) | The Pups Penned switch: turns itself off after 8 hours, and comes on by itself with Workers Mode. |

The YAML is simplified from what runs at Bell House, with example names (two windows instead of thirteen). Suggestions welcome: open an issue or a discussion.

*The story is mine, in my own words. The YAML and the Files notes were drafted with Claude from the house's own logs.*
