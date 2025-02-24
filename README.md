# DiscordTS
Create your discord bot using TypeScript and decorators!  
This module is built on `discord.js`, so the internal behavior (methods, properties, ...) is the same.

## Installation

Use `npm` or `yarn` to install DiscordTS with the peer dependecies (`discord.js` and `@types/discord.js`):
```sh
npm i @typeit/discord discord.js @types/discord.js
```
> You can install `@types/discord.js` as a dev dependency

you must install `reflect-metadata` for the decorators and import it at your entry point
```sh
npm i reflect-metadata
```
```typescript
import "reflect-metadata";

// start ...
```

Your tsconfig.json should looks like that :
```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es2017",
    "noImplicitAny": false,
    "sourceMap": true,
    "outDir": "build",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "declaration": true,
    "importHelpers": true,
    "forceConsistentCasingInFileNames": true,
    "lib": [
      "es2017",
      "esnext.asynciterable"
    ],
    "moduleResolution": "node"
  },
  "exclude": [
    "node_modules",
    "tests",
    "examples"
  ]
}
```

## Getting started
So we start with an empty class (abstract is not necessary but this is more type-safe, the class shouldn't be initialized)
```typescript
abstract class MyDiscordApp {
}
```

Then you must declare it as a Discord app class with the `@Discord` decorator :

```typescript
import { Discord } from "@typeit/discord";

@Discord // Decorate the class
abstract class MyDiscordApp {
}
```

We can now declare methods that will be executed whenever a Discord event is triggered.  
Our methods must be decorated with the `@On(event: string)` or `@Once(event: string)` decorator.  
When the event is triggered, the method is called and we receive values (in arguments) related to the event.
Here, we receive the message instance (details below) :

```typescript
import { Discord, On } from "@typeit/discord";

@Discord
abstract class MyDiscordApp {
  @On("message")
  private onMessage(message: Message) {
    // ...
  }
}
```

## Start your application
In order to start your application, you must use the DiscordTS client (not the client that is provided by discord.ts!).  
It works the same as the discord.js's Client (same methods, properties, ...) but the `login` method is overriden and you can set the `silent` property in order to not log anything in the console.
```typescript
import { Client } from "@typeit/discord";

function start() {
  const client = new Client();
  client.login(
    "YOUR_TOKEN",
    `${__dirname}/*Discord.ts`
  );
}

start();
```

## Client payload injection
You also receive, always as the last payload, the client instance :
```typescript
import {
  Discord,
  On,
  Client
} from "@typeit/discord";

@Discord
abstract class MyDiscordApp {
  @On("message")
  private onMessage(
    message: Message,
    client: Client // Client instance injected here
  ) {
    // ...
  }
}
```

## Events and payload
Here you have the details about the payloads that are injected into the method related to a specific event.
Be aware that the types must be imported from discord.js (except for `Client`).
In this example of the event `"channelUpdate"` we receive two payloads from the event :
```typescript
@Discord
abstract class MyDiscordApp {
  @On("channelUpdate")
  private onChannelUpdate(
    oldChannel: Channel,  // first one
    newChannel: Channel   // second one
  ) {
    // ...
  }
}
```

(Works for `@Once(event: string)` too)

```typescript
on(event: 'channelCreate', listener: (channel: Channel) => void);
on(event: 'channelDelete', listener: (channel: Channel) => void);
on(event: 'channelPinsUpdate', listener: (channel: Channel, time: Date) => void);
on(event: 'channelUpdate', listener: (oldChannel: Channel, newChannel: Channel) => void);
on(event: 'clientUserGuildSettingsUpdate', listener: (clientUserGuildSettings: ClientUserGuildSettings) => void);
on(event: 'clientUserSettingsUpdate', listener: (clientUserSettings: ClientUserSettings) => void);
on(event: 'debug', listener: (info: string) => void);
on(event: 'disconnect', listener: (event: any) => void);
on(event: 'emojiCreate', listener: (emoji: Emoji) => void);
on(event: 'emojiDelete', listener: (emoji: Emoji) => void);
on(event: 'emojiUpdate', listener: (oldEmoji: Emoji, newEmoji: Emoji) => void);
on(event: 'error', listener: (error: Error) => void);
on(event: 'guildBanAdd', listener: (guild: Guild, user: User) => void);
on(event: 'guildBanRemove', listener: (guild: Guild, user: User) => void);
on(event: 'guildCreate', listener: (guild: Guild) => void);
on(event: 'guildDelete', listener: (guild: Guild) => void);
on(event: 'guildMemberAdd', listener: (member: GuildMember) => void);
on(event: 'guildMemberAvailable', listener: (member: GuildMember) => void);
on(event: 'guildMemberRemove', listener: (member: GuildMember) => void);
on(event: 'guildMembersChunk', listener: (members: GuildMember[], guild: Guild) => void);
on(event: 'guildMemberSpeaking', listener: (member: GuildMember, speaking: boolean) => void);
on(event: 'guildMemberUpdate', listener: (oldMember: GuildMember, newMember: GuildMember) => void);
on(event: 'guildUnavailable', listener: (guild: Guild) => void);
on(event: 'guildUpdate', listener: (oldGuild: Guild, newGuild: Guild) => void);
on(event: 'guildIntegrationsUpdate', listener: (guild: Guild) => void);
on(event: 'message', listener: (message: Message) => void);
on(event: 'messageDelete', listener: (message: Message) => void);
on(event: 'messageDeleteBulk', listener: (messages: Collection<Snowflake, Message>) => void);
on(event: 'messageReactionAdd', listener: (messageReaction: MessageReaction, user: User) => void);
on(event: 'messageReactionRemove', listener: (messageReaction: MessageReaction, user: User) => void);
on(event: 'messageReactionRemoveAll', listener: (message: Message) => void);
on(event: 'messageUpdate', listener: (oldMessage: Message, newMessage: Message) => void);
on(event: 'presenceUpdate', listener: (oldMember: GuildMember, newMember: GuildMember) => void);
on(event: 'rateLimit', listener: (rateLimit: RateLimitInfo) => void);
on(event: 'ready', listener: () => void);
on(event: 'reconnecting', listener: () => void);
on(event: 'resume', listener: (replayed: number) => void);
on(event: 'roleCreate', listener: (role: Role) => void);
on(event: 'roleDelete', listener: (role: Role) => void);
on(event: 'roleUpdate', listener: (oldRole: Role, newRole: Role) => void);
on(event: 'typingStart', listener: (channel: Channel, user: User) => void);
on(event: 'typingStop', listener: (channel: Channel, user: User) => void);
on(event: 'userNoteUpdate', listener: (user: UserResolvable, oldNote: string, newNote: string) => void);
on(event: 'userUpdate', listener: (oldUser: User, newUser: User) => void);
on(event: 'voiceStateUpdate', listener: (oldMember: GuildMember, newMember: GuildMember) => void);
on(event: 'warn', listener: (info: string) => void);
on(event: 'webhookUpdate', listener: (channel: TextChannel) => void);
on(event: string, listener: Function);
```

## Examples
An example is provided in the [`/examples` folder](https://github.com/OwenCalvin/DiscordTS/tree/master/examples) !
