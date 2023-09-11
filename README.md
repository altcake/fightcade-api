# fightcade-api
An unofficial TypeScript wrapper for the [Fightcade](https://www.fightcade.com/) Public API.

## Installation

```sh-session
bun add fightcade-api
```

```sh-session
pnpm install fightcade-api
```

```sh-session
npm install fightcade-api
```

```sh-session
yarn add fightcade-api
```

## Example usage

This library supports TS, ESM, and CommonJS.

#### ECMAScript Modules

```ts
import { Fightcade } from 'fightcade-api';
```

You can also import library functions individually:

```ts
import { GetUser } from 'fightcade-api';
```

#### CommonJS

```ts
const Fightcade = require('fightcade-api').default;
```


There are several examples provided in the source's TSDoc.

### GetUser

```ts
async function GetUser(username: string): Promise<User>
```

```ts
import { Fightcade } from 'fightcade-api';

try {
  // Print the amount of ranked matches per game for the user 'biggs'
  const username = 'biggs';
  const user = await Fightcade.GetUser(username);
  for (const gameid in user.gameinfo) {
    if (user.gameinfo[gameid].rank) {
      console.log(`${gameid}: ${user.gameinfo[gameid].num_matches}`);
    }
  }
} catch(e) {
  console.error(e);
}
```

### GetReplay

```ts
async function GetReplay(quarkid: string): Promise<Replay>
```

```ts
import { Fightcade } from 'fightcade-api';

try {
  // Print the date of the replay '1638725293444-1085'
  const quarkid = '1638725293444-1085';
  const replay = await Fightcade.GetReplay(quarkid);
  const date = new Date(replay.date);
  console.log(date.toString());
} catch(e) {
  console.error(e);
}
```

### GetReplays

```ts
async function GetReplays(): Promise<Replay[]>
async function GetReplays(limit: number): Promise<Replay[]>
async function GetReplays(limit: number, offset: number): Promise<Replay[]>
async function GetReplays(limit: number, offset: number, best: boolean): Promise<Replay[]>
async function GetReplays(limit: number, offset: number, best: boolean, since: number): Promise<Replay[]>
async function GetReplays(limit = 15, offset = 0, best = false, since = 0): Promise<Replay[]>
```

```ts
import { Fightcade } from 'fightcade-api';

try {
  // Print the game channel names of the 15 most recent replays.
  const replays = await Fightcade.GetReplays();
  replays.forEach(replay => console.log(replay.channelname));
} catch(e) {
  console.error(e);
}
```

### GetUserReplays

```ts
async function GetUserReplays(username: string): Promise<Replay[]>
async function GetUserReplays(username: string, limit: number): Promise<Replay[]>
async function GetUserReplays(username: string, limit: number, offset: number): Promise<Replay[]>
async function GetUserReplays(username: string, limit: number, offset: number, best: boolean): Promise<Replay[]>
async function GetUserReplays(username: string, limit: number, offset: number, best: boolean, since: number): Promise<Replay[]>
async function GetUserReplays(username: string, limit = 15, offset = 0, best = false, since = 0): Promise<Replay[]>
```

```ts
import { Fightcade } from 'fightcade-api';

try {
  // Print the game channel names of the 15 most recent replays belonging to the user 'biggs'.
  const username = 'biggs';
  const replays = await Fightcade.GetUserReplays(username);
  replays.forEach(replay => console.log(replay.channelname));
} catch(e) {
  console.error(e);
}
```

### GetReplayURL

```ts
function GetReplayURL(replay: Replay): string
```

```ts
import { Fightcade } from 'fightcade-api';

try {
  // Print the replay URLs of the 15 most recent replays belonging to the user 'biggs'.
  const username = 'biggs';
  const user_replays = await Fightcade.GetUserReplays(username);
  user_replays.forEach(replay => console.log(Fightcade.GetReplayURL(replay)));
} catch(e) {
  console.error(e);
}
```

### GetVideoURL

```ts
async function GetVideoURL(replay: string): Promise<string>
async function GetVideoURL(replay: Replay): Promise<string>
async function GetVideoURL(replay: string | Replay): Promise<string>
```

```ts
import { Fightcade } from 'fightcade-api';

try {
  // Print the Replay's FightcadeVids URL.
  const quarkid = '1638725293444-1085';
  const replay = await Fightcade.GetReplay(quarkid);
  const url = await Fightcade.GetVideoURL(replay);
  console.log(url ?? 'Replay not found.');
} catch(e) {
  console.error(e);
}
```

### GetVideoURLs

```ts
async function GetVideoURLs(replays: string[]): Promise<VideoURLs>
async function GetVideoURLs(replays: Replay[]): Promise<VideoURLs>
async function GetVideoURLs(replays: string[] | Replay[]): Promise<VideoURLs>
```

```ts
import { Fightcade } from 'fightcade-api';

try {
  // Print the replay's FightcadeVids URLs if there are any.
  const replays = await Fightcade.GetReplays();
  const urls = await Fightcade.GetVideoURLs(replays);
  for (const quarkid in urls) console.log(urls[quarkid]);
} catch(e) {
  console.error(e);
}
```

### GetRankings

```ts
async function GetRankings(gameid: string): Promise<Player[]>
async function GetRankings(gameid: string, limit: number): Promise<Player[]>
async function GetRankings(gameid: string, limit: number, offset: number): Promise<Player[]>
async function GetRankings(gameid: string, limit: number, offset: number, byElo: boolean): Promise<Player[]>
async function GetRankings(gameid: string, limit: number, offset: number, byElo: boolean, recent: boolean): Promise<Player[]>
async function GetRankings(gameid: string, limit = 15, offset = 0, byElo = true, recent = true): Promise<Player[]>
```

```ts
import { Fightcade } from 'fightcade-api';

try {
  // Print the top 15 recent ranked UMK3 players and their ranks.
  const gameid = 'umk3';
  const rankings = await Fightcade.GetRankings(gameid);
  rankings.forEach(player => {
    if (player.gameinfo && player.gameinfo[gameid].rank) {
      console.log(`${Fightcade.Rank[player.gameinfo[gameid].rank]}: ${player.name}`);
    }
  });
} catch(e) {
  console.log(e);
}
```

### GetGame

```ts
async function GetGame(gameid: string): Promise<Game>
```

```ts
import { Fightcade } from 'fightcade-api';

try {
  // Prints the publisher of the game.
  const gameid = 'umk3';
  const game = await Fightcade.GetGame(gameid);
  console.log(game.publisher);
} catch(e) {
  console.log(e);
}
```

### GetEvents

```ts
async function GetEvents(gameid: string): Promise<Event[]>
async function GetEvents(gameid: string, limit: number, offset: number): Promise<Event[]>
async function GetEvents(gameid: string, limit = 15, offset = 0): Promise<Event[]>
```

```ts
import { Fightcade } from 'fightcade-api';

try {
  // Print the 15 most recent active events for a game.
  const gameid = 'garou';
  const events = await Fightcade.GetEvents(gameid);
  events.forEach(event => console.log(event));
} catch(e) {
  console.log(e);
}
```

## Links

- [Fightcade](https://www.fightcade.com/)
- [Bun](https://bun.sh/)
- [npm](https://www.npmjs.com/package/fightcade-api)
- [NodeJS](https://nodejs.org/en/)
- [TypeScript](https://www.typescriptlang.org/)
- [zod](https://zod.dev/)
- [tsup](https://tsup.egoist.dev/)
- [TSDoc](https://tsdoc.org/)
