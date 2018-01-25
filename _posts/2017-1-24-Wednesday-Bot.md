---
layout: post
title: It is Wednesday My Dudes
---

On a random night in out software devs discord chat, we decided that we should work on a discord bot. Every Wednesday day one dude would always post the "It's Wednesday my dudes" meme. To make his life easier. I made the bot.

The bot takes advantage of a wrapper for the discord API. I started by using [discord.io](https://www.npmjs.com/package/discord.io) but there was an issue with a version mismatch between the repo and npm. I switched to [discord.js](https://discord.js.org/#/). Both had great straight forward documentation.

I wanted to make sure that we could expand the types of posts and replies that the bot could do with ease. Making a super basic middleware allows the bots code to add in different listeners while being fed in a message in one location.

The last feature, before it's initial release, was the ability for the bot to post memes along side it's replies. What would be the best way to pull good, funny, and relevent content? We looked at using giphy's API because you could query for gifs based on tags and who doesn't like a good gif?

![Gif Macho Man Randy Savage saying, "Oooh Yeah."](https://media.giphy.com/media/l2SpKLnUXi63DMyCQ/giphy.gif)

But the keywords we were using to query weren't bringing back the results of the frog or the meme that we were expected. It was going to best to pull form a curated collection of memes. Something like an imgur album or a youtube playlist

[Wednesday Bot Repo ](https://github.com/brooksbecton/ItsWednesdayMyDudesDiscordBot)
