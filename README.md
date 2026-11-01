# Chat Aquarium

A little pixel-art fish tank along the bottom of the screen, as an OBS browser source, and everyone who chats
gets a fish. A new chatter's fish drops in from above with a splash; after that it swims about the tank on its
own, wandering between the seaweed, the rocks and the coral, and now and then leaping out of the water.

- When they chat again, their fish darts about and blows bubbles (or leaps, if it's near the top), and what they
  said pops up in a speech bubble over it.
- `!feed` sprinkles fish food in, and every fish nearby races for it.
- A treasure chest pops open now and then in a burst of bubbles, and an air stone bubbles away.
- A chatter who has gone quiet for a while (20 minutes by default) swims off the side of the tank.

Everything is a full 3D voxel model, drawn at a low resolution and seen flat on with no perspective, so it looks
like 2D pixel art; only the sand shows a little depth (the 2.5D look). Fish turn round in 3D, so you see their
face as they turn. The same chatter always gets the same fish (its shape, fins and markings), in their chat
color. The tank's layout is the same every time for the same channel.

It shares its settings, chat and voxel code with [Chat Slam](https://github.com/Ixitxachitl/wrestle-game) and
[Duel](https://github.com/Ixitxachitl/duel-game), at a finer pixel size.

Lives at <https://aquarium.ixitxachitl.com/>.

## Setup

In OBS: **Add → Browser**, URL `https://aquarium.ixitxachitl.com/?channel=yourchannel`, at your canvas size
(e.g. 1920×1080). The background is transparent inside OBS.

The tank takes the bottom 10% of the screen; **Tank height** (`height`) and **Tank width** (`width`, centred)
change that. **Pixel size** (`pixel`) is 5 at 1080p unless you set it.

Right-click the source → **Interact** and move the mouse over it:

- The bar in the bottom-right corner adds a fish by name, feeds them, or empties the tank.
- The **gear** opens the settings: your Twitch / Kick channel, TikTok or YouTube, how long fish stay, the most
  fish at once, names and speech bubbles, bots to leave out, the feed command and its cooldown, the tank's size,
  the water's tint and the volume.

Every setting can also be a URL parameter, which overrides it. The full list is in the comment at the top of
`index.html`. `?demo=1` has fake chatters swim in and chat, for trying it out.

## Chat

| Command   | What                                                                                      |
| --------- | ----------------------------------------------------------------------------------------- |
| anything  | your fish joins the tank, or darts about and blows bubbles if it's already there          |
| `!feed`   | sprinkles in fish food (30-second cooldown by default; the broadcaster and mods skip it) |

- **Most fish at once** is 40 by default. When the tank's full, the fish whose chatter has been quiet longest
  swims off to make room.
- **No fish for** leaves out the usual chat bots (Nightbot, StreamElements and friends); add anyone else there.

Chat is read anonymously: Twitch and Kick directly, YouTube with a Data API key (or a WebSocket bridge), TikTok
through a bridge such as TikFinity.

Custom integrations (Streamer.bot, etc.) can call `window.aquarium.add(name, { color, text })`,
`window.aquarium.feed()`, `window.aquarium.remove(name)` and `window.aquarium.clear()`.

## Screenshots

Headless Chrome doesn't run animation frames under `--virtual-time-budget`, so add `timer=1` (frames on timers
instead):

```sh
chrome --headless=new --hide-scrollbars --window-size=1920,1080 --use-angle=swiftshader --enable-unsafe-swiftshader \
  --virtual-time-budget=20000 --screenshot=shot.png "https://aquarium.ixitxachitl.com/?demo=1&volume=0&timer=1"
```
