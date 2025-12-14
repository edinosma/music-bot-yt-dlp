# music-bot-yt-dlp
Modifications of code from SpaceCowboyZZ after I got tired of trying to make another bot work.

## Building:
Per usual, virtual environments are preferred, but not required.

```python
pip install -r requirements.txt
```

You must also [install ffmpeg](https://ffmpeg.org/download.html).

## Running:
```python
python main.py [path to bot token config] [optional: path to cookies file]
```

Example bot token file:
```
bot token here
```

## Potential Problems
YouTube will definently block you. You may have some initial luck, but they work fast.

[Use a throwaway account and extract the cookies per the yt-dlp wiki.](https://github.com/yt-dlp/yt-dlp/wiki/FAQ#how-do-i-pass-cookies-to-yt-dlp) You can then pass in the path to the cookies file.

If everything else fails, [see if yt-dlp has an update](https://github.com/yt-dlp/yt-dlp), then rerun the [Building](#building) section code again.

---
*Original message from SpaceCowboyZZ below.*

It's very basic, and I don't plan on making it more complex, but it still needs some improvements regarding the queue. More testing is needed, but it works.

Here's how it works:

It's very basic, but the more complicated part was making yt-dlp not break, since it isn't asynchronous. I made it run in threads using concurrent.futures, so if you send a really big playlist (100 songs, for example), you won't need to wait until yt-dlp fetches all the videos in the playlist before you can use the !play command again. The issue is that you'll need to wait until all the videos are fetched for them to be placed in the queue. So, if you use !play when there is already a playlist being searched, if it is a single video or a small playlist, it will be sent to the queue before the big playlist is fetched. I didn't implement any ways for the task to stop while they are still being executed because I don't really need it (lol) and because it works for me the way it is.

queue, actual_url, and thumb_url are used to store information about the video to send in the embed message when a song is currently playing. bot.playstatus is used to check if the bot is still in "play" mode, so the next song in the queue is played after the currently playing song ends. bot.doom is to make sure the bot stops after using !stop if there is a video or playlist being searched in the background. That's pretty much it.
