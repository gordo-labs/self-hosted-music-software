# The Self-Hosted Music Software Landscape

I built this directory out of need, and out of a curiosity I could not drop. I wanted the files I already own to feel like **one system** — desk, phone, commute, crates — without the week turning into a homelab exam. The first user was me. The product question came next: why isn’t there a **horizontal, stable** way to keep a coherent catalog across every part of that life, simple enough that listening stays **fun** instead of becoming a technical challenge?

The tools below are not the failure. They are excellent specialists. Open and closed sit on the same map. The hole is cohesion: five truths for the same music, unpaid glue, a stack that works on a good LAN day and frays as soon as you leave the house.

That is why this research exists. And that is why I built **[Tape Music Suite](https://tapemusicsuite.com)** — v1 now, and the versions after it. An opinionated horizontal stack for local music. Keep rekordbox on stage and foobar2000 at the desk if those tools still win their job.

This repo is the open catalog. [PRs](CONTRIBUTING.md) from people who maintain software in this stack are welcome. Merges are reviewed; the list does not auto-publish to the Tape site.

Essay companion: [The State of Self-Hosted Music Listening](https://tapemusicsuite.com/blog/state-of-self-hosted-music-listening).

## Jump to

- [Library management](#library-management)
- [Desktop playing](#desktop-playing)
- [Serving / backends](#serving--backends)
- [Mobile clients](#mobile-clients)
- [Networking / away-from-home](#networking--away-from-home)
- [Discovery](#discovery)
- [DJ / performance](#dj--performance)
- [Playlists / export](#playlists--export)
- [Multi-room / streamers](#multi-room--streamers)
- [Hybrid stacks](#hybrid-stacks)
- [Related reading](#related-reading)
- [Contributing](CONTRIBUTING.md)

---

## Library management

Here’s how I see this layer: make a local archive searchable, identifiable, and trustworthy without destroying folder truth — tags in files, release identity, inbox/staging, duplicate judgment. Deeper essay: [library management](https://tapemusicsuite.com/blog/local-music-library-management).

### Intake & acquisition

Arrival tools. They feed Downloads → Preview → archive; they are not the catalog authority. Tape’s future notes treat this as an optional plugin over external daemons, not core playback.

- **[Lidarr](https://lidarr.audio/)** — Strength: wanted-artist / quality-profile manager in the *arr family — “I want this release” as state, not a one-off search. Weakness: indexer/download-client gravity; the wanted DB is yet another authority beside the files you actually play.
- **[slskd](https://github.com/slskd/slskd)** — Strength: Soulseek as a daemon API — the transfer engine collectors already drop into an intake folder. Weakness: P2P seeking is not library hygiene; path chaos lands in Inbox unless something else stages it.
- **[Soularr](https://github.com/mrusse/soularr)** — Strength: bridge that reads Lidarr wanted state and acquires through slskd — the orchestrator our plugin research named instead of reimplementing Soulseek inside a hub. Weakness: three moving parts (Lidarr + slskd + Soularr) before a file is even tagged.
- **[Soulseek](https://www.slsknet.org/) / [Nicotine+](https://nicotine-plus.org/)** — Strength: the human seeking UI that still finds rips and promos storefronts never carried. Weakness: desktop client as intake, not as identity; folders arrive messy on purpose.
- **[yt-dlp](https://github.com/yt-dlp/yt-dlp)** — Strength: CLI extraction from web sources when the “download” is a YouTube/SoundCloud idea, not a storefront purchase. Weakness: metadata is whatever the page had; this is intake, not enrichment.

### Enrichment & identity

- **[MusicBrainz Picard](https://picard.musicbrainz.org/)** — Strength: release-aware tagging against [MusicBrainz](https://musicbrainz.org/) / [AcoustID](https://acoustid.org/) that writes identity into the files so it survives a player reinstall. Weakness: matching still costs attention, and same-title different masters punish overconfident auto-accept.
- **[beets](https://beets.io/)** — Strength: scriptable CLI import, rename, and metadata pipeline for people who want the library as versionable data. Weakness: it can become yet another authority beside a player DB, and the learning curve is real before the pipeline feels safe.
- **[SongKong](https://jthink.net/songkong/) / [Yate](https://2manyrobots.com/yate/) / [Jaikoz](https://www.jthink.net/jaikoz/)** — Strength: automated tag repair that buys time back on mass enrichment. Weakness: destructive confidence against ambiguous releases.

### Batch file editors

- **[Mp3tag](https://www.mp3tag.de/en/)** — Strength: durable batch tag editor (Windows + Mac) that writes portable edits to disk — the craft surface when DJ apps invited tagging and nothing round-tripped. Weakness: it won’t invent a listening or phone workflow for you; it’s hygiene, not a suite.
- **[Kid3](https://kid3.kde.org/) / [TagScanner](https://www.xdlab.ru/en/index.htm)** — Strength: cross-platform / niche batch tagging on the same plane as Mp3tag-class. Weakness: fragmented naming and fewer shared workflows.

### Desk / OS library homes

- **[foobar2000](https://www.foobar2000.org/) (as manager)** — Strength: columns and components as organize+listen UI without installing a separate library app. Weakness: deepest state stays in the foobar world; phone continuity is still a separate problem.
- **[MusicBee](https://www.getmusicbee.com/)** — Strength: whole-Windows-home for daily library, playlists, and playback — an emotional center that may never need a server. Weakness: Windows gravity and dual-library drift if the handset diverges from the desk home.
- **[MediaMonkey](https://www.mediamonkey.com/)** — Strength: Windows library manager/player gravity with careful enrichment rituals. Weakness: auto-organize narratives can scare people who already fear destructive cleanup.
- **[Swinsian](https://swinsian.com/)** — Strength: Mac-native library with playlist/XML-bridge habits that still feed export stories. Weakness: bridge labor remains; reliability and scale pain show up the same places iTunes-era lists did.
- **[Apple Music](https://www.apple.com/apple-music/) / [iTunes](https://www.apple.com/itunes/)** — Strength: historic smart-list authority and XML export source that still shapes how people think playlists should work. Weakness: reliability frays at scale; streaming volatility and Date Added / re-import pain compound the handoff.

### DJ hygiene & accidental managers

- **[Lexicon](https://www.lexicondj.com/)** — Strength: commercial DJ library manager put _beside_ performance apps so the stage DB is not the only hygiene home. Weakness: another paid seat in a fragmented toolchain; sync into DJ apps can still drop crates if paths fight.
- **Beatport Pro–shaped workflows** — Strength: enrichment intake from the [Beatport](https://www.beatport.com/) storefront world taught “complete” metadata expectations that make promo libraries feel finished. Weakness: personal rips never matched that shape; legacy desktop Pro appears discontinued — Pro URL TBD (no invented `pro.` page).
- **[rekordbox](https://rekordbox.com/en/) / [Serato](https://serato.com/dj) / [Traktor](https://www.native-instruments.com/products/traktor-pro) DBs** — Strength: performance databases that become de-facto catalogs — cues, grids, and show confidence where it matters. Weakness: tags and prep often stay app-only; they may not round-trip to files, and migration or path moves hurt.

### Server indexes as authority

- **[Navidrome](https://www.navidrome.org/) / [Jellyfin](https://jellyfin.org/) / [Plex](https://www.plex.tv/) indexes** — Strength: scan-time catalog truth that clients can browse without another desk app. Weakness: a rescan “fixes” one truth and can break playlist edge cases another client depended on — now you have yet another authority.

### Rituals (non-app)

- **Intake folders (Downloads → Preview → archive)** — Strength: reversible staging before main-root commit; real management even when no library app is open. Weakness: discipline is unpaid labor; skip it and the main root inherits chaos.

---

## Desktop playing

Here’s how I see this layer: craft playback on a serious desk machine — formats, gapless, DSP, dense listen UI — not merely staging for a gig. Deeper essay: [desktop playing](https://tapemusicsuite.com/blog/local-music-desktop-playing).

- **[foobar2000](https://www.foobar2000.org/)** — Strength: high-craft local playback and a component ecosystem that still defines serious desk listening for a durable cohort. Weakness: deepest state stays on the machine; “foobar on mobile?” is continuity demand the player itself never solved.
- **[MusicBee](https://www.getmusicbee.com/)** — Strength: integrated Windows listen + organize so one app can be the whole home. Weakness: leaving Windows (or adding a phone path) reopens dual-library risk.
- **[Plexamp](https://www.plex.tv/plexamp/) (desktop path)** — Strength: polished playback surface for [Plex](https://www.plex.tv/) music — closed polish reference without pretending to replace foobar craft. Weakness: you’re inside the Plex loop; management and file hygiene may still need another tool.
- **[Roon](https://roon.app/)** — Strength: high-polish commercial listening suite over local/NAS libraries — a closed reference bar for curated desk listening. Weakness: cost and subscription objections are fair; it’s not cheap hygiene, and continuity outside Roon is still your problem.
- **[Feishin](https://github.com/jeffvli/feishin)** — Strength: desk/browser UI against OpenSubsonic-family servers; common in the Navidrome + Symfonium pride stack. Weakness: it’s a client, not a library manager — tagging and backups stay elsewhere.
- **OS stock players** — Strength: zero-friction default glass that plays files today. Weakness: baseline serious users outgrow; no library craft, no portable identity story.
- **[Audirvana](https://audirvana.com/) / [VOX](https://vox.rocks/)** — Strength: audiophile / high-res desk listening for format and DSP loyalty. Weakness: niche surface area; continuity outside the desk player is still your problem.
- **[Strawberry](https://www.strawberrymusicplayer.org/) / [Quod Libet](https://quodlibet.readthedocs.io/) / [Clementine](https://www.clementine-player.org/) / [DeaDBeeF](https://deadbeef.sourceforge.io/)** — Strength: Linux/open desk players on the same plane outside Windows-centric stacks. Weakness: thinner shared “how we run this with a phone” muscle memory.

---

## Serving / backends

Here’s how I see this layer: index owned files, expose catalog + stream API, stay up — a finished backend vertical beside desk and phone layers. Deeper essay: [music library servers](https://tapemusicsuite.com/blog/local-music-library-servers).

- **[Navidrome](https://www.navidrome.org/)** — Strength: lean Subsonic/OpenSubsonic music index + stream with a small footprint and replaceable clients — the DIY-Spotify convergence point I see most often. Weakness: “server up” is not organization, discovery polish, or DJ export; ops and tagging still sit on you.
- **[Jellyfin](https://jellyfin.org/) (music)** — Strength: music as a module on a broader self-hosted media appliance you may already run for video. Weakness: client quality and metadata awkwardness vary; music UX inherits whatever companion you pick.
- **[Ampache](https://ampache.org/)** — Strength: long-running web music server with API clients and Subsonic-family muscle memory. Weakness: older UX gravity; not always the first pick when people want a lean modern footprint.
- **[Subsonic](https://www.subsonic.org/)-family / forks** — Strength: protocol-era servers that shaped the whole client ecosystem; older deployments still work. Weakness: lineage and licensing history confuse newcomers; forks and successors scatter the “which one” answer.
- **[OpenSubsonic](https://opensubsonic.netlify.app/)** — Strength: spec / lingua franca that makes replaceable clients possible — a technical shared language, not a moral category. Weakness: a spec is not a product; someone still has to run a server and a client that both speak it honestly.
- **[Plex](https://www.plex.tv/) (music)** — Strength: index and stream owned music inside an ecosystem that usually pairs with Plexamp polish. Weakness: closed loop and cost/identity tax; management may still need a side tool.
- **[gonic](https://github.com/sentriz/gonic)** — Strength: minimal Subsonic-compatible music server for people who want even less than Navidrome’s surface. Weakness: thinner docs and client assumptions.
- **[Polaris](https://github.com/agersant/polaris)** — Strength: lean self-hosted music streamer (Rust) with its own API and first-party Android client; cited when people want a small server without the Subsonic-family gravity. Weakness: thinner shared “how we run this with Symfonium” muscle memory than Navidrome.
- **[Music Assistant](https://www.music-assistant.io/)** — Strength: Home Assistant–adjacent music engine that can fan a library out to many player types (including [Sendspin](https://www.sendspin-audio.com/)). Weakness: abstraction over local *and* cloud sources — easy to reintroduce a second catalog while chasing whole-home sync.
- **[Lyrion](https://lyrion.org/) (LMS)** — Strength: legacy Squeeze network-audio lineage still honest in some collector homes. Weakness: niche continuity; not the default DIY-Spotify path most people mean today.
- **[Emby](https://emby.media/)** — Strength: commercial media-server peer on the same plane as Plex/Jellyfin when music mode appears. Weakness: music is rarely the reason people choose it.
- **[Docker](https://www.docker.com/)** — Strength: the ops surface that makes mounts, backups, and rescans repeatable. Weakness: not a music product — listening trust now includes containers, volumes, and recovery fear.

---

## Mobile clients

Here’s how I see this layer: use the owned archive away from the desk — browse, search, play, offline/cache, car — without rebuilding in a storefront. Deeper essays: [phone map](https://tapemusicsuite.com/blog/local-music-library-on-phone-map) · [phone clients](https://tapemusicsuite.com/blog/local-music-library-on-phone-clients).

### Android & closed polish

- **[Symfonium](https://symfonium.app/)** — Strength: Android API client high bar — profiles, smart playlists, offline cache against servers; the daily-driver citation in DIY-Spotify setups. Weakness: paid client value objections; export back to DJ apps and desk authorities still fragile.
- **[Plexamp](https://www.plex.tv/plexamp/)** — Strength: closed polish ceiling for phone listening inside Plex — pre-cache, sonic features, finished-feeling UI. Weakness: you’re buying the Plex loop; outside that ecosystem the bar is a reference, not a portable library manager.

### iOS (OpenSubsonic / Jellyfin-shaped)

- **[Amperfy](https://github.com/BLeeEZ/amperfy)** — Strength: iOS client against Subsonic-family / Jellyfin-shaped backends (GitHub is project home). Weakness: iOS field churn — features and polish vary; you’re still dependent on server honesty and cache freshness.
- **[Shelv](https://vkugler.app/)** — Strength: iOS owned-library streaming client; class holds if renamed. Weakness: smaller surface than the Android high bar; continuity across desk ratings/playlists is not automatic.
- **[Arpeggi](https://apps.apple.com/app/arpeggi/id6503619183)** — Strength: iOS OpenSubsonic/Jellyfin-field client with App Store as distribution home. Weakness: App Store–only footprint; expect the same server/cache seams as peers.
- **[Narjo](https://www.narjomusic.com/)** — Strength: another iOS client in the owned-library field. Weakness: inventory peer, not a guarantee of Symfonium-class depth — verify against your backend before committing.
- **[Nautiline](https://nautiline.app/)** — Strength: iOS client for people already on Subsonic/Jellyfin-shaped stacks. Weakness: same field limits — phone is projection, not the place tags get fixed.
- **[NaviBeat](https://navibeat.app/)** — Strength: newer iOS / Apple multi-device peer in the Subsonic/Jellyfin field. Weakness: newer means less shared muscle memory; treat as peer, not settled default.

### Caching, on-device, DAP

- **Sonamp-class caching** — Strength: aggressive start-fast / pre-cache pattern collectors praise over cellular. Weakness: product homepage **TBD** — treat as pattern until Comms confirms a software product (not Sonance hardware).
- **Local-files / on-device players** — Strength: phone-as-DAP without a hub narrative; files in the pocket, honest and simple. Weakness: second-library risk if the pocket copy is not a projection of the hub (TBD / n/a).
- **Android / portable DAP players** — Strength: dedicated portable playback of owned files for travel subsets. Weakness: subset vs NAS canonical — which truth wins on write? (URL TBD)
- **[Poweramp](https://www.powerampapp.com/) / [USB Audio Player Pro](https://www.extreamsd.com/)** — Strength: on-device Android library playback with EQ/format craft. Weakness: desk and server identities stay elsewhere.

### DJ-cloud mobile prep (alternate plane)

- **[rekordbox](https://rekordbox.com/en/) Cloud** — Strength: prep on phone via DJ-ecosystem cloud when stage workflows already live there. Weakness: trade is a second catalog tax — name it, don’t pretend it’s the default owned-archive thesis.
- **[MIXO](https://www.mixo.dj/)** — Strength: cross-DJ / cloud prep bridging DJ apps and mobile — demand validation that phone prep matters. Weakness: second-catalog tax and paid fragmentation again.

### Ecosystem & classic Subsonic clients

- **[Finamp](https://github.com/finamp-app/finamp)** — Strength: popular Jellyfin music phone surface when the household already standardized on Jellyfin. Weakness: UX inherits companion quality; you’re still projecting a server catalog.
- **[Tempo](https://github.com/CappielloAntonio/tempo) / [DSub](https://github.com/daneren2005/Subsonic) / [Ultrasonic](https://gitlab.com/ultrasonic/ultrasonic) / play:Sub** — Strength: earlier-generation Subsonic phone clients with historical density and muscle memory. Weakness: polish and offline expectations often lag the current Android high bar. play:Sub lives on the [Subsonic apps list](https://www.subsonic.org/pages/apps.jsp); no separate product homepage.

---

## Networking / away-from-home

Here’s how I see this layer: reach the same hub catalog off-LAN without copying the library into a cloud music service. Good networking tools; wrong tax when the promise was a music suite. Deeper essay: [remote access](https://tapemusicsuite.com/blog/local-music-library-remote-access). Architecture proof (not this directory’s hero): [Iroh inside Tape](https://tapemusicsuite.com/blog/iroh-device-connection-without-tailscale).

- **LAN-only** — Strength: correct default for owned libraries; no remote tax, disks feel local. Weakness: leave the house and the lifestyle pauses unless you add another layer.
- **Port forward + reverse proxy + DDNS ([Caddy](https://caddyserver.com/) / [Traefik](https://traefik.io/) / [nginx](https://nginx.org/))** — Strength: classic remote exposure of a media server when you own the ops skill. Weakness: open-internet risk and an ops skill gate before playback.
- **[Tailscale](https://tailscale.com/)** — Strength: the overlay I see named most often next to Navidrome/Jellyfin/Plex stacks — zero-config mesh so the phone treats home services as local. Weakness: sidecar app + identity on every device; wrong tax when the promise was a music suite.
- **[Headscale](https://headscale.net/)** — Strength: self-hosted Tailscale control plane for people who want the same overlay without Tailscale’s coordination server. Weakness: you now operate the control plane too.
- **[Netbird](https://netbird.io/) / [ZeroTier](https://www.zerotier.com/)** — Strength: other mesh/overlay products people actually install to reach a home music server. Weakness: same sidecar gravity — excellent homelab tools, still not a listening product.
- **[WireGuard](https://www.wireguard.com/)** — Strength: lean VPN tunnels into the homelab; named constantly beside commute clients, often underneath Tailscale. Weakness: you’re a part-time network operator; cellular + VPN + cache fail together.
- **[Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/)** — Strength: outbound tunnel without inbound ports. Weakness: third-party path and config surface between you and the catalog.
- **[ngrok](https://ngrok.com/)** — Strength: quick temporary tunnels for testing a server from outside. Weakness: situational, not a commute lifestyle.
- **Tape Private Network / [Iroh](https://www.iroh.computer/)** — Strength: embedded P2P pairing aimed at own devices inside the music suite — stance/proof for device connection without making mesh the product. Weakness: not a substitute for tagging, playlists, or server ops; architecture proof, not series hero.
- **[Dropbox](https://www.dropbox.com/) / [Syncthing](https://syncthing.net/) sync-folder second copy** — Strength: mobile strategy via a synced tree when VPN feels like the product. Weakness: works until it encodes a second truth.

---

## Discovery

Here’s how I see this layer: rediscover forgotten owned tracks; radio-like paths from _your_ catalog; smart lists that respect local identity — not [Spotify](https://www.spotify.com/) Radio with different branding. Not stage software. Deeper essay: [discovery](https://tapemusicsuite.com/blog/local-music-library-discovery).

- **[Plexamp](https://www.plex.tv/plexamp/) sonic / radio** — Strength: sonic analysis and radio-like paths inside owned Plex music; often leads local “mix” polish conversations. Weakness: closed loop; best discovery UI may not be where tags get fixed.
- **[Symfonium](https://symfonium.app/) smart playlists** — Strength: rule-based playlists on the phone client; high bar when tags are good enough. Weakness: weak tags yield weak lists; rules that live only on the phone don’t export cleanly.
- **Server random / similar ([Navidrome](https://www.navidrome.org/) / [Jellyfin](https://jellyfin.org/)-class)** — Strength: built-in random/similar endpoints that cost nothing extra once the server is up. Weakness: often thinner than closed sonic loops — useful, not magical.
- **Desk dynamic playlists ([foobar2000](https://www.foobar2000.org/) / [MusicBee](https://www.getmusicbee.com/) / [iTunes](https://www.apple.com/itunes/)-class)** — Strength: dynamic lists where the desk authority owns the rules. Weakness: best discovery UI may not be where enrichment happens or where the phone listens.
- **Manual crate rituals / Preview folders** — Strength: human crate digging and staging as rediscovery — underrated and reversible. Weakness: unpaid discipline; doesn’t scale without tagging hygiene (n/a ritual).
- **[MusicBrainz](https://musicbrainz.org/) / [Discogs](https://www.discogs.com/)-assisted browsing** — Strength: release identity / provenance that makes search trustworthy. Weakness: not a replacement pitch for either service as a listening product — identity fuel, not a radio engine.
- **[Last.fm](https://www.last.fm/) / [ListenBrainz](https://listenbrainz.org/) scrobble habits** — Strength: optional play-history aided rediscovery when you already scrobble; ListenBrainz is the open side-channel for the same job. Weakness: don’t invent centrality; history is a side channel, not the library.
- **[Bandcamp](https://bandcamp.com/)** — Strength: intake and collection culture that feeds the archive you actually own. Weakness: purchase culture more than a “discovery engine” claim inside the local library.

---

## DJ / performance

Here’s how I see this layer: stage prep and show software — cues, grids, hardware, the night’s confidence. A different job from rediscovering your own catalog, and a different job from carrying playlists between apps. Deeper essay (handoff, not a vs-table): [playlists, export, and DJ handoff](https://tapemusicsuite.com/blog/local-music-playlists-export-dj).

- **[rekordbox](https://rekordbox.com/en/)** — Strength: performance preparation and show workflows — stage strengths deserve respect. Weakness: cloud prep is a separate plane; personal-library continuity and path moves still hurt outside the show.
- **[Serato](https://serato.com/dj)** — Strength: crate-centric DJ performance library with hardware and show confidence. Weakness: crates/paths/portability threads — manager sync and folder moves can orphan work.
- **[Traktor](https://www.native-instruments.com/products/traktor-pro)** — Strength: DJ collection and performance surface with smart-list / migration narratives in real workflows. Weakness: migration hell and collection handoff remain unpaid when the desk library isn’t Traktor.
- **[Engine DJ](https://enginedj.com/) / [VirtualDJ](https://www.virtualdj.com/) / [djay](https://www.algoriddim.com/)** — Strength: alternate performance surfaces in some workflows. Weakness: more destinations mean more export seams — still no vs-table.
- **[Mixxx](https://mixxx.org/)** — Strength: open performance deck with serious local analysis (BPM/key lineage shared with KeyFinder). Weakness: still a stage/listen app; crates and tags may not be the hub catalog.

### Analysis & key (prep, not the show)

Specialists our DJ-analysis roadmap named instead of pretending Rekordbox built-in is enough.

- **[Mixed In Key](https://mixedinkey.com/)** — Strength: paid harmonic-mixing reference bar — batch key/energy into tags DJs actually trust. Weakness: another app before the stage DB; not a library manager.
- **[KeyFinder](https://ibrahimshaath.co.uk/keyfinder/)** — Strength: local OSS batch key estimation that writes files; historical “folder in, tags out.” Weakness: dated desktop; Mixxx now carries the algorithm for many people.
- **[Lowkey](https://lattebits.com/lowkey)** — Strength: newer local batch key/BPM → ID3 so Rekordbox/Serato see it without a cloud upload. Weakness: small shared muscle memory; still a sidecar before the hub.

---

## Playlists / export

Here’s how I see this layer: build ordered listening as labor; carry that work to phone and/or stage apps without forking the catalog. Performance destinations live in [DJ / performance](#dj--performance). Deeper essay: [playlists, export, and DJ handoff](https://tapemusicsuite.com/blog/local-music-playlists-export-dj).

### Smart lists, bridges, formats

- **[Apple Music](https://www.apple.com/apple-music/) / [iTunes](https://www.apple.com/itunes/) smart playlists** — Strength: historic rule engines that still feed XML export stories. Weakness: reliability pain at scale; Date Added and identity loss on re-import.
- **[MIXO](https://www.mixo.dj/)** — Strength: move playlists/libraries across DJ ecosystems — demand validation that bridges matter. Weakness: second-catalog tax and paid fragmentation.
- **[Lexicon](https://www.lexicondj.com/) export/sync** — Strength: manager ↔ DJ bridge to keep hygiene and performance in sync without living only in the stage DB. Weakness: sync breaks still cost crates when paths fight; another paid seat.
- **M3U / XSPF paths** — Strength: lowest-common-denominator playlist interchange almost everything can read. Weakness: break when mount points / NAS roots change (n/a format).
- **XML / crate exchange utilities** — Strength: one-off bridges between library apps when nothing else speaks. Weakness: absurd pipelines = unpaid engineering (URL TBD).

### Mobile playlist labor

- **[Symfonium](https://symfonium.app/) / [Plexamp](https://www.plex.tv/plexamp/) playlists + offline** — Strength: sequence on phone for life; easy to cache and actually use on the commute. Weakness: fragile round-trip into DJ apps — export remains the seam.

---

## Multi-room / streamers

Here’s how I see this layer: whole-home playback and dedicated streamer boxes — rooms in sync, a Pi as a DAC endpoint — not the commute phone path and not library hygiene. Adjacent to serving; not a substitute for a hub catalog. No dedicated essay in this series (lateral).

- **[Sendspin](https://www.sendspin-audio.com/)** — Strength: open whole-home sync (Open Home Foundation) aimed at audio + artwork across rooms, often via [Music Assistant](https://www.music-assistant.io/). Weakness: a multi-room protocol is not desk→phone continuity; HA gravity can become the product.
- **[moOde Audio](https://moodeaudio.org/)** — Strength: Raspberry Pi streamer OS with a serious web UI for local and renderer playback. Weakness: appliance listening in the room; the phone and the tag pipeline still live elsewhere.
- **[piCorePlayer](https://www.picoreplayer.org/)** — Strength: RAM-boot Squeeze/Lyrion player (and optional LMS host) that people trust through power pulls. Weakness: Lyrion-lineage world; not the default Navidrome + Symfonium commute stack.
- **[Sonos](https://www.sonos.com/) / Chromecast / UPnP ([Rygel](https://gnome.pages.gitlab.gnome.org/rygel/))** — Strength: room endpoints people already use when the phone is a controller, not the speaker. Weakness: renderer identity is not catalog identity; groups and caches drift from the hub.

---

## Hybrid stacks

Here’s how I see this layer: specialists get you most of the way; living across them still asks for unpaid integration. These constellations are not failed attempts to be a suite — they are proof. Open and closed coexist. Assemble-stack essay: [assembling a local music stack](https://tapemusicsuite.com/blog/local-music-library-assemble-stack).

- **[Navidrome](https://www.navidrome.org/) + [Symfonium](https://symfonium.app/) + [Feishin](https://github.com/jeffvli/feishin)** — Strength: open-leaning pride stack — lean server + Android daily driver + desk/web client for DIY owned-library listening without a storefront. Weakness residual: tagging, backups, remote policy, DJ export.
- **[Plex](https://www.plex.tv/) + [Plexamp](https://www.plex.tv/plexamp/)** — Strength: closed polish reference loop (pre-cache, sonic, phone UX). Weakness: management may still need another tool; you’re inside one ecosystem’s gravity.
- **[Navidrome](https://www.navidrome.org/) ∥ [Plex](https://www.plex.tv/) (parallel)** — Strength: two backends on purpose — lean music index + broader media appliance, each loved for a job. Weakness: two authorities; lifetime math and which-truth-wins arguments never fully leave.
- **[Jellyfin](https://jellyfin.org/)-first + phone client** — Strength: music as a module on an existing video appliance. Weakness: UX inherits client quality; music is rarely the appliance’s first love.
- **[foobar2000](https://www.foobar2000.org/) + USB / synced phone folder** — Strength: anti-server honesty; listening craft supreme. Weakness: continuity is manual; second-library risk when the pocket copy drifts.
- **[MusicBee](https://www.getmusicbee.com/)-as-whole-Windows-home** — Strength: quiet variant — one Windows app as emotional center ± occasional phone sync. Weakness: dual-library drift when the handset diverges.
- **[Lexicon](https://www.lexicondj.com/) + [rekordbox](https://rekordbox.com/en/)/[Serato](https://serato.com/dj) ± [MIXO](https://www.mixo.dj/)/rekordbox Cloud** — Strength: hygiene beside performance; protect stage tools from being the only DB. Weakness: paid fragmentation and sync/crate seams still show up on bad weeks.
- **[iTunes](https://www.apple.com/itunes/)/[Apple Music](https://www.apple.com/apple-music/) smart lists → DJ XML** — Strength: historic smart-list labor still feeding performance apps. Weakness: half-working bridges that ship weekends — reliability and identity loss at scale.
- **[Dropbox](https://www.dropbox.com/) / [Syncthing](https://syncthing.net/) synced music folder** — Strength: sync-as-mobile-strategy when VPN feels like the product. Weakness: encodes second truth the moment both sides write.
- **NAS canonical + travel DAP subset** — Strength: subset in pocket; NAS remains home authority. Weakness: dual library — which truth wins on write? (n/a / TBD)
- **Desk + server + [WireGuard](https://www.wireguard.com/) / [Tailscale](https://tailscale.com/) / [Headscale](https://headscale.net/) + phone client** — Strength: full franken lifestyle that can feel complete on a good LAN day. Weakness: completeness theater until one seam fails — cellular + VPN + cache.
- **[beets](https://beets.io/) + [Navidrome](https://www.navidrome.org/) + [piCorePlayer](https://www.picoreplayer.org/) + [Symfonium](https://symfonium.app/)** — Strength: file pipeline + lean index + room streamer + commute client. Weakness: three listen surfaces; ratings and playlists still have to pick an authority.
- **[Music Assistant](https://www.music-assistant.io/) + [Sendspin](https://www.sendspin-audio.com/) + [Home Assistant](https://www.home-assistant.io/)** — Strength: whole-home sync when the house is already an HA node. Weakness: smart-home gravity; local files can become one source among storefronts.
- **[Lidarr](https://lidarr.audio/) + [Soularr](https://github.com/mrusse/soularr) + [slskd](https://github.com/slskd/slskd) → intake folder** — Strength: wanted-library automation that drops files where a hub can scan — the acquisition stack our plugin research mapped instead of shipping Soulseek in-process. Weakness: three daemons plus legal/ops posture; Inbox still has to become identity.

“Almost one catalog feeling” is not almost a brand. Keep a quiet frankenstack if crates arrive, caches tell the truth, and remote nights are not incident response.

---

## Related reading

- [The State of Self-Hosted Music Listening](https://tapemusicsuite.com/blog/state-of-self-hosted-music-listening) — flagship state-of-play
- [Local library on the phone — map](https://tapemusicsuite.com/blog/local-music-library-on-phone-map)
- [Library management](https://tapemusicsuite.com/blog/local-music-library-management)
- [Desktop playing](https://tapemusicsuite.com/blog/local-music-desktop-playing)
- [Music library servers](https://tapemusicsuite.com/blog/local-music-library-servers)
- [Phone clients](https://tapemusicsuite.com/blog/local-music-library-on-phone-clients)
- [Remote access](https://tapemusicsuite.com/blog/local-music-library-remote-access)
- [Discovery](https://tapemusicsuite.com/blog/local-music-library-discovery)
- [Playlists, export, and DJ handoff](https://tapemusicsuite.com/blog/local-music-playlists-export-dj)
- [Assembling a local music stack](https://tapemusicsuite.com/blog/local-music-library-assemble-stack)
- [Iroh inside Tape — Personal Private network](https://tapemusicsuite.com/blog/iroh-device-connection-without-tailscale) — networking stance / architecture proof only

---

If you maintain five truths so the same music can follow you from desk to phone to set, that is a cohesion problem — not a failure of any specialist named above.

This catalog is research for that build — not a ranking, not a feature war. I’m not asking you to abandon rekordbox on stage or foobar2000 at the desk if those tools still win their job. Trial on Tape is twenty hours of use or thirty days, then a license — V1 at $100, or Lifetime at $120 early bird ($150 later).

Want something added or corrected? **[Open a pull request](https://github.com/gordo-labs/self-hosted-music-software/compare).** Maintainer of a tool in this stack? Even better. How to file it: [CONTRIBUTING.md](CONTRIBUTING.md).
