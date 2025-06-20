<h1 align="center">Void Base</h1>
<p align="center"><em>A simple yet flexible Discord bot base built with <a href="https://discord.js.org/">Discord.js</a> and <a href="https://www.typescriptlang.org/">TypeScript</a>.</em></p>

<p align="center">
  <a href="https://github.com/Fayaz-246/void-base">
    <img src="https://img.shields.io/github/stars/Fayaz-246/void-base?style=social" alt="GitHub stars">
  </a>
  <a href="https://github.com/Fayaz-246/void-base">
    <img src="https://img.shields.io/github/license/Fayaz-246/void-base" alt="License">
  </a>
  <a href="https://discord.js.org">
    <img src="https://img.shields.io/badge/discord.js-14.19.3-blue?logo=discord" alt="Discord.js">
  </a>
  <a href="https://www.typescriptlang.org/">
    <img src="https://img.shields.io/badge/typescript-5.8.3-blue?logo=typescript" alt="TypeScript">
  </a>
</p>

---

## Table of Contents

- [About](#about)
- [Credits](#credits)
- [Usage](#usage)
  - [Installation](#installation)
  - [Configuration](#configuration)
  - [Basic Features](#basic-features)
  - [File Formats](#file-formats)
    - [Slash Commands](#slash-commands)
    - [Buttons](#buttons)
    - [Modals](#modals)
    - [Menus](#menus)
  - [Events](#events)
  - [Database (DB)](#database-db)

---

## About

**Void Base** is a Discord bot foundation focused on simplicity, clarity, and speed. It's designed to help you get started quickly while maintaining flexibility for expansion.

Built using:

- [Discord.js](https://discord.js.org/)
- [TypeScript](https://www.typescriptlang.org/)

---

## Credits

- **Created By:** [Fayaz](https://fayaz.is-a.dev/)
- **Inspired By:** [MicroBase](https://github.com/MusicMakerOwO/MicroBase) by [MusicMaker](https://github.com/MusicMakerOwO/)
- **Special Thanks:** Mr. ChatGPT himself; learned a ton while building this.

---

## Usage

### Installation

Clone the repository, install dependencies, compile, and run:

```bash
git clone https://github.com/Fayaz-246/void-base.git
cd void-base
npm install
npm run build    # or: npm run build:unix (if you're on Linux/macOS)
npm start
```

**Note:** Rename `.env.example` to `.env` and add your bot token.

---

### Configuration

Configuration is done in `src/config.ts`. Using TypeScript for config gives you type safety and editor hints.

```ts
interface BaseConfig {
  embedColor: ColorResolvable;
  verbose: boolean;
  checkEventNames: boolean;
  logTables: boolean;
}
```

---

### Basic Features

- Full support for interactions (except context menus; coming soon!)
- Built-in caching via the `TimedCache` from [CachesJS](https://cachesjs.com/) _`src/lib/TimedCache.ts`_
- Response caching for static commands via `.setCached(true)`

---

## File Formats

Organize your files like so:

- [`src/slashCommands/{type}`](#slash-commands)
- [`src/buttons/{type}`](#buttons)
- [`src/modals/{type}`](#modals)
- [`src/menus/{type}`](#menus)

---

### Slash Commands

Slash commands **must** be placed in a subfolder inside `src/slashCommands/`.

**Example:**

```ts
// src/slashCommands/test/test.ts
import InteractionBuilder from "../../classes/interactionBuilder";

module.exports = new InteractionBuilder()
  .setName("test")
  .setDescription("An example command")
  // .setCached(true)
  .setRun(async (interaction, client) => {
    await interaction.reply({ content: "Heyo!", flags: 64 });
  });
```

The `InteractionBuilder` class is similar to `SlashCommandBuilder` with added methods:

- `.setCached(boolean)`
- `.setRun(callback)`
- `.build()`

---

### Buttons

**Directory:** `src/buttons/{type}`

```ts
import buttonFileBuilder from "../../classes/buttonFileBuilder";

module.exports = new buttonFileBuilder()
  .setCustomId("test")
  .setRun(async (interaction, args, client) => {
    await interaction.reply(`Hello from ${client.user?.username}`);
  });
```

---

### Modals

**Directory:** `src/modals/{type}`

```ts
import modalFileBuilder from "../../classes/modalFileBuilder";

module.exports = new modalFileBuilder()
  .setCustomId("test_modal")
  .setRun(async (interaction, args, client) => {
    const inp = interaction.fields.getTextInputValue("input");
    await interaction.reply(`Input: ${inp}`);
  });
```

---

### Menus

**Directory:** `src/menus/{type}`

The `type` folder should be one of:

- `string`
- `user`
- `channel`
- `role`

**Example:**

```ts
import { StringSelectFileBuilder } from "../../classes/menuFile";

module.exports = new StringSelectFileBuilder()
  .setCustomId("menu_test")
  .setRun(async (interaction, args, client) => {
    await interaction.reply(interaction.values[0]);
  });
```

---

### Component Arguments

Custom IDs are parsed by splitting on `-`, and the resulting array is passed as `args`.

Example:

```ts
Custom ID: "test_Hi@Lol-1-arg2-whatsup"
Args: ['1', 'arg2', 'whatsup']
```

> All component builders extend the [`baseBuilder`](https://github.com/Fayaz-246/void-base/blob/main/src/lib/baseBuilder.ts) class.

---

### Events

Events go in `src/events/{eventName}/`.

- Each file in the folder is executed when the event is triggered.
- Files must export a function with signature: `(client, ...args)`
- Subfolders **are not supported** inside event folders.

---

### Database (DB)

Void Base uses SQLite for simplicity and speed.

- Define tables in `src/lib/db.ts`
- Access the DB via `client.db` inside your command files.
