<!-- PROJECT LOGO -->
<br />
<div align="center">

  <a href="https://github.com/FelixLusseau/discord-message-scheduler">
    <img src="docs/logo-black.svg" alt="Logo" width="180" height="180">
  </a>

<h3 align="center">Discord Message Scheduler</h3>

  <p align="center">
    A simple Discord Bot that can send scheduled messages.
    <br />
    <br />
  </p>
</div>



<!-- ABOUT THE PROJECT -->
## About The Project

![Schedule creator screenshot][product-screenshot]

Discord Message Scheduler provides a convenient bot interface to grant users the ability to create scheduled messages via the bot.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Features

* Supports both prefixed and slash commands
* Uses SQLite database
* Modern Discord Modal interface
* Easy to set up

### Built With

[![Python][Python.org]][Python-url] [![discord.py][Discord.py]][Discordpy-url]


<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started

Follow these instructions to deploy your own Discord Message Scheduler bot.

### Prerequisites

This bot is build on Python 3.10. You will need to download the latest version of Python using any of the methods below.

* From official website (Any OS): [https://www.python.org/downloads/](https://www.python.org/downloads/).
* Using APT (Debian/Ubuntu):
  ```sh
  sudo apt update
  sudo apt install python3.10 python3.10-dev software-properties-common
  ```

You will also need a Discord bot.
1. Create a Discord application at [https://discord.com/developers/applications](https://discord.com/developers/applications)
2. Within the settings page, navigate to the **Bot** tab and create a bot
3. (Optional) If you don't want others to invite your bot, then disable **PUBLIC BOT**.
4. Enable **SERVER MEMBERS INTENT** and **MESSAGE CONTENT INTENT** under the Privileged Gateway Intents settings
5. Navigate to **Oauth2** → **URL Generator** tab, click both **bot** and **applications.commands**, and check the following buttons:
   * Read Messages/View Channels
   * Send Messages
   * Send Messages in Thread
   * Send TTS Messages
   * Embed Links
   * Attach Files
   * Read Message History
   * Mention Everyone
   * Use External Emojis
   * Use External Stickers
   * Add Reactions
   * Use Slash Commands
6. Open the link in the **Generated URL** box at the bottom to invite your bot.


### Installation

1. Clone the repo
   ```sh
   git clone https://github.com/FelixLusseau/discord-message-scheduler
   ```
2. Install Python dependencies, choose a method from below:
   1. Using pip
      ```sh
      python3.10 -m pip install -U pip
      python3.10 -m pip install -U -r requirements.txt
      ```
3. Rename `.env.example` to `.env` and replace `BOT-TOKEN` with your bot's token and `BOT-PREFIX` with your desired bot prefix.
   Example `.env` file:
      ```dotenv
       TOKEN=BOT-TOKEN
       PREFIX=?
       SYNC_SLASH_COMMANDS=on
      ```
   `SYNC_SLASH_COMMANDS` should be set to "on" the first time you start the bot and every time you update. It should be set to "off" during normal usage since syncing slash commands may take a long time.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- USAGE EXAMPLES -->
## Usage

To start the bot, simply run:
```sh
python3 start.py
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- COMMANDS GUIDE -->
## Commands Guide

This bot accepts both prefixed and slash commands.


<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li> Schedule related:
    <ul>
    <li><a href="#schedule-create-channel"><code>/schedule create</code></a></li>
    <li><a href="#schedule-list-channel"><code>/schedule list</code></a></li>
    <li><a href="#schedule-show-event-id"><code>/schedule show</code></a></li>
    <li><a href="#schedule-edit-event-id-new-channel"><code>/schedule edit</code></a></li>
    <li><a href="#schedule-delete-event-id"><code>/schedule delete</code></a></li>
    </ul>
    </li>
    <li> General:
    <ul>
    <li><a href="#info"><code>/info</code></a></li>
    <li><a href="#help-category-or-command"><code>=help</code></a> (only prefixed command)</li>
    </ul>
    </li>
  </ol>
</details>

> The default bot prefix is `=`.

`[arg]` - Optional <br /> `<arg>` - Required

### `/schedule create [channel]`

Creates a scheduled message. You can optionally supply a `channel` argument to specify a channel for the message.

A Discord modal prompt will open, asking for the following:

- `Message` - The message that the bot should send at the scheduled time.
- `Scheduled Time` - The time to send the message, accurate to the second.

  Formats:
  - Just date: `24/2/2023` (Month/Day/Year), `December 12`, `nov 26 2023`
  - Just time: `1:12am`, `midnight`, `13:42`, `7pm`
  - Date and time: `24/02/23 19:31:03`
  - Other date formats: `March 30 2023 4:10pm`
  - Simple time: `tomorrow`, `next week`, `thursday at noon`
  - Slightly complicated relative time: `in 1 day, 2 hours and 10 minutes`
  - ISO 8601 format: `2023-08-11T01:59:41.981897`
- `Timezone` - The timezone to parse your time.

  Formats:
  - Formal [TZ database name](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones): `America/Los_Angeles`, `Europe/Berlin`, `Asia/Shanghai`
  - [Time zone abbreviation](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones): `GMT`, `UTC`, `CEST`, `PDT`, `EST`, `ET` (not recommended, may be inaccurate)
  - Timezone offset: `+12:30`, `-3000`, `+0123`, `UTC+1232`, `UTC-12:32`
- `Repeat` - The number of minutes between every repeat of the scheduled message. Set `0` to disable.

### `/schedule list [channel]`

Shows you a list of upcoming scheduled messages. You can optionally supply a `channel` argument to specify a channel to check for your upcoming scheduled messages.

### `/schedule show <event-id>`

Shows you the full details of a scheduled message. To find the `event-id`, use the [`/schedule list`](#schedule-list-channel) command.

### `/schedule edit <event-id> [new-channel]`

> Due to limitations with the bot, it is not possible to edit the time or timezone. You will need to delete and re-create the schedule event.

Edits the contents of an upcoming scheduled message. To find the `event-id`, use the [`/schedule list`](#schedule-list-channel) command.

To change the current channel, set `new-channel` to the channel that you want.

### `/schedule delete <event-id>`

Deletes/un-schedules an upcoming scheduled message. To find the `event-id`, use the [`/schedule list`](#schedule-list-channel) command.

### `/info`

Shows info about this bot.

### `=help [category-or-command]`

> This command is only available as a prefixed command.

Shows the help page. Optionally, set `category-or-command` to be a category or command name to view more info on the subject.

Note: `[category-or-command]` is case-sensitive.


<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* Inspired by [Taaku18/discord-message-scheduler](https://github.com/Taaku18/discord-message-scheduler)


<p align="right">(<a href="#readme-top">back to top</a>)</p>


[product-screenshot]: docs/demo.gif
[Python.org]: https://img.shields.io/badge/Python-3.10-45697E?style=for-the-badge&logo=python&logoColor=white
[Python-url]: https://python.org
[Discord.py]: https://img.shields.io/badge/discord.py-0769AD?style=for-the-badge&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAIrklEQVRYw61X+1NTZxo+8F/woyTkQgJJSM45QeSSixAu4SJahSpqvSAQkKso2nZ3rVqmW9dWIHhp1VKdqlQE62id3c4OrfXSWuttJVwE23GmZfoT3Rm3xfDs+30nJES0ujObmWcOIeec53nv7ycIz/jgXo6Af80iW5i5447FHbcet50+3HL04vusAG5mTuG79CBuLArim4VTuJ4awDV7L67KPlyR9MErcmzgfI6QUHlCcDb5BbHhoPBSHwx5iDRXwH2PMHMvO4YEyLi32I+77nHcdU3jrhO4lYXgjQz8di0dv11NQ/D6QuDbVIIduCZPk4BxfG3z/3jRKQvmn2PkusOCVPuBINYeegH5cJ6CAGEoN45EtJMXJnE/G8HbbkxcysC5ThntTRbUVSRh7VIj1pYaULfKiPaGZAzss2Ci34aZyyIef7kQb+5tnbTWHmkn8jiplkT4DpCQg88jLyDkEzlDnkgCBskbCN7LwY3eDLRVWZCZqoNWm4D4eDUWPIX4+ARoNQnIsGuxY0Mi2ncuQ1r9IUibPwSRD5L1IvOAVHNAEElINPmoVxEwwkU4yAvDBEx+vRhvt9ggpmg5qUqdgISEPwa7J16lRkJiMpKyV0KsfB9S3Qcg8mHRd9AhkgckX7cg1nSHyEeIfLRQuY54RRIyjJF8BD53YVVpMn+h+iWI50Gt5tDb3bCu3UMiDoMEDBOxyMgVdJCAsSJFwGhhHAkYxGgBhi65Uewxcqv/Z+J5QlTQWRfBumY3hYJCUtM9KNb44wgkoCsk4EFxDMYK2wmYvObByiXJ88npRRwaLX3XENRhK2fvYZ7ioVLNf1Yvu2DbsJe8cABE3i7XvB8jcQEPmIAimYRMBoe92L1Fio41I9XqoJecMC4uh8G1HNpkGzR6IzQGEyGZfk+EZNWjudKKxkobPC4jT8goISTU6C5j5AyTYnWXTBCEmfElsSTAj/EiXO9zw2bRzom5GjoxC+blrbBUvEXXrTCXtSFlzR5YK/fDVu2HraoT1vXvwlq+DZ7yMrzR6sSlHgd69qUjl4SE30UCNIlJsJTvACUhiNwvbeqIFTBerKcQjD8ZKcSWKhsvqUjs0mCroZvrPoS4+QjkrZ/Cvv2cgrYBQj+H3HYW8razsLWchnn9PjiXr8Shvzpw8/xieJyGiCfonYbMYiLvhFTdNS5VdeqZAB8miqfH/unBIrs+4n5SnFxSC/uOz4hwIERKhNsYzobQR8R9kLaegdT6KYdMf9uaTsJesgZ3LrjRvTstygtao4USchcJ8E+LVZ0+JqAXD4txpjsTGs2cG5NSyPoDEQHPIWeE8ix5ay+HRJ5KLa/HzXNujH6Rg1RJH1XKphIfVQPlQlVHLxMQIGBnixzlfuYqRhAmb3ua/Ey05WEBpyFtoVCV1eFGvwsssatXW6LebXS9wkIACkGABJRMTY8UYVNFSuQmSj7TKy2K9WHL+zmx1HyCCE9zyyPEJKLlJMTGHiI/RehF+oqNuP+5G5jwomfv3DCokLjQA3FTBwnomBIwURJ8fL8Qr5aaQrWvZKt1034l7iG3M0LzshaqikwY3Csg1h/jice8JNYf5f9jv/GKqT6MNRu8+PX7XGDMi1ufuZBiDlUX6wmSg1r0exSCzuB8AaHOJTUeVzKduZyutupuaIwpSFDF8xwxeNbAQuVooQ5nyFmtNCT6TUNJ5iktwOXTLrCuSvMFv1z3IM8VqoZ5AkIh2LgyFAIVuSjdy93KracQsDJMzS9FklEZSgy162w46XfiZJcDDRtt/OXs+VRJh+/6nQr5cD7H7/dysaHcrLyfhyCHQrCfCZgKJ+GfG0NJSFYYaYrNup7FOa10NS4czcLH72Ug22HAa2UWPLycB/xQDDwswqMruWSABY70RBx6eyFmAorlXEAgj2NHrTUswOhcFkrCjkC4DE91Zij9nDyQ7K0MZ721/iPklzjx6CrF84cSTN4owK93vaDuSfEljBaCzZB/387HT1c8mBku4NY/vpOLJ/cVcjba322TwvPFVFQdKsPO3nAjGv7Cw+tVNUcAbzJUWualjagokzF40on/DBEhCcYEkVP7xgP6/sCrgIh/JhH/6MnEN30OEkCiAwr2tIhcgMZghnX1zqhGxFvx9HAh6teTmxZQElpSYaXJpTSaPl5WplVvwbbYi7XlNt7d/v6xA9/2u3mzuXzKidMd6djVImHvDhlffpKF3+8S8QhZP5SLS0fSkSbruHEGyi8ijrTiucPoK8pci0nLw6A1STAta+YzgHmB9XvL6l1Q6ZJ4wul1GpiStDAna2FM1HDX6mhdO/63NE78+LYHQxedeGerxDcq9oxGb4BlxTY+jKTwMJozjqeHvHhjsxgqF4JGA11KGtX4ciRRWHS29Hnzn2POOsaqoH5dCt+mUkVd+B4+jp1L2RSMHsd8IRkrCS8kj77KwdKCpMhColZHlhH1izckRrYgVKpzu5/OlgHrunfIemUhkXz7YyIb0VMr2a1zLj5G/18rmdZsR8qqv/CVTOQrWXecshN2hZbSsflL6a0BB0rzkigf1C9l+XwozzHLUyp28qVUoqWU1nJaSg9EltLnreVPhgqwp6sZhuwKvn69bAhmQ6bRGXjDYXvgH67lzzyYPHCLZwaaBx1/Usow5bV2GByl1OfNColKNScv5oB+Y3uiIaMQlldf51bLzziYSE8fTOYezaaHCoTzFxoFU+vFOLntXDsNokm2E7BZb6URaqKJyJbTxLQ82nSdfFll49VIlWJasllJNCKVGz6i65FJsj7qaCbWvuCQ+svNUkFYPiZkvdknSNsHYqgjyjR2/dSUxgnTzCN86WiivYCR0FhmV7npuILGnmm54di4XH/UL28+IpOYGBIhMLzwcDr7EV8fEOTtCkiAYG/tiyVyPW1APmpKvSQgIG85NUVLSFBq/iQoN52YIvKA1NjTS2J8UsMxvb3haCwJ4MQk4rnH8/8CmJyMTDqH+n8AAAAASUVORK5CYII=
[Discordpy-url]: https://github.com/Rapptz/discord.py
