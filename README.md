# Sharecene project reference

This document is the current spine of Sharecene. It is meant to be uploaded into the project files and updated when the concept changes.

Sharecene is P2P middleware that lets Godot act as a browser of verses. It is not an MMO backend, not a consensus engine, not a court, not guaranteed storage, and not an anonymity network. It is a substrate for hosting, discovering, opening, observing, and connecting Godot verses under explicit technical limits.

A verse is the current name for the authored thing formerly called a world or game. A verse is made of Godot scenes. Sharecene is therefore a multiverse: a shared field of authored verses, and also shared poetry.

The central concept is not merely a custom runtime loading PCK files. Sharecene is based on the Godot editor as a verse-browser shell. The Godot editor already has useful browser-like machinery: it can browse/download assets from the Asset Library and launch a game in a separate window. Sharecene replaces the Asset Library/download substrate with its own P2P/versioned-file substrate, while using/customizing the editor-derived shell to browse, fetch, inspect, and launch verses.

Verses run in a separate Sharecene runtime: Godot with host-dangerous capabilities removed. The browser/editor-derived shell is trusted Sharecene software. The launched verse runtime is the de-implemented Godot runtime.

Verse authors use the normal Godot editor. A Sharecene addon provides development tools, validation, packaging, and PCK publishing support.

## 1. Core shape

Sharecene has three large pieces:

1. A Godot editor-derived verse browser shell.
2. A de-implemented Godot runtime for launching verses.
3. A P2P substrate for hosting, herd discovery, logs, ambient traces, and direct channels.

The browser shell fetches and manages verses.

The runtime executes verses.

The substrate moves files, knowledge, traces, and live peer observations.

## 2. The five verse-builder mechanisms

Sharecene exposes exactly five mechanisms to verse builders:

1. Versioned Files Hosting
2. Peer Log Stream
3. Ambient History
4. Presence and Status
5. Direct Channels

Cultural Knowledge exists below these mechanisms. It is substrate memory propagated through the herd. It is not a sixth verse-builder mechanism.

Avatar Sheets also do not add a sixth verse-builder mechanism. They are a browser-shell/client feature built from local Markdown documents, Sharecene-safe runtime services, Versioned Files Hosting where needed, and Peer Log Streams for proof of ownership and transfer.

## 3. Non-features

Sharecene does not provide:

```text
global authoritative simulation consensus
conflict resolution
reconciliation
canonical truth
hidden anonymity
content availability guarantees
historical permanence guarantees
central moderation
central telemetry
platform-wide reputation
matchmaking requirement
account economy requirement
platform court
platform-wide forced item semantics
universal item enforcement
retroactive deletion of already-hosted verse copies
```

These are not missing features. They are outside the substrate.

A verse builder may build server-operated multiplayer, matchmaking, account systems, economic persistence, or authoritative game logic above or beside Sharecene. Those systems are not Sharecene itself.

## 4. Godot browser model

Sharecene should be designed as an editor-derived browser of verses, not merely as a stripped runtime.

The Godot editor gives a starting point for:

```text
verse browsing UI
asset-library-like download patterns
package/project handling
launching a game in a separate window
author tooling integration
PCK packaging knowledge
```

Sharecene replaces the asset source with:

```text
Sharecene URLs
versioned file hosting
libtorrent-backed content acquisition
Cultural Knowledge
peer discovery
```

Launching a verse means:

```text
browser shell fetches/verifies verse package
browser shell prepares launch context
browser shell starts de-implemented runtime
runtime loads verse PCK
runtime exposes only Sharecene-safe services
```

This avoids designing Sharecene as a blind PCK loader. It is a browser shell plus a safe verse runtime.

### Browser-shell window UX

Sharecene uses a browser-like host window model.

The title bar is owned by Sharecene and includes:

```text
tab selection button (shows tab list)
previous arrow
next arrow
address bar
minimize
maximize
fullscreen toggle
close Sharecene
```

In fullscreen mode, the title bar may be masked unless hovered.

All remaining window area is dedicated to the currently selected verse view.

### Process boundary for verse execution

Even when a verse appears embedded inside the Sharecene host window, verse execution remains separate-process.

The verse process is launched by trusted Sharecene software.

The browser-shell embedding is a presentation/integration model, not in-process runtime cohabitation.

## 5. Runtime sandbox model

The Sharecene runtime is Godot with host-dangerous capabilities removed.

Removed or denied capabilities include:

```text
arbitrary filesystem access
OS access
environment access
process access
wild networking
unrestricted native extension loading
```

The intended approach is de-implementation, not merely policy wrapping.

The runtime must assume verses can be hostile or malformed.

Validators run in the same de-implemented Godot runtime as the verse itself. They are GDScript, not a separate foreign execution system.

The browser shell remains trusted Sharecene code. The verse runtime is constrained.

## 6. Verse authoring path

Verse authors use the normal Godot editor.

A Sharecene addon provides:

```text
PCK packaging
manifest generation
locator generation
validator packaging and publishing
local test launch
basic validation checks
```

The published verse artifact is a Godot PCK plus Sharecene metadata.

The addon is authoring support. The Sharecene browser shell is user-facing verse browsing support. The Sharecene runtime is the constrained execution support.

## 7. URL model

A Sharecene URL has the following syntax:

    sharecene://verse-name/human-friendly-comment#content-hash

The verse context names the verse.

The human-friendly comment is descriptive text for humans.

The authoritative anchor is the content hash.

Example:

```text
sharecene://verse-name/human-friendly-comment#content-hash
```

Only the content-hash locator is authoritative.

The verse name and human-friendly comment are not trusted proof text. They may be helpful. They may be misleading.

The content hash anchor is authoritative.

Hash verification proves only:

```text
retrieved bytes match locator
```

It does not prove:

```text
human-readable parts truthfully describe retrieved bytes
```

This creates a phishing risk that cannot be solved cryptographically. The UI may display author keys, known author state, pinned author state, and locators clearly. It must not pretend the verse name or human comment is proof.

## 8. Versioned Files Hosting

Versioned Files Hosting is immutable file hosting backed by libtorrent.

It distributes:

```text
verse PCKs
assets
manifests
validator bundles
hosted chunks
```

It is not live verse state.

It is not an MMO server.

It is not guaranteed storage.

A locator identifies content by hash. Identification does not imply availability.

Proof:

```text
content hash H can exist as a valid identifier
no reachable peer may currently possess bytes matching H
therefore H identifies content without guaranteeing retrieval
```

Unavailable content is not a Sharecene failure unless a higher layer explicitly promised custody.

Old chunks not recently shared may be deleted according to user storage preferences.

## 9. Swarm, herd, and address book

Swarm is reserved for libtorrent's torrent-level meaning.

Herd means the totality of currently running Sharecene clients.

A local herd view is the subset currently known and reachable by one client.

The address book is the locally stored set of known Sharecene peer addresses.

Libtorrent discovery is used opportunistically. It reveals peers that can be added to the Sharecene address book.

When Sharecene starts:

```text
client reads local address book
client contacts known peers
reachable peers exchange address-book entries
local herd view expands toward the running herd
```

When a user downloads a Sharecene verse, the package should provide at least the verse author's address.

There is no separate central address-book service in the concept.

## 10. Cultural Knowledge

Cultural Knowledge is shared herd knowledge propagated by recency.

It includes:

```text
address-book data
verse-author-published validating functions
recent shared herd metadata
```

When peers greet each other, they may announce the date or revision marker of their most recent Cultural Knowledge.

If one peer knows newer Cultural Knowledge, it can transmit what the other lacks.

This is propagation by recency.

The date or revision marker is synchronization metadata. It is not a global clock authority.

Cultural Knowledge persists within the user's storage limit. Pruning old peer addresses or stale knowledge is storage hygiene, not semantic expiration.

Different peers knowing different records is not a conflict. It is incomplete overlap.

Cultural Knowledge handling is:

```text
receive
verify when applicable
deduplicate
store while useful
propagate newer-known material
```

## 11. Peer Log Stream

A Peer Log Stream is append-only published knowledge.

It is a special case of file hosting: a potentially ever-growing hosted file.

It is available to every member of the herd, authors and visitors.

One member may initiate several streams.

Each stream is a linear string.

A valid stream is not a branch tree.

Sharecene does not provide fork resolution, canonical branch choice, stream conflict semantics, or reconciliation.

Each entry extends one stream.

Previous entries remain valid.

Each entry is joined to a previous URL and a next URL.

The previous URL lets a reader dig backward from the middle.

The next URL is created with the stream private key.

Readers verify it with the stream public key.

A fake continuation cannot be made by attaching an arbitrary next URL.

The link material is outside the payload.

The next link must not force infinite future hashing:

```text
current content hash can be computed now
next URL can be verified as stream-authorized
no infinite dependency on future hashes is created
```

A valid old entry remains valid history. Seeing it again is duplication, not replay attack.

Rule:

```text
valid entry seen again = duplicate
valid older entry = history
invalid continuation = reject
```

### Stream ownership donation

A Peer Log Stream can be donated by handoff-through-presealed-continuation.

Donation is not a platform court decision.

Donation is not consensus.

Donation is not global authority transfer by Sharecene.

It is a cryptographic and content-addressed stream transition inside one linear string.

The current owner may publish an abdication entry.

That entry is the last entry authored under the old ownership.

The abdication entry publicly announces:

```text
this stream is being donated
old owner abdicates after this point
old-owner continuation after the accepted successor is invalid or rogue evidence
```

As with every Peer Log Stream entry, the abdication entry is joined to a next URL or locator.

That next locator commits by content hash to exact successor bytes.

The successor bytes are therefore not negotiable after the locator exists:

```text
next locator = hash(successor bytes)
different successor bytes = different hash = not this continuation
```

For donation, the old owner privately gives the new owner:

```text
the exact unpublished successor bytes committed by the abdication entry's next locator
the continuation material needed to publish or authorize that successor
```

The new owner must verify before accepting:

```text
hash(successor bytes) matches the abdication entry's next locator
the abdication entry publicly announces old-owner abdication/donation
the successor establishes continuation material controlled by the new owner for the following entry
```

The new owner cannot alter the successor bytes without breaking the content hash.

The new owner can only publish those exact bytes or withhold them.

Once the successor is published and verified, future continuation authority follows the successor's declared or bound continuation material.

Any attempt by the old owner to continue after the published abdication-successor transition is invalid or rogue evidence.

Remaining risks:

```text
old owner may never complete the private handoff
new owner may withhold the successor bytes
successor bytes may become unavailable if nobody preserves them
old owner may act in bad faith before the abdication-successor transition is public
```

These risks are availability and bad-faith risks.

They are not content-rewriting risks.

They do not create Sharecene consensus, canonical truth, reconciliation, guaranteed storage, or a platform court.

### Log-stream objects and ownership

A Peer Log Stream may define a named object whose history is meant to be known across verses.

Example:

```text
The Glass Thorn
forged by verse-author W
issued to user-player key A
transferred to user-player key B
```

In this sense, an object such as a sword can exist universally within Sharecene: not because every verse must honor it, but because the herd can know the object's stream, lineage, and public history.

Ownership is proven by continuation capability.

The current owner is the client/user-player able to produce the next valid stream continuation according to that object's stream rules.

Readers may know the sword and its history without being able to prove ownership.

To everyone except the current owner, the continuation/private material is not available. It is therefore accurate to say:

```text
to non-owners, the key is lost
```

An owner can forget an owned object or drop it into oblivion by failing to preserve or provide the continuation material.

If a reader wants truth, the reader needs proof. The owner can provide proof by producing or revealing verifiable continuation evidence for the object stream.

Limits remain:

```text
verses may ignore the object
verses may interpret the object differently
latest chunks may be unavailable
readers may have stale knowledge
ownership proof requires available/verifiable stream material
Sharecene still does not provide consensus or a court
```

Log chunks disappear like other hosted chunks when no one preserves or requests them.

## 12. Verses, title deeds, ownership, and updates

A verse is the authored Sharecene unit made of Godot scenes.

The term verse replaces the older words world and game when referring to this authored unit.

Sharecene is a multiverse of verses.

The term also carries the sense of shared poetry.

A verse can have a stable Peer Log Stream identity.

This stream functions as the verse's title deed.

A verse title-deed stream may record:

```text
verse identity
current verse package locator
current manifest locator
current validator bundle locator
author metadata
verse policies
recommended version
ownership records
ownership transfers
succession or abdication records
```

The verse package remains immutable content hosted through Versioned Files Hosting.

The title-deed stream is the public lineage that points to current and past immutable packages.

Ownership of a verse means authority over the verse's public lineage and future continuation.

It does not mean control over every already-hosted immutable copy.

The current owner is the user-player/client able to generate the next valid title-deed stream continuation or rotate continuation authority.

Proof:

```text
can produce next valid continuation for the verse title-deed stream
therefore controls current verse lineage authority
```

Verse ownership can therefore express:

```text
this user owns this verse
```

This means:

```text
this user controls the verse's recognized update lineage
```

It does not mean:

```text
this user can delete old hosted packages
this user can force peers to forget old versions
this user can force all readers to accept the newest entry
this user receives platform-wide authority
Sharecene becomes a court
Sharecene becomes consensus
```

Old verse package chunks remain valid and may keep circulating.

Ownership gives update and continuation authority, not retroactive deletion.

### Updating a verse from inside Sharecene

Verse ownership provides the native path for authors to update their verse directly from inside Sharecene.

The trusted Sharecene browser shell may expose an owner-authoring/update flow:

```text
owner opens Sharecene browser shell
owner selects an owned verse
owner creates or selects a new verse package
Sharecene publishes the immutable package through Versioned Files Hosting
Sharecene generates the new package locator
Sharecene creates a new title-deed stream entry
owner signs or authorizes the continuation
herd members can discover the newer lineage state
```

The owner may update:

```text
verse package
manifest
validator bundle
avatar-sheet policy
direct-channel policy
author notes
recommended version
metadata
ownership or succession records
```

The final lineage update must happen through trusted Sharecene software.

A launched verse runtime may provide in-verse authoring tools, editors, or creative interfaces.

However, an arbitrary launched verse runtime must not receive authority to mutate its own public title-deed lineage directly.

The secure path is:

```text
edit or build
package
verify
publish immutable content
sign or authorize title-deed continuation
append lineage
```

This preserves the separation between:

```text
trusted browser shell and authoring addon
de-implemented hostile-capable verse runtime
public verse lineage
```

Sharecene therefore becomes both a browser of verses and the place where verse owners can continue their verses without becoming a platform authority.


## 13. Ambient History

Ambient History is not Peer Log Stream.

It is not persistent publishing.

It is not privacy machinery.

Ambient History is unconscious validation trace production.

Clients emit Ambient History chunks.

Those chunks are randomly cut.

Those chunks overlap.

The random overlapping cuts exist to prevent cheating around predictable seams.

Peers duplicate and bounce Ambient History chunks a bounded number of times.

Peers scan Ambient History chunks with validating functions carried by Cultural Knowledge.

Ambient History propagation is bounded by:

```text
minimum copies
minimum hop count
probabilistic counter decay
```

The counter has a 50% chance to decrement on each bounce.

Therefore a counter value `n` has an expected `2n` bounce opportunities before exhaustion.

This gives bounded expected lifetime without a perfectly predictable boundary.

Goal:

```text
not instant death
not infinite propagation
not predictable seams
```

Implementation detail to watch: mutable relay counters must be relay-envelope data, not immutable validation payload data.

## 14. Validating functions

Validating functions are published by verse authors.

They belong to Cultural Knowledge.

They propagate independently from Ambient History chunks.

Propagation is not execution.

A validator is executed only against Ambient History chunks from the same verse.

A validator from verse W does not inspect chunks from verse X.

A validator runs in the same Sharecene runtime as the verse itself.

In practice, this means GDScript inside Godot with Sharecene's host-dangerous capabilities removed.

Validators are not Sharecene judgment. They are alarms.

A validating function examines Ambient History data and detects impossible data according to the verse author's rules.

Alarm limits:

```text
validator silent -> no detected violation
validator screaming -> detected violation candidate
```

Silence does not prove fairness.

An alarm does not automatically convict.

An alarm creates something to inspect, flag, or act on.

The verse author publishes the verse. The verse author publishes the validators. The validators run only for that verse. Therefore validator quality belongs to the same responsibility domain as the verse itself.

This is fair-game security, not Sharecene real security.

## 15. Real security and fair-game security

Sharecene has two distinct security domains.

### Real security

Real security is Sharecene's responsibility.

It includes:

```text
runtime host safety
removal of dangerous Godot capabilities
local individual protection
rogue flag transport
phishing-aware URL presentation
safe direct-channel consent gates
```

This is the same category of responsibility a browser has toward hostile websites.

### Fair-game security

Fair-game security is the verse builder's responsibility.

It includes:

```text
preventing cheating
checking impossible movement
checking impossible resource changes
checking impossible combat events
checking impossible game claims
```

Sharecene provides Ambient History and validating functions as tools.

The verse author writes the fairness rules.

Sharecene does not decide who won.

Sharecene does not decide what happened.

Sharecene provides a medium in which impossible claims can raise alarms.

## 16. Presence and Status

Presence signaling is inevitable.

Anonymity is impossible.

This is not a bug and not a missing feature.

In a P2P system where clients connect, exchange addresses, transfer chunks, and open channels, participation reveals presence.

A client may use a pseudonym, but peer behavior, addresses, cache shape, and repeated presence can still identify continuity.

Therefore Sharecene does not promise anonymity or unlinkability.

Status is chosen.

Status may indicate availability:

```text
open
occupied
away
hosting-only
```

Status may voluntarily indicate the currently visited verse.

The currently visited verse is not announced without user-player consent.

Proof:

```text
network participation -> presence
user declaration -> status
verse-location announcement -> optional status content
```

Therefore:

```text
presence is inevitable
status is chosen
visited-verse announcement requires consent
```

## 17. Direct Channels

Direct Channels are one-way real-time peer connections.

A channel is a spectating view.

Interaction requires reciprocal channels.

When interacting, each client spectates the others.

A channel is the on-the-fly communication of Ambient History data as it is produced.

Sharecene does not grant authority through a channel.

Sharecene does not guarantee that the other peer accepts one's claims.

Consent is one toggle per verse.

It is not a per-interaction trigger.

Verses may explain the consent gate in-lore.

The user-player remains sovereign and can revoke consent at any time.

Proof:

```text
one-way channel = A observes B
reciprocal channels = A observes B and B observes A
interaction requires mutual observation
```

Therefore:

```text
interaction = reciprocal spectating
```

If either side revokes consent, reciprocal interaction ends.

Implementation detail to watch: direct-channel key setup should use key agreement and derived session keys, not a loose serialized `session_key_material` field.

## 18. Avatar Sheets

Avatar Sheets are client-managed, per-verse Markdown documents.

They are Sharecene's text-only inter-verse character-sheet feature.

They are not a global MMO inventory.

They are not platform economy state.

They are not canonical truth.

They are not a sixth verse-builder mechanism.

A client keeps a separate avatar sheet for each verse.

Example:

```text
local client
  avatar-sheets/
    verse-A.md
    verse-B.md
    verse-C.md
```

The sheets are free-form Markdown.

Sharecene does not impose a schema.

A sheet may contain:

```text
name
titles
inventory
scars
oaths
achievements
debts
relationships
visited verses
notes
references to log-stream objects
proof URLs
```

A verse may read every other verse's avatar sheet available inside the same client.

A verse may rewrite only its own avatar sheet.

The trusted browser shell owns the sheets and enforces this rule.

The de-implemented runtime accesses avatar sheets only through Sharecene-safe services, not through arbitrary filesystem access.

Rule:

```text
verse V may read avatar sheet of verse X
verse V may write avatar sheet of verse V
verse V may not write avatar sheet of verse X
```

Within its own sheet, a verse has full editorial freedom.

It may:

```text
ignore Avatar Sheets completely
create no sheet
erase its own sheet
rewrite its own sheet
append to its own sheet
replace its own sheet with a new interpretation
```

This means an avatar sheet is a verse's testimony about a user-player's avatar.

It is not a protected biography.

It is not a cross-verse law.

It is not automatically trusted.

A verse reading another sheet decides locally what to do with it.

Example:

```markdown
# Julien

Carries The Glass Thorn.

Owes three debts to the Red Chapel.

Was declared dead once, incorrectly.

Proof:
sharecene://relics/glass-thorn#<latest-known-content-hash>
```

### Avatar Sheets and log-stream-owned objects

Avatar Sheets may refer to log-stream-defined objects.

A famous object can be known by many verses and many peers.

The sheet does not contain the object.

The sheet contains text, references, and proofs.

Example:

```markdown
## Inventory

- The Glass Thorn
  - object stream: sharecene://relics/glass-thorn#<content-hash>
  - current owner proof: available on request
```

A verse can inspect the reference and ask for proof.

A valid proof is not merely the text line in the sheet.

A valid proof is verifiable continuation material or a verifiable stream continuation showing that the user-player controls the current ownership position.

Therefore:

```text
avatar sheet = testimony
peer log stream = proof
verse policy = interpretation
```

Sharecene does not force a verse to honor any avatar-sheet claim.

Sharecene only gives verses a safe way to read local cross-verse testimony and a way to verify log-stream proofs when the owner provides them.


## 19. Abuse model

Sharecene has two abuse levers:

1. Local individual protection.
2. Rogue flagging.

Local individual protection means immediate client-side action:

```text
drop
ignore
blacklist
block
disconnect
```

If a peer sends excessive requests, malformed data, unwanted channel attempts, or hostile traffic, the local client protects itself.

Rogue flagging means one user-player flags another peer as rogue.

A rogue flag is not proof.

Before review, the only certain fact is:

```text
A flagged B
```

Anything else may be false.

The signal bubbles up to verse authors, who may inspect what they can inspect and decide whether to ban a peer from their verse.

Sharecene does not run a court.

Sharecene does not provide central moderation.

Sharecene exposes signals and lets authors and communities act.

Implementation detail to watch: local abuse score or threshold logic must remain local and mechanical. It must not become shared reputation.

## 20. Time

Sharecene does not provide a global clock.

Time is not a Sharecene mechanism.

A client may know the current date and time.

A verse may use dates and times inside its own logic.

Cultural Knowledge may use date or revision markers for recency propagation.

Those markers synchronize knowledge.

They do not create global time authority.

Sharecene itself does not decide what time is.

## 21. Implementation anchor

The next implementation plan must be corrected around the editor-derived verse browser model.

Do not design Sharecene as only a hardened game runtime that loads PCKs.

Design it as:

```text
Godot editor-derived verse browser shell
Sharecene substrate replacing Asset Library/download layer
de-implemented Godot runtime for launched verses
normal Godot editor + Sharecene addon for authors
```

Important implementation corrections:

```text
verse browser shell must remain sovereign
verse launch needs a verse context, not raw PCK loading only
verse execution remains separate-process even when shown in shell-embedded view
ambient payload and relay envelope must be separated
provenance-bearing records need signed origin or must be explicitly non-authoritative
validator bundles must be verse-author-bound
direct-channel key setup must use derived session keys, not serialized key material
avatar-sheet storage must remain browser-shell owned
avatar-sheet runtime access must enforce read-all/write-own-verse
verse title-deed updates must be performed through trusted shell/addon paths
launched verse runtimes must not gain direct authority over their own public lineage
sandbox gates must pass before arbitrary PCK dogfood
```

## 22. One-month implementation target

The current exploration assumes a constrained implementation target.

The target is not a production-grade universal platform.

The target is one full pass of the five mechanisms under the strict non-features above.

This scope is only plausible because Sharecene refuses many common platform features:

```text
no consensus
no central moderation
no anonymity network
no guaranteed storage
no platform reputation
no matchmaking requirement
no account economy requirement
no platform-wide forced item semantics
no universal item enforcement
```

The implementation must prove the substrate, not become every possible multiplayer service.

## 23. Vega Conflict test

A later comparison against Vega Conflict can test the product boundary.

The question is not:

```text
Can Sharecene recreate every backend assumption of Vega Conflict?
```

The question is:

```text
Can a complex multiplayer verse with Vega-like capabilities be expressed using only Sharecene's five mechanisms?
```

If not, the missing parts should be classified as verse-author layers above Sharecene, not silently added to the substrate.

## 24. Final law

Sharecene is not a consensus engine.

Sharecene is not a court.

Sharecene is not an anonymity network.

Sharecene is not guaranteed storage.

Sharecene is a Godot editor-derived verse browser with P2P hosting, public presence, Cultural Knowledge, peer log streams, verse title-deed ownership, ambient validation traces, consent-gated spectating channels, and browser-shell-owned Avatar Sheets.

The design is coherent only if those limits remain explicit.
