# Reversing CraftLandia's anti-cheat ModPack (Part 1)

Today's adventure is on reversing an anti-cheat Minecraft 1.5.2 modpack

## Introduction

Before we get started, I should probably introduce you to CraftLandia, the biggest Brazilian Minecraft server.

![craftlandia website](https://i.imgur.com/J1eGfyF.png)

It is one of those generic pay-to-win[^brazilispoor] survival servers with many things:

- Player shops
- Warps
- PvP arenas
- PvP events
- Etc..

## Introduction II

CraftLandia has multiple different servers (at the time of writing, that number is 3), but today we will be focusing on the Legacy server.

| Server Name | Game Version   |
| ----------- | -------------- |
| Legacy      | 1.5.2          |
| Chronos     | 1.8.x - 1.12.x |
| Replay      | 1.8.x - 1.12.x |

Although Minecraft 1.5.2 is known for being very easy to cheat on, it's the  preferred version for some players due to the high prices of better computers.

Thus, they decided to make a client-side anti-cheat modpack designed to help preventing cheaters that they named "AntiHack"[^antihack]. 

## First dive into their modpack

Looking at the [thread](https://forum.craftlandia.com.br/xf/threads/anti-hack.950196/) where they announced the modpack, we can see all the mods[^craftlandiaislazy] they feature:
![mods list](https://i.imgur.com/Bq4G99k.png)

A whopping 6 mods, but this gives us some insight to what they are using. [The Macro / Keybind mod by Mumfrey](https://www.minecraftforum.net/forums/mapping-and-modding-java-edition/minecraft-mods/1275039-macro-keybind-mod) requires LiteLoader, so we can assume that they are using it at least.

Now that we have looked at what their modpack features, we will now want to run it.

## Running the client

After going to their website, downloading their launcher and running it, we are met with the following window[^yourfuncubed]:

![launcher window](https://i.imgur.com/n4dSsnx.png)

Legacy is the one we're interested, thus I will give it a click. After doing so, it begins downloading and verifying the hashes for all of the required files. This sets up a `.minecraft` folder locally:

![.minecraft folder](https://i.imgur.com/Kiva02u.png)

After it's all downloaded and ready to launch, we're then presented with this:

![boring launcher](https://i.imgur.com/3zrOJJ3.png)

This is just a generic offline mode launcher. After finally launching the game, we're greeted with this, a Minecraft 1.5.2 client:

![game](https://i.imgur.com/zUxuWir.png)

To give some insight, this is a modpack made with [MCP](https://minecraft.fandom.com/wiki/Programs_and_editors/Mod_Coder_Pack).

But how do I get to use it outside of their server? Well, let's see here.. There's a single button with the text "Play CraftLandia".
Let's click it!

Unsurprisingly, clicking it brings us to the multiplayer menu:

![multiplayer menu](https://i.imgur.com/WEttY8L.png)

So, lets see.. Maybe we can join a different server, like localhost.

Nope!

![nope](https://i.imgur.com/Fa4o75j.png)

It says that we typed an invalid IP. We'll check on this later.

So, being out of options, there's nothing else to do other than joining the server.

But before we do that though, let's first look into the `.minecraft` folder.

## Snooping around

Turns out that it's just a normal `.minecraft` instance.

![.minecraft 2.0](https://i.imgur.com/tOtOfUP.png)

But wait, what's that? We can look at the stitched items and terrain since that is outputted by the game.

![stitched items](https://i.imgur.com/0N8fBKI.png)

![stitched blocks](https://i.imgur.com/dK1QZxh.png)

Do you, the reader, notice anything weird? *I do.*

They have "backported" some items from newer versions and even added some new blocks, for some reason.

## Using the client

Now that we're done snooping around, we might as well join their server. Below we can already see[^laughing] some HUD elements. 

![in-game](https://i.imgur.com/UMbx9qI.png)

While exploring the mods, we see that they have some kind of ToggleSprint mod.

But beware that this is not ToggleSprint, but instead is Omni-Directional KeepSprint.

Whenever the player enables it, they sprint forever and can also sprint in all directions.

Another mod we can find is the Macros mod mentioned earlier.

![macros mod](https://i.imgur.com/TjVbMRI.png)

### Wait, what's that?!

![skins](https://i.imgur.com/wM33Dz6.png)

**Is that a skin in Minecraft 1.5.2?** *More on that later.*

![australia](https://i.imgur.com/UXgKXyd.png)

**HE'S UPSIDE DOWN?** HOW? *More on that later.*

---

And just like they said in their announcement thread, people using their client show up with a green tick in chat.

<img src="https://i.imgur.com/wpPEWYv.png" title="" alt="chat" width="462">

And non validated players show up with a red **X** instead.

<img src="https://i.imgur.com/KRDnLen.png" title="" alt="red x" width="462"> 

They also restrict warping into PvP-enabled places without their client.

![oops](https://i.imgur.com/w9rxTlU.png) 

> Oops! In order to go there, you need to be using the anticheat client!
> Download it in here: <link>

![noskin](https://i.imgur.com/k7lroHR.png)

Also, no skins nor upside down players without their client.

Here's a fun fact about 1.5.2: *The Toggle Perspective Keybinding was hard-coded to F5.*

## Reversing it

Well, now that we know how it works, we just need to reverse this. That's simple, right? ***RIGHT?!*** 

![obf](https://i.imgur.com/RkgT8zq.png)

It's obfuscated, but why? The answer is simple: **people do everything they can to cheat.**

Exercise time! Let's open [Process Hacker](https://processhacker.sourceforge.io/) and see what it is doing.. Uh, this isn't supposed to happen.. it just closed itself? *More in this later.*

Snooping around we can see that it's requesting the process list by using the `tasklist` executable.

![tasklist](https://i.imgur.com/PSGTP4W.png)



Let's see it in action:

[![](https://res.cloudinary.com/marcomontalbano/image/upload/v1647426204/video_to_markdown/images/streamable--967zzi-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://streamable.com/967zzi)

*Jump to the 40 seconds mark if you want to see it in action.*



This particular set of arguments passed to `tasklist` makes it so it takes a while to list all processes if there are many, thus it might take a while for the client to detect Process Hacker.

## Next step in reversing

*TODO - This will be seen in the next part of this boring writeup.*

## Final words

Hope you enjoyed this long info dump about a bad modpack made by an irrelevant server outside of Brazil.

## Footnotes

[^brazilispoor]: Brazil has that problem because of their economy.

[^antihack]: Literal translation of this would be anticheat.
  Brazilians have this weird addiction of mixing English names with non English ones.
  "Hack" here is referring to "a cheater", although the original English meaning is still kept on other instances.

[^craftlandiaislazy]: Honestly, this just screams laziness to me..

[^yourfuncubed]: **CraftLandia**
  Your fun cubed

[^laughing]: Keep in mind that Brazilians use kkk to laugh. More information can be found [here](https://en.wiktionary.org/wiki/kkk "https://en.wiktionary.org/wiki/kkk").
