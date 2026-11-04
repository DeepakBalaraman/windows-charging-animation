# Charging Animation

Charging Animation is a Windhawk mod for Windows. Plug the charger in or pull it out and it answers with one of six animations over the desktop, a card with the battery level, and a sound if you want one. The three parts are independent, and each one has its own switch for each event, so you can keep the full effect, or just the card, or just the sound.

Most Windhawk mods hook into another program. This one does not need a host: it starts its own copy of windhawk.exe, watches the power line from there and draws everything itself.

[sample.webm](https://github.com/user-attachments/assets/600c0655-646e-4bf9-aa80-66d7bec91260)

## Requirements

- Windows 10 or Windows 11.
- Windhawk installed, with the mod enabled in it.
- Windhawk 2.0 or newer if you want the color picker in the settings. Older versions still accept a value in that field, it just shows as plain text.

## Installing

The mod is one file, `charging-animation.wh.cpp`.

1. Install Windhawk if it is not on the machine yet.
2. In Windhawk, create a new mod and open it for editing.
3. Copy everything in `charging-animation.wh.cpp` and paste it over the contents of the new mod.
4. Save the mod so Windhawk compiles it, then switch it on in the Mods list.
5. Open its settings and choose the animations, the card and the sounds you want.

The settings list stays short because rows your choices do not use hide themselves. Bezel, for example, has no starting point and no direction to travel, so those two rows disappear while it is selected.

## What happens on a power event

The mod watches the AC line rather than the battery percentage, so it fires when the cable goes in or comes out, not when the level passes some number.

For each event it can:

- play an animation on the monitor the pointer is on,
- show a battery card for a few seconds, refreshed once a second,
- play a .wav file or a system sound.

Each of the three has a "when plugged in" and a "when unplugged" switch of its own, in its own section of the settings.

## Animations

| Animation | What it does | Captures the desktop |
| --- | --- | --- |
| Wave (Refraction) | Bends a picture of the desktop outward along a ring that grows from `Origin`, tinted with the event color. | Yes |
| Linear Wave (Refraction) | The same bend with a straight front. Two fronts leave `Origin` together and run along the line it sits on, one each way. | Yes |
| Edge Glow (Neon Border) | A band of light swells out of the screen borders while a bright spot travels around them. | No |
| Bezel (Screen Push) | A dark frame grows out of all four edges and the whole picture slides inward to fit inside it. | Yes |
| Circuit Trace (PCB Glow) | A circuit board wakes along the bottom of the screen and charge pulses run along its traces. The layout is different on every event. | No |
| Matrix Code (Character Rain) | Columns of characters drop down the screen, released from `Origin` outward. | No |

A few details worth knowing before you choose:

- The two wave effects take one picture of the desktop when the animation starts. Anything moving behind them holds still for the second or so they run, which the bend hides well.
- Edge Glow never touches the desktop. Set its color to `rainbow` and the band becomes a gradient running around the screen.
- Bezel shrinks the picture by the same amount both ways, so nothing is stretched and the frame comes out a little thinner across the top and bottom. It is plain black and gray with no tint, and it gives up after about two seconds, because the shrunk picture is only a still image.
- Circuit Trace has no starting point, so it ignores `Origin`. It does have a direction: on an unplug, `Reverse` sends each pulse the other way, so the board drains instead of charging.
- Matrix Code gives up after about two seconds as well. Its characters are drawn from printable ASCII, so no font can leave a missing glyph box behind.

## Settings

There are three groups of settings, plus one switch at the bottom for quiet mode.

### Background Effect

| Setting | Values | Default |
| --- | --- | --- |
| When plugged in | On or off | On |
| When unplugged | On or off | Off |
| Animation | The six animations above | Wave |
| Plugged-in color | Any format from the color table | Windows accent |
| Unplugged color | Any format from the color table | Windows accent |
| Plugged-in / Unplugged custom color | Any format from the color table | `#4285F4` and `#EA4335` |
| Origin | Nine spots on the monitor, or `Cursor Position` | Bottom |
| Unplug direction | Forwards or Reverse | Reverse |
| Strength | 0 to 40 | 16 |
| Duration | 300 to 5000 ms | 1650 |
| Motion | Smooth, Linear, Ease In, Ease Out, Ease In And Out | Smooth |
| Opacity | 0 to 100 | 100 |
| Dim screen | 0 to 90 | 0 |

The two custom color fields only appear when that event's color is set to Custom. `Origin` is hidden for Bezel and Circuit Trace, `Unplug direction` is hidden for Bezel, and `Motion` only shows for the three effects that use it.

`Strength` means something slightly different in each effect: how far the desktop bends, how wide the light band is, how deep the frame pushes in, how thick the traces are, how long the cascade trails behind its head. At 0 the wave keeps only its glow.

`Origin` is where an effect starts, either one of nine spots on the monitor or the pointer's own position at the moment the event fires.

`Duration` is how long the animation runs, with two exceptions: Bezel stops at 2200 ms and Matrix Code at 1800 ms whatever this says, since both are a burst rather than a slow move.

`Dim screen` darkens the monitor the animation is on, the way a phone dims its display when it starts charging. It fades in over the first quarter second, holds while the animation runs and fades back out at the end. The card sits above the dim and stays bright. The range stops at 90 so the screen never goes fully black.

`Unplug direction` decides which way an unplug travels. `Forwards` plays it exactly like a plug-in, out from the origin. `Reverse` starts at the screen edges and collapses back toward the origin, like the charge being pulled away.

### Sounds

Both fields take an absolute path to a .wav file, or a system sound alias such as `Notification.IM` or `WindowsBackground`. Leave a field empty for silence.

A value with no backslash and no colon in it is read as a system alias, anything else as a file path. A file that is not there, or an alias Windows does not know, plays nothing at all.

### Stats Panel

| Setting | Values | Default |
| --- | --- | --- |
| When plugged in | On or off | On |
| When unplugged | On or off | On |
| Style | The five cards below | Fluent |
| Position | `Follow Effect Origin`, the corners, the edges, `Center` or `Cursor Position` | Follow Effect Origin |
| Accent color | Match Effect Color, Custom, a named color, or Windows Accent Color | Match Effect Color |
| Custom accent color | Any format from the color table | `#4285F4` |
| Details | On or off | On |
| Duration | 1000 to 15000 ms | 4000 |
| Opacity | 0 to 100 | 100 |
| Font size | 10 to 48, in pixels at 100% display scaling | 20 |

`Follow Effect Origin` puts the card where the animation started, so it moves with `Origin`. The other choices are fixed spots. Wherever it lands, the card stays inside the working area, so the taskbar is not covered.

`Details` adds the status line under the number. On a charge it reads something like `Charging · 1h 20m to full · 65 W`, and on battery `On Battery · 3h 10m left`. The time and the wattage only appear when the battery reports a rate for them, so a machine that cannot measure one shows the status on its own.

The five styles:

**Fluent (Windows 11-ish)** is the default. The desktop behind it is blurred once when the card appears and covered with the theme color, with a thin border and a soft shadow. A battery icon sits in a tinted tile next to the percentage, the status line goes underneath, and a level bar fills from empty as the card slides in and turns red below 20% while on battery.

**Fluent (Minimal)** is the same card at badge size, with half the padding, a smaller number, no status line and a thinner bar.

**PineApple (Card)** is the plain system HUD: a rounded square, as wide as it is tall, black in dark mode and white in light mode, with a large battery icon on top, the percentage below it, the status line under that and a capsule bar along the bottom. The accent color is not used in it, and the icon and the bar turn red when the battery runs low. It fades and pops in instead of sliding, and `Center` is the usual place to put it.

**Galaxy UI** is the wide soft card, with corners that take about a third of its height. The percentage is twice the normal size, with a bolt in a round tinted badge beside it, the status line underneath and one thin bar along the bottom edge. On an unplug the badge shows a battery instead of the bolt.

**PineApple (Circle)** is the phone charging screen: a thick ring that fills clockwise to the battery level, a bolt and the percentage in the middle, and the status line on a frosted pill underneath. The middle of the dial is a soft patch of the blurred desktop, so the number sits on a little frost instead of bare wallpaper, and the patch fades away before it reaches the ring. The number and the bolt carry a soft shadow that keeps them readable on a bright background.

The Fluent and Galaxy cards follow the Windows app theme, and the PineApple card flips between black and white with it. All five refresh once a second while they are up.

### Colors

`Plugged-in color` and `Unplugged color` accept any of these:

| Format | Example |
| --- | --- |
| A name | `purple`, `mint`, `gold` |
| Short hex | `#48F` |
| Hex | `#4285F4` |
| RGB | `66,133,244` |
| Windows accent | `accent` |
| Rolling gradient | `rainbow` |

The names are blue, red, green, yellow, orange, purple, violet, pink, magenta, cyan, teal, white, gold and mint, in any capitalization. A name that is not recognized falls back to the event default, blue when plugging in and red when unplugging, so a typo does not break the animation. `rainbow` rolls a gradient through the effect for as long as it runs.

### Quiet mode

Quiet mode asks Windows whether now is a good time for a notification. While a full-screen app, a game or a presentation is in the foreground, the animation, the sound and the card are all skipped, and the next power event after that plays normally.

## Notes and limits

- Everything happens on the monitor the pointer is on. `Cursor Position` follows the pointer itself, and the other origin spots are on that monitor.
- A change of resolution, display scaling or monitor setup stops a running animation instead of letting it draw in the wrong place. The next power event starts fresh.
- If the machine is slow enough that frames start taking too long, the two wave effects render at a coarser pixel step until the load passes, so the motion stays smooth instead of stuttering.
- Some full-screen protected video comes back black when the desktop is captured. The effects that do not capture anything, Edge Glow, Circuit Trace and Matrix Code, always draw normally.

## Troubleshooting

- **Nothing happens when I plug in.** The mod reacts to the power source switching, not to the battery level, so a desktop with no battery never triggers it. On a laptop, check the "when plugged in" switch in each of the three sections, and check quiet mode: a full-screen game or a presentation pauses the whole mod until it closes.
- **The sound plays but the screen stays still.** The animation is switched off for that event.
- **The card jumps around.** `Follow Effect Origin` puts it wherever the animation started, and that depends on `Origin`. Pick a fixed corner if you want it in the same place every time.
- **The sound does not play.** A value with no backslash and no colon is read as a system alias, so check the spelling, or give a full path to a .wav file.
- **Bezel or Matrix Code ends too early.** Both stop at about two seconds by design. Raising Duration does not extend them.

## Credits

The wave effect is inspired by the [Magic Pointer mod](https://windhawk.net/mods/magic-pointer). The layered window handling, the drawing surface and the rasterising helpers started from that code and were adapted for this one.

## License

MIT. See [LICENSE](LICENSE).

Copyright (c) 2026 Deepak Balaraman.

More from the author at [dpakb.com](https://dpakb.com), and on [GitHub](https://github.com/DeepakBalaraman).
