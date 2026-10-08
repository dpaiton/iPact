# Sci-fi “hand terminal” app for tabletop storytelling

This project is to make an app that assists a game master (GM) with telling a story in a sci-fi universe. The universe is defined by the Starfinder second edition rule books by Paizo. The app itself will present as a general-purpose hand terminal for space-faring citizens to use for a myriad of purposes. In this discovery document we will use “device” to mean simulated hand-terminal device, and never to mean the underlying phone itself. The app must run on apple and android phones. 

In this initial design, the app has three modes: “corporate”, “hacked”, and “GM”. Corporate and hacked modes are for players. The GM mode has master control over the content visible on the player devices, and also includes features to help with running a game. There will also need to be a backend hosted server to facilitate communication routing, logging, and eventually AI-powered content generation. 

# Ethos

This is an app that is designed to make the phone feel like a futuristic hand terminal. The content is entirely fiction. As such, it will make every effort to obfuscate or hide the actual phone workings behind the app. The app will lie: for example, it could say “end-to-end encrypted” under a DM thread with a mob boss, when that is not at all true. This is ok. The meta is that it is a story telling device; the in-game illusion is that it is a device itself, not an app on their normal phone.

All of it has to feel seamless for the players. They have to feel like they’ve unlocked a new feature or something has gone wrong with the device. To do this, there are certain design decisions that should be decided carefully at the beginning.

The device app is meant to be a creative platform. Part of the immersion will be that the device feels expansive. It feels like it can do anything. We will use tools of storytelling and deception to convey this feeling.

# Creative feature ideas

This section outlines a list of features that are not expected in the POC. They are intended to sign post and indicate the breadth and possible scope of later extensions. The app must be built in a modular way that allows for expanding, modifying, deleting, etc, so that these can be supported.

- An AR experience that distorts the image from the phone’s camera to look like night vision.  
- Give the GM the ability to send an advertisement blast to every player character (PC) terminal at once, or a secret message to just one, or cause all of them to deactivate and cease to work.  
- In-game geofenced zones where comms work but not well, so PCs can send messages but some of the letters get garbled.  
- A “lockpick” minigame that requires a PC to solve a puzzle before they can get an important door open.  
- Push a short audio-video experience to all devices, like a “Welcome to Absalom Station” pre-recorded video for a PC to watch as their transport ship docks.  
- Hacked terminal devices may be able to get more information about PCs and NPCs that are near them by stealing the data from their device. This would be very difficult and time-consuming, and detectable by anti-theft software (available on the corporate app store, but much better versions exist for hacked terminals).

# POC Feature list

The scenarios listed in the previous section are possible, creative features. This list is more technical, and these are a much higher priority. Not all will make it into the POC, but all of them are at least a “nice to have”.

## onboarding

- There will be a disclosure/consent (like a TOS) at onboarding time that explains everything. For example, that all data logs are stored on the GM’s server and everything in the app is fictional, not to be trusted, etc. The user will be encouraged to read this before being required to accept.  
- Otherwise, onboarding should be minimal. Part of the fun is discovery. The users should have to play around to discover.

## engineering

- The app is only accessible on a phone, but will work on both android and apple  
- The app will have a futuristic UI and be responsive. Efficient, simple, upgradable code is essential.  
- The app will use a single codebase for both device types.  
- The app will have access to a cloud hosted server that can serve it content like push notifications. The cloud hosted server is actually an Ubuntu desktop workstation located at the GM’s residence. There will be a server app for that as well.  
- The app will put the phone in “game mode”, reducing the likelihood of interruptions that pull the user out of the experience.  
  - need to investigate: both android & ios have a game mode api, although it may not be accessible without building natively  
- The app uses wifi or phone internet to connect to other devices, however, this is “skinned” differently to fit the theme (see “comms” section below)  
- The player accounts hold a state that can be updated by the player or by the GM. This state will adjust over time, and so the schema needs to support adding and removing fields. Example fields:  
  - Player and PC identity details, like player name, character name, character class, character species or race  
  - PC location details (in-game)  
  - terminal state (low-service, hacked, corrupted, locked, etc)  
  - available features  
  - available maps  
- Each device is managed and individually addressable from the single GM device. And the devices can be unique in their feature set or current state. State modifications (e.g. low signal, blurry screen, etc) can be sent to individual devices.  
- IAM is a core component of the backend. It must be easy for the GM to modify privileges, send data to individuals, and prevent accidental information leak.  
- All messages and activity logs on the devices are recorded in the backend server.  
- Testing should be simple and concentrated. There should not be dozens of edge case tests for each feature. The tests should test the core functionality, assumptions, and end-to-end happy path user flows.

## interface

- It should not look like a conventional app. It should look like a scifi hand terminal interface.  
  - It should be immersive, like an operating system  
  - The interface must be modular to support creative expansion of offerings and ideas

## comms

Much of the nuance with comms requires having a system for distances. The Starfinder 2e ruleset has such a system, but it is unclear how that should be encoded in the hand terminal app. As a POC, we can simply bucket location into zones based on the campaign (e.g. “Absalom station”, “orbit”, “deep space”) and assign a small number of categorical distances (“proximal” – can send information p2p; “close” – must send information over the wire, but comms are immediate; “far” – must send information over the wire and comms have delay).

- When PCs or non-player characters (NPCs) are nearby in-game, they can send direct messages to each other simply with proximity.  
  - It is possible to see who is nearby and has their comms set to visible & available via a map view.  
  - At a distance, devices have to be each other’s contacts to communicate. Devices have a contact ID. You can direct beam your contact card to another device if it is nearby. Otherwise you use the ID to add someone.  
  - Time to communicate scales with distance. It is instantaneous within Absalom station, but it can be minutes to hours if great amounts of space are between them. It is longer than simply the speed of light due to latency added by relays and directional comms.  
  - Peers can connect and send each other messages. The messages can be voice or text or images or videos. The PCs can use their phone cameras to compile & send these, or they can upload images.  
  - Every app instance is connected to a cloud hosted server. When someone DMs someone else, it actually relays through the cloud station. It is not actually peer-to-peer, although it is skinned as such.

## contacts and profiles

This will eventually expand to be a companion app for players to play the game. They can interact with their character sheet to equip items, execute rolls, etc. For now, though, it’s mostly a reference and a way to establish familiarity among in-game device owners.

- Players can see their PC’s profile and the profile of others, including NPCs  
  - NPC profiles are populated by the GM, and only visible if the NPC shares its contact information  
  - PC profiles have an “internal” and an “external” view. The internal view looks like a character sheet, with skills, equipment, and character stats all displayed. The external view looks more like a Pokemon card, with a profile, name, and some blurb or about section.   
  - In the initial version PC and NPC contact details will be uploaded to the backend by the GM, but later versions will support PC character creation from a “meta” or “setup” portal in the app.

## navigation

- maps are available most of the time, but which map depends on the GMs discretion  
  - GMs can push updates to maps in real-time  
  - maps could be static or interactive.  
  - maps could follow a city grid and have features for shop discovery, search, and/or navigation.   
  - maps could behave like a game mini-map with dots indicating PC positions and a birds-eye view of the terrain.

## corporate mode app store

- In corporate mode there is an app store filled with semi-bland corporate apps like a note taking app and an alarm clock.  
  - After a phone is hacked, it no longer can access the app store. All corporate apps are removed.  
  - Players can share apps. PCs will receive apps from other PCs or NPCs. They’re like collectables or power-ups.

## hacking

- the phones are corporate by default, but they can be hacked  
  - hacking is similar to rooting a phone. you uninstall the corporate OS and install a new one.  
  - upsides: The device gets access to illegal apps. For example: lock-picking, terminal hacking, locations for black-market businesses or smuggling tunnels, encrypted messaging to criminals, drone controllers, etc.  
  - downsides: The device triggers corporate scanners and can get you in trouble. It loses access to the app store: the PC must now speak to other PCs and NPCs to get apps. They have to be careful who they talk to.  
  - A PC might be skilled to “develop their own apps”. This would mean they coordinate a feature set with the GM, and then the GM engineers that capability and deploys it to only that PCs device. 

# Build plan

This is an app that will never be sold, never listed on any real app store, and never used by anyone that is not a friend. We can assume all users, including the GM, are of high trust. We can also assume no one is uploading sensitive data to the app. All communications are expected to be "in character". Since the “device” is actually an app and it is expected to be used by friends, there is no need to harden it or worry about security. Bugs are fine, they can be explained away. It does not need to be robust to every possible unhappy path.

This app will not be listed on any app stores. It will be downloaded via a private URL provided to the PCs.

It will be built incrementally, with a minimal end-to-end proof of concept delivered first.

The AI engineer build sequence should be in this order: plan, write schemas, write tests, write the implementation. The AI engineer should get human review at each step.

## Recommended tools

1. **Shared language:** [TypeScript](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html) across the mobile app, console, server, and shared contracts.  
2. **Mobile application:** [React Native](https://reactnative.dev/docs/environment-setup) through the open-source [Expo framework](https://docs.expo.dev/get-started/create-a-project/), with [Expo Router](https://docs.expo.dev/router/introduction/) for navigation and [Expo SecureStore](https://docs.expo.dev/versions/latest/sdk/securestore/) for the persistent device token. Use [Expo development builds](https://docs.expo.dev/develop/development-builds/introduction/) because push notifications require a native build.  
3. **GM console:** [React](https://react.dev/learn) with [Vite](https://vite.dev/guide/). This keeps the console lightweight and usable from any browser on the GM’s network.  
4. **Backend:** [Node.js](https://nodejs.org/learn/getting-started/introduction-to-nodejs) with [Fastify](https://fastify.dev/docs/latest/Guides/Getting-Started/). Fastify’s plugin model fits campaign, messaging, profile, and story-control modules.  
5. **Realtime delivery:** [Socket.IO](https://socket.io/docs/v4/) for device rooms, broadcasts, acknowledgements, reconnection, and React Native support. Persist every important event because its [delivery documentation](https://socket.io/docs/v4/delivery-guarantees/) confirms disconnected clients need application-level replay.  
6. **Persistence and contracts:** [SQLite](https://sqlite.org/about.html) for a simple single-file database, [Drizzle ORM](https://orm.drizzle.team/docs/sqlite/get-started-sqlite) for typed schemas and migrations, and [Zod](https://zod.dev/) for validating shared API and command payloads.  
7. **Terminal UI:** [NativeWind](https://www.nativewind.dev/docs/getting-started/installation) for maintainable styling and [React Native Reanimated](https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/getting-started/) for glitches, low-signal effects, transitions, and blackouts. Pin a stable NativeWind version compatible with the chosen Expo release.  
8. **Notifications and installation:** [Expo Notifications](https://docs.expo.dev/push-notifications/push-notifications-setup/) for background alerts. [EAS Build internal distribution](https://docs.expo.dev/build/internal-distribution/) is the practical managed exception to the open-source preference for installing builds on friends’ phones without app-store publication.  
9. **Testing:** [Vitest](https://vitest.dev/guide/) for server and contract tests, [React Native Testing Library](https://oss.callstack.com/react-native-testing-library/docs/start/quick-start) for mobile components, [Playwright](https://playwright.dev/docs/intro) for the GM console, and [Maestro](https://docs.maestro.dev/get-started/quickstart) for a few complete mobile flows.  
10. **Deployment:** [Docker Compose](https://docs.docker.com/compose/) for repeatable Ubuntu deployment. Use [Tailscale](https://tailscale.com/docs/install/linux) with [Tailscale Serve](https://tailscale.com/docs/reference/tailscale-cli/serve) to privately expose the home server to trusted player devices without opening router ports.

## POC 

### goal

In one two-week sprint, one agent should deliver a GM-controlled experience running on one iPhone and one Android phone. Players enroll with a campaign invite code, select a player/character, and wait for GM approval. The POC proves profiles, contacts, messaging, remote story effects, media delivery, persistence, and background alerts.

### deliverables

- A short onboarding disclosure/consent screen.  
- Three application roles: GM, corporate, and hacked.  
- Simple IAM with account creation for PCs that the GM can discover and assign roles to. Accounts allow for determining which device may see which messages, maps, apps, effects, etc.  
- Server-side authorization only to prevent accidental spoilers or mistaken commands. It will use user/pass login and support biometric authentication. The password is encrypted and not viewable by the GM. Password recovery is done via a GM manual reset to a sharable password, and then the PC changing it themselves.  
- HTTPS/WSS in transit, without end-to-end encryption.  
- GM-readable messages and activity logs.  
- Basic backup and recovery, with no enterprise hardening, abuse prevention, or compliance work.

### first sprint

Gate each step with human review

1. **Plan and scripted scenario**  
   Define the demonstration narrative, acceptance criteria, onboarding disclosure, trust assumptions, and explicit non-goals.  
2. **Architecture and extension contract**  
   Define the mobile client, GM web console, server, durable event log, and feature-module registry. POC modules are bundled with the app and remotely unlocked—not downloaded executable code.  
3. **Schemas and visibility rules**  
   Model campaigns, invite codes, players, characters, devices, device state, PC/NPC profiles, contacts, conversations, messages, feature grants, commands, and audit events. Profile fields receive visibility tiers such as internal, external, contact-only, and hacked-intelligence.  
4. **Tests before implementation**  
   Write focused tests for enrollment, GM approval, profile visibility, NPC contact sharing, exact device targeting, message persistence, reconnect replay, and the primary end-to-end scenario.  
5. **Project and server foundation**  
   Establish shared TypeScript contracts, database migrations, API endpoints, realtime connections, seed data, and modular client registration.  
6. **Enrollment and device identity**  
   Implement invite-code entry, safe character selection, pending approval, persistent device tokens, GM approval/rejection, reassignment, and revocation.  
7. **Mock design and visuals**  
   Build design mocks for the terminal interface  
8. **Player terminal shell**  
   Build the onboarding disclosure, corporate/hacked presentation, terminal navigation, app launcher, connection state, and reconnect behavior.  
9. **Contacts and profiles**  
   Implement:  
   * The PC’s private character-sheet-style internal profile.  
   * External collectible-card-style PC profiles.  
   * GM-authored NPC profiles.  
   * Contact sharing and visibility grants.  
   * GM-triggered hacked-intelligence reveals.  
   * Actual theft gameplay, detection, and anti-theft software remain later features, but the data model supports them.  
10. **GM console**  
    Add campaign setup, player/device approvals, PC and NPC editing, contact grants, profile-intelligence reveals, device status, messaging, and individual/global controls.  
11. **Messaging and story-control engine**  
    Implement server-relayed text conversations, broadcast/private messages, delivery state, reconnect replay, and background alerts. Add commands for corporate/hacked mode, low signal, garbling, delayed delivery, blackout, restore, banners, sending media from the GM to players, and feature unlocks.  
12. **Ubuntu deployment and physical demo**  
    Deploy persistent services, configure remote access and notifications, create a basic backup procedure, and validate the scripted scenario on both phones.

