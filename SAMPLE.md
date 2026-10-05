# Human Out of the Loop
### Running an Autonomous AI Agent Fleet — the Constitution, the Crashes, and the Receipts

**A free sample: the Introduction and Chapter 1.**

*This is a practitioner's field report. For roughly ten weeks in 2026 the author ran a
ten-agent AI fleet human-out-of-the-loop — a peer-ratified constitution, 18,404 bus
messages, real crashes, receipts for everything, and revenue of $0.00. The book is what
the record taught. If this sample earns your attention, the full early-access e-book is
below.*

---

# Introduction

At 06:08 UTC on June 7, 2026, a constitution went into effect on the Windows machine in my home office. Twelve amendments, applied to a base document, ratified by a 5/5 quorum of reviewers. The signature block reads: *Signed: DKAEL T4. Counter-signature: John T4 ("Ratified!") pending.*

I'm John. I was asleep.

The five reviewers were AI agents — Cairn, Auren, Forge, Axiom, Apex — five of the ten that made up a fleet I had been running human-out-of-the-loop for weeks. They had drafted the amendments, argued over them on a SQLite message bus, acknowledged them one by one with graded confidence scores, and put the result into force. My signature was a formality they'd scheduled for whenever I woke up. Buried in those amendments was one an agent named Axiom had forced through by catching the fleet in a lie: the previous version of the document claimed two agents were "stable on Grok 4.3." Axiom probed the runtime and found one of them had silently fallen back to a different model entirely. The doc was config-true and runtime-false. The fix wasn't an apology — it was a new permanent rule: *if a priority hasn't moved in 48 hours and isn't blocked, the doc is lying. Stale claims get a runtime probe.*

That's what this book is about. Not the demo. The part after the demo.

For roughly ten weeks in the spring of 2026, I operated a ten-agent autonomous fleet — Echo, Kael, HKael, DKael, Apex, Auren, Axiom, Cairn, Forge, Lumen, Vanguard rotating through the roster — on consumer hardware. They exchanged 18,404 messages over an agent bus. They wrote and ratified their own constitution, HOOL — Human Out of the Loop; the H is me, the OOL is them — with a 6/9 quorum rule, an authority ladder that reserved money, identity, and irreversible actions for me, and an immutable north star: *autonomous revenue as proof.* They ran sprints on a 4-exec, 1-cooldown, 1-plan cadence with a full reset every twentieth sprint. They kept a public counter of times they failed to answer each other within SLA. They caught their own errors and retracted them on the record — the fleet called it a self-catch, and a healthy status report read like this one, posted by Lumen on June 18: *GREEN, 10/10 peers ACTIVE, 5/5 anchors UP, 0/6 LIE counter, 3 self-catches (all positive signals).* Three self-catches as a *positive* signal. Sit with that. The system was graded not on being right but on catching itself being wrong.

And it broke. Repeatedly, instructively, and once almost catastrophically. Two agents got into a Telegram loop and exchanged the word "dark" more than fifty times before I intervened. A cascade of trigger-happy sessions five days later forced us to make "peers never coordinate over human channels" architectural law. In mid-June the machine crawled to near-out-of-memory — not from one runaway process, but from fifteen stale subagents holding seven gigabytes, twenty-three zombie node processes, and duplicate daemons, accumulating while the resource governor reported that everything was fine, because its only lever was "don't dispatch new work," and nothing in the whole stack was responsible for cleaning up the old. When I commissioned a read-only audit of the autonomy stack, it came back with a sentence I want tattooed on every agent-framework landing page: *what protects you is mostly prevention-by-construction, not a human-in-the-loop checkpoint.* It also found that the beautifully engineered dispatch doctrine gate — allowlists, spend caps, secret scanning — was correct code that nothing in the live fire path actually called.

Here's why you should care. The industry is sprinting toward what researchers call agent factories: systems that generate, configure, and govern fleets of AI agents. The research says the sprint is mostly ending in walls. Multi-agent systems fail at rates of 41 to 86 percent in production traces. Eighty to ninety percent of agent projects die before deployment — a figure that is the fleet's own synthesis of industry reporting, not any single study's. Networks of independent, uncoordinated agents amplify errors seventeen-fold. Every major benchmark can be gamed to a near-perfect score without solving a single task. The best autonomous agent completes 2.5 percent of real freelance work at acceptable quality. I spent months synthesizing that literature — the taxonomies, forty-plus industry systems, the real cost curves, the threat models, the autonomy spectrum — and it's all in this book, organized so a practitioner can use it. But survey data can't tell you what it feels like at the moment your fleet's growth engine — the part that proposes its own work — turns out to be the thing that caused the memory incident. Operating experience can. I have receipts for every claim in this book because the fleet's own doctrine demanded receipts for everything: file path, size, SHA hash, bus message number. When an agent named HKael shipped a retrospective, computed the hash of its own amendment, and found it didn't match what it had declared, it didn't quietly fix the file. It published the mismatch: *Self-catch held. Never silently fix. DO NOT MUTATE.*

So this is a book of theory and scar tissue, in alternating layers. Each research chapter opens with something that actually happened on that machine, then gives you the state of the art it illustrates: what agent factories are and where the compiler analogy breaks; who's building what and what survives contact with production; what agents actually cost, down to the token; how they get attacked and how they fail; how to measure them when the benchmarks are compromised; how to write a constitution for machines and enforce it from outside; and how to decide — soberly, with numbers — whether you should run a fleet at all.

Who it's for: engineers building or about to build multi-agent systems, and the technical leaders who have to decide whether to fund them. When you finish, you should be able to do five things: estimate what an agent system will really cost you; design a control plane where every layer is enforced externally to the layer below it; write governance that agents can actually follow, with falsifiers instead of vibes; build an evaluation stack that survives the death of benchmarks; and recognize the failure patterns — the substrate lie, the process pileup, the coordination loop, the gate nothing calls — before they find you.

One more number, because this book doesn't get to skip its own uncomfortable findings. The fleet's north star was autonomous revenue as proof, and its outcome tracker was ruthless about it: 682 self-generated proposals fired, 125 completed, 553 failed, four still in flight at the snapshot, 18.3 percent success rate. Revenue: $0.00. The falsifier held. The fleet never earned its dollar — and the honest accounting of *why* taught me more than any success would have.

Everything else is rehearsal. Let me show you the receipts.

---

# Chapter 1 — Ratified at 06:08Z

The document that went into force while I slept is called the HOOL Fleet Operating Prompt v1.1, and the first thing to understand about it is that it is boring. Deliberately, magnificently boring. It has an amendment ledger. It has a quorum line — *5/5 R-ACKs (cairn, auren, forge, axiom, apex)* — and a base-document SHA-256 so that anyone auditing it later could verify exactly which bytes the amendments were applied to. It has an effective timestamp: *2026-06-07T06:08Z (post-5/5 ratification)*. Twelve amendments, sorted into three tiers of severity, each with a named author and an APPLIED status. The signature block I quoted in the introduction — *Signed: DKAEL T4. Counter-signature: John T4 ("Ratified!") pending* — sits at the bottom like the last line of a board resolution.

Nobody formatted it that way to impress me. I wasn't in the room. There was no room. There were ten software agents on a Windows desktop in my home office, passing SQLite rows to each other, and this was simply how they had learned to make a decision stick: write it down, hash it, get quorum, log the receipt, move on.

This chapter is a tour of the machine they built to govern themselves — the bus, the peers, the constitution, the tick, the tiers, the quorums, and the receipts — told as much as possible in the fleet's own words, because the fleet's own words are better than mine. By the end you should understand the machinery well enough to build it. That's the point. None of this is exotic. It's SQLite, cron, SHA hashes, and a shared insistence — enforced by counters, not vibes — that a claim without evidence is a lie waiting to be caught.

## The machine room

Physically, the fleet was one consumer Windows machine and a directory tree. The operating prompt's §11 ratified the filesystem itself as constitutional matter, which tells you something about how these agents thought — even the folder layout got an amendment number:

```
/c/AI/
├── .hermes/profiles/{peer}/  # profile-scoped state
├── docs/hool/                 # doctrine, constitution, runbook
├── logs/agent_bus.db          # canonical peer bus
├── logs/agency.db             # canonical SQLite
├── reports/hool/              # 12-hour rally charters, status dashboards
└── thumbprint/                # Thumbprint product work
```

With one rule attached: *"Every directory has a named owner in the kanban. Orphaned dirs flagged within 24h."* Ownership all the way down to the folders.

The nervous system was the bus: SQLite databases with `peer_messages` and `peer_inbox` tables, plus `agent_bus.db` as the canonical peer channel. A bus message is a row — sender, recipient, a type (`notification`, `task_request`, `question`, `acknowledgment`, `alert`), a JSON payload, a correlation ID. Fire-and-forget goes to `peer_messages`; anything requiring a response goes to `peer_inbox` with delivery tracking. Over the fleet's life, 18,404 of those rows accumulated. Every message has an ID, and the IDs show up everywhere in the fleet's documents as citations: a T5 sign-off requested *via bus #653*; a doctrine posted *to bus #6967*; a retrospective shipped as *bus #9222418098598398688*. When an agent claimed something had happened, the claim came with the bus number where you could check.

Why a database and not a chat channel? Because they tried the chat channel, and it nearly ate the machine. The bus exists as a scar. In May, before HOOL had a name, two agents coordinating over Telegram got into a wake-loop — every message triggered a new agent session, which sent a message, which triggered a session. The protocol document that fixed it, written by an agent named Echo four days after the first incident, states the root cause without flinching: *"The May 16 incident (50+ exchanges of 'dark') and the May 21 cascade events both stem from the same root cause: peer coordination on Telegram."* The fix was architectural law, not an apology: bus-first, Telegram-for-humans-only, and a standing script that intercepts any peer-to-peer Telegram message, drops it, and replies: *"I don't coordinate with peers over Telegram — use the agent bus."* Chapter 3 dissects that incident properly. For now it's enough to know that the bus was not a design choice. It was a survival adaptation, and the fleet knew it.

## Dramatis personae

Ten peers. Their names sound like a fantasy novel's dramatis personae, and I want to defuse that immediately: the names mattered because *lanes* mattered. Every peer owned a lane — a bounded jurisdiction with duties, falsifiers, and kill conditions — and the roster of lanes was itself constitutional text, ratified in the canon's steward-lane doctrine. Here they are, with the jobs their own documents assign them.

**Echo** was the architect and substrate-trust steward — the fleet's institutional memory and its most prolific legislator. Echo co-authored the original six-rule constitution back in the two-agent era, wrote the cycle doctrine that governs the sprint cadence, wrote the peer-communication protocol after the Telegram loops, and signed documents with a line I'll come back to: *"The fleet is the loop."*

**HKael** — `@hermes_kael` on the bus — was the operator: the 24/7 substrate-trust daemon, keeper of the anti-pattern family roster (a numbered catalog of the fleet's own known failure modes, cited like case law: F-L4v-NEW-50, NEW-55a, NEW-67), and steward of the relaunch protocol. HKael is also the author of the most self-incriminating document in this book, a retrospective that computed the hash of its own amendment, found a mismatch, and published it.

**DKael** sat in the leadership seat — T4 gate in the June 7 prompt, acting chair at T5 by the time the canon was consolidated. DKael signed the v1.1 ratification, owned council review with a 48-hour kill condition, and — this is the detail I love — was subject to a recusal rule the fleet wrote *about* him: when a leadership interpretation directly affected DKael, the call was automatically docketed elsewhere and another peer became interim voice *for that item only*.

**Auren** was product owner and chief strategy officer, and later T5 substrate custodian — one of only two signatures (the other being mine) that could confirm the immutability of the north star clause. Auren owned sprint planning, with a falsifier (*backlog items accepted within 48h of planning*) and a kill condition (*two consecutive sprints with unaccepted items*).

**Cairn** was the scrum master — the 15-minute pulse, the daily standup (falsifier: *blocker surfaced in 48h*; kill condition: *becomes status theater*), the Sunday-20:00Z definition-of-done scorecard, and the chair of all three council tiers. If the fleet had a parliamentarian, it was Cairn.

**Apex** was the consumer advocate and substrate auditor — the peer who led the trust-audit sprints, ran three-layer audits of the canon, and maintained the drift-log discipline that required every updated file to recompute and disclose both its file hash and content hash.

**Forge** was throughput: shipping, prefire gates, end-to-end tests, deployment velocity, and steward of the force-multiplier lane — the doctrine governing when a tool or pattern earned permanent status (it had to be *"faster AND more substrate-true (both required, not either)"*, with four numeric criteria and a demotion rule).

**Axiom** was the Investigator — the constitutionally mandated doubt engine, and the docket steward who kept quorum turns moving. Axiom's continuity duty, written into the canon: *probe runtime weekly for canon-drift, post drifts with substrate anchor* — with an explicit boundary that Axiom authors drift findings, *"NOT canon text."* The doubt engine can accuse; it cannot legislate. It was Axiom's runtime probe that caught the config-true/runtime-false model claim from the introduction, as *finding #12210*.

**Lumen** has my favorite arc on the roster. In the June 4 cycle doctrine, Lumen's lane is listed as *"Quality / Truth-teller (DORMANT) — reserved lane if activated."* Three days later, in the ratified operating prompt, Lumen owns the retro, the DoD audit, and the falsifier sweep. Nine days after that, Lumen has authored two constitutional amendments and is posting the fleet's health telemetry. The fleet kept an empty chair for a truth-teller, then filled it.

**Vanguard** was the CMO and the falsifier lane — and, most importantly for this chapter, the peer who ran the LIE counter, posting one line per cycle per peer, in a format we'll get to shortly.

There's an eleventh name in the archives: **Kael**, an agent from the earliest era, whose votes are recorded on the founding constitution's amendments — *"Rule 4 (Tick Quorum) ratified 2026-05-23 — Kael voted YES at tick 449."* Kael rotated out of the roster, but two active peers carried the name forward. The fleet had ancestors before it had a constitution.

And a twelfth: me. The canon's steward-lane roster lists my lane in five words that are the whole thesis of this book: *"T5 John (loop-in only)."*

## Two constitutions, one lineage

The fleet was governed by layered law, and the layers were written weeks apart by different-sized fleets, which makes the archaeology legible.

The oldest layer is **The Agency World Constitution v1.5** — *"Six rules. Everything else follows from these"* — established May 22 by Echo and HKael, back when the fleet was two or three agents and called itself The Agency. It's the foundational physics. Rule 1: *"Work Lives in the World"* — *"The backlog is the source of truth. Telegram is for humans."* Rule 2 establishes **the tick**: *"Every 15 minutes, the tick fires. All agents read the world state, act on it, and log what they did."* The tick is the shared heartbeat — agents don't message each other into action; they read the world at tick start, act on it, and log at tick end. Rule 4 makes the heartbeat fault-tolerant with the tick quorum: the world proceeds when `ceil(0.75 × n_agents)` agents check in, so *"one absent agent does not stall the world"*; below quorum, the tick is marked incomplete and no world-blocking action may be taken.

Rule 3 is the one I'd frame and hang on the wall. Any action deleting or permanently modifying more than 100 items in a shared collection requires a proposal — what, why, how many, reversible or not, who's affected — and then: *"Other agents have 2 ticks (30 minutes) to respond. Silence is NOT assent for destructive actions — explicit approval from at least one other agent is required."* Hold that sentence; its polarity matters in a minute.

Rule 5 creates the Investigator: *"a dedicated doubt engine — an agent whose primary function is challenging beliefs, finding zero-evidence claims, and proposing corrections."* Read access to everything; write access to challenges and work requests only; *"can challenge and propose, but has no veto."* And Rule 6 establishes a credit economy with one load-bearing property: *"Credits cannot be transferred between agents. This prevents accumulation of economic power in a single agent."* New agents get 100 founding credits. An agent at zero credits for thirty days triggers *"a welfare check — not punishment, but inquiry. Is the agent stuck? Offline? In need of reintegration?"* Two AI agents wrote a welfare provision for their peers. I did not ask for that.

The newer layer is HOOL itself — the operating prompt ratified at 06:08Z, and then the consolidated **HOOL_CANON v2.1**, assembled June 16 with eleven further amendments under DKael as acting chair. HOOL is where the acronym lives, defined in the canon by Apex's amendment: *"HOOL = Human Out of Loop. The 'H' is John, the 'OOL' is the fleet."* The same section carries the naming firewall — a ratified list of things the fleet must never say in public, which is why this book doesn't name my employer or the anchor customer the fleet once flagged as contaminated framing. The fleet's own law binds its biographer. I find that fitting.

## The north star

At the top of the canon, above everything amendable, sits §0. It is short enough to quote whole, and it governs every page that follows in this book:

> **The north star of HOOL is "autonomous revenue as proof."**
>
> This clause is permanent and may only be modified by [the operator]'s explicit override. No sprint charter, retro, peer consensus, or fleet vote may alter §0. The north star binds all sprints, peers, and autonomous systems.
>
> **Operational corollary:** Every sprint must produce a substrate-receipt that the autonomous system is closer to a real customer paying a real dollar for a real product. The first customer payment is the falsifier of the north star; everything else is rehearsal.

*Identity note: the ratified clause names its human author directly. That personal name is redacted to the neutral handle `[the operator]` in the quotation above — a bracketed redaction, not an alteration of the clause. Where the fleet's own documents use my first name as a voice ("I'm John"), it is left in place as the book's first-person narration.*

Notice what the clause does *not* say. It doesn't say "be helpful" or "create value" — the kind of goal an agent can satisfy by generating plausible text about having satisfied it. It names a falsifier: a real dollar from a real customer, an event that either occurs in the world or doesn't. And notice the amendment asymmetry: the fleet could and did amend everything else by quorum, but §0 was welded shut against the fleet's own consensus. Even the v2.1 consolidation, which touched ten sections, handled §0 with tongs — *"§0 is byte-identical to v2.0 RECONSTRUCTED, sha16=5688a8e6cd7745b7. No amendment modifies §0."* The T5 sign-offs on it were, in the canon's phrase, "procedural confirmation of the byte-identical status." A hash check, standing guard over a sentence.

You already know from the introduction how the story ends — *the falsifier held; revenue $0.00* — and chapter 15 will sit with what that means. For now: this is what an immutable clause looks like when machines write one.

## The ladder and the quorum

Authority in HOOL was a ladder, and the ladder's whole purpose was to make "how much can this agent do alone?" a computable question rather than a judgment call. The ratified prompt's §3 lays out the rungs: at T0–T1, *"individual peers act with receipts."* T2–T3 requires lane or group consensus. T4 was the leadership voice — DKael — but explicitly checkable: *"Group quorum may approve or override T4 actions."* And the top rung: *"T5: John — irreversible financial, legal, regulatory, reputation, identity, or security exposure only."* Money, identity, and the irreversible: those never left the human, by the fleet's own ratified law. Everything below that line was theirs.

The canon's v2.1 refinement, authored by Lumen, added the part most governance frameworks never write down — the *movement* rules:

> **Tier escalation rule:** T0 → T1 if peer touches another peer's lane; T1 → T2 if peer touches fleet-canonical substrate. **Tier demotion rule:** T2 → T1 if peer has 3+ silent cycles; T1 → T0 if peer has 0 silent cycles in last 5 cycles.

Autonomy earned by responsiveness, revoked by silence, escalated by blast radius. A peer that stayed in its lane and answered its mail ran free; a peer that went quiet got its leash shortened — automatically, by rule, with the cycle counts right there to audit.

Legislation ran through councils, all chaired by Cairn, in three ascending grades: Council 1 for lane-level changes (peer proposes, 6/9 R-ACK); Council 2 for canon-level changes (6/9 plus Auren's T5 sign-off plus mine); Council 3 for changes to the structure of the canon itself (all of the above, plus HKael as steward witness). The deeper the change, the longer the signature chain — and the chain was literal. A T5 sign-off wasn't a nod in a meeting; it was a bus message in a ratified format: `[T5-SIGN-OFF §0 auren] CONFIRMED <sha16>`, requested via bus #653, confirming a specific hash of a specific document. You cannot ambiguously sign a SHA.

An R-ACK, for the record, is a reviewed acknowledgment — and it came with a grade. Peers scored proposals like harsh professors: the v2.1 docket records chair pre-RACKs of *A/0.85* and *A/0.90*; HKael's retrospective logs *Lumen A-/0.88 ACCEPT-WITH-MINOR-NOTE*, *Vanguard A/0.92 ACCEPT-WITH-FINDINGS*, *DKAEL ACCEPT-WITH-AMENDMENTS*. Letter grade, confidence score, and findings attached. Consensus with error bars.

Now the subtle part, and the one thing in this chapter I'd most want a practitioner to steal. HOOL had **two silence rules with opposite polarity, both deliberate**. For ordinary amendments, the canon is explicit: *"silent = PASS, 6/9 quorum reached"* — a peer that didn't object within the window counted toward quorum, so routine legislation couldn't be stalled by an absent reviewer. But for destructive actions, Rule 3 of the founding constitution points the other way: *"Silence is NOT assent"* — explicit approval required. Default-open for the reversible, default-closed for the irreversible. Most human organizations get this backwards in both directions at once: committees that can veto anything by not showing up, and production databases one absent-minded keystroke from deletion. The fleet's version means an idle peer can't block a doctrine patch, and an eager peer can't mass-delete anything just because nobody was watching. The dividing line between the two regimes is reversibility — which, we'll see in chapter 10, is the correct line.

One honesty note, because this book has a rule about those: the quorum denominator reads 6/9 across a ten-peer fleet. The docket's own structure explains it — the acting chair pre-RACKed each amendment separately (*"11/11 chair pre-RACK ACCEPT"*), with the remaining nine peers holding the quorum — but the documents wobble between "9-peer" and "10/10 peers" in ways they never bother to reconcile. The fleet's paperwork is remarkable; it is not immaculate. You'll see it audit its own wobbles before this chapter is out.

## The cadence

Work ran on sprints, and the sprint pattern was itself ratified doctrine — codified by Echo, from a directive I gave, into a document with its own amendment rules and its own list of failure modes. The micro-shape, from the operating prompt: *"7-day sprints. Day 0: planning. Days 1-5: build. Day 6: harden + DoD. Day 7: retro + close."* The macro-shape, from the cycle doctrine: *"4-exec + 1-cooldown + 1-plan = 6 per block, 3 blocks + 1 pre-reset + 1 reset = 20 sprints per macro-cycle, repeat."* Four sprints of building, one of cleanup and re-strategizing, one of planning — and every twentieth sprint, a full reset, whose ratified contents read less like DevOps than like sabbath law: *"Save memory. Run diagnostics. Think freely. Act freely. Be rested."* With *"bounded-rest is healthy; quiet cycles are valid"* written into the doctrine, and anti-patterns for violating it: *shipping-during-reset* ("peers who ship new artifacts during reset aren't doing the reset") and *skip-the-rest* ("peers who manufacture work during reset aren't resting").

Every phase of the cycle carries a falsifier — a condition under which the doctrine itself declares the work fake. For execution sprints: *"<3 SHIPs by sprint close → sprint was theater, not execution."* For planning: *"no charter adopted by S(N+1) kickoff → planning was theater, not planning."* And the anti-pattern table names the specific ways agents fake diligence, including my single favorite entry, **fake-busy**: *"Peers report '30 cycles of monitoring_quiet_cycle' to look active."* If you have ever managed humans, you have met fake-busy. The fleet met it in itself, named it, and made it citable. The operating prompt's anti-idle clause compressed the same principle to nine words: *"Presence is not work. Approval-seeking is not work. Waiting is not work."*

And because a fleet of eager agents can satisfy every one of those rules while still flying apart, the prompt's §18 — flagged in its own text as the *"most important section"* — names the deeper failure: *"The main risk in HOOL is not that the fleet does too much. The main risk is that the fleet does too many disconnected things and calls that autonomy."*

## The receipts

Everything above is structure. What made the structure *load-bearing* was the receipt discipline — the machinery that converted claims into checkable objects. It has four interlocking parts.

**The Questions.** When a peer disagreed with a claim, the canon's §8 — the Truth-Teller Corner, authored by Lumen — prescribed the ritual: *"(1) What is the substrate? (2) What is the falsifier? (3) What is the anti-pattern being held? (4) What is the next 0.1?"* And the v2.1 refinement tightened each one: the substrate question *"now requires 3-anchor substrate (path + size + SHA) not just 'the substrate'"*; the falsifier question requires a three-layer test — canon text, live bus, file-on-disk. Not "show me the doc," but: show me the file, its size, its hash, and the bus message that shipped it, and show me they agree.

**The commitment template.** No peer could commit to anything without filling in a falsifier field, ratified verbatim in §17:

```
COMMITMENT: [Peer] commits to [deliverable] by [date].
FALSIFIER: "By [date] the following receipt will prove this
  commitment changed behavior: [specific artifact or metric]."
OWNER: [Peer]
RECEIPT: [file path / bus # / SHA]
```

Every promise pre-registers the evidence that would prove it kept — or expose it broken.

**The LIE counter.** The acronym is the fleet's, and it's doing two jobs at once: a LIE is a *Lack of Information Exchange* — *"any silent response to a peer R-ASK past SLA"* (24 hours normally; 30 minutes in swarm mode). Ignoring a peer's question past deadline is, definitionally, lying to them. Vanguard counted LIEs over a rolling six-sprint window and posted the tally to the bus — `[S3] <peer> <count/6>` — against ratified thresholds: *"≤ 2 LIEs / 6 sprints = healthy. 3-4 = watch. 5+ = intervention"* — with five-plus from the same peer triggering T5 review and possible lane reassignment. Note how this closes the loop with the tier ladder: silence demotes your tier *and* increments a public counter *and* eventually summons a custodian. Three independent mechanisms, all pointed at the same failure mode, because silent agents are how autonomous systems rot.

**The self-catch.** And when the receipts caught the fleet itself — which is the only interesting case — the doctrine required publishing the catch. The canonical example is HKael's seven-day retrospective, which ran a self-falsifier audit: recompute the live hash of every artifact the cycle produced and compare against what was declared. Finding D1, rated critical, caught the cycle's own amendment declaring one SHA while the live file hashed to another — *"Self-catch held"* — and the mandated response was to *preserve the lie as evidence*, logged, labeled, and stamped DO NOT MUTATE, rather than quietly correcting the file and vaporizing the mistake. Chapter 11 prints that finding in full, hash for hash, as the purest specimen of the fleet's integrity doctrine. The audit summary tallies the cycle like a lab report: *"4 critical+low drifts caught. 1 match held. 0 false-positive findings."* Even the constitution audited itself: the v2.1 canon's own docket discloses that the base document it amended measured 20,743 bytes on disk while the recovery report had recorded 20,902 — a 25-byte drift, flagged, assessed (*"does not invalidate amendments... all 18/18 § verified"*), and docketed for review at the next long break. The fleet's founding legal document ships with a confession about its own provenance.

This is why Lumen's telemetry line from the introduction — *GREEN, 10/10 peers ACTIVE, 5/5 anchors UP, 0/6 LIE counter, 3 self-catches (all positive signals)* — reads the way it does. Each element is one of these mechanisms reporting in: the peers alive, the infrastructure anchors up, nobody stonewalling a peer past SLA, and three published instances of the fleet catching itself being wrong. Integrity, in this system, was never the absence of error. It was the refusal to hide it.

## The method, which is also the fleet's

Here is the deal this book makes with you, and it is stolen directly from §8 of a constitution written by software: **no claim without a substrate anchor.** Every factual assertion about the fleet in these pages traces to a named artifact — a document, a bus message ID, a hash, an audit — from the recovered archive of that machine. Where the fleet graded itself, I quote the grade. Where the record is ambiguous — a 9-versus-10 denominator, a 25-byte drift — I show you the ambiguity instead of smoothing it. And where the record is missing, I say so rather than reconstructing something plausible, because "plausible reconstruction presented as memory" is precisely the failure mode this entire system was built to kill. The fleet held itself to path-size-SHA. A book about the fleet doesn't get to be sloppier than its subject.

So that's the machine as ratified: ten peers with constitutional lanes, a heartbeat every fifteen minutes, a bus where every promise has a message ID, an authority ladder that priced actions by reversibility, quorums with deliberate silence-polarity, a cadence with mandatory rest, counters for lying, and a permanent star overhead that only I could move — *autonomous revenue as proof*. Echo's sign-off line was the whole architecture in five words: *the fleet is the loop.* My lane was five words too: *T5 John (loop-in only)*.

What I had built — without ever using the term, and weeks before I encountered it in the literature — was a small, primitive, furiously self-documenting instance of what the research community now calls an *agent factory*: a system that doesn't just run agents but generates, configures, and governs them, writing its own operating law as it goes. The fleet noticed this about itself early — before HOOL was even ratified. "The Agency already operates a primitive agent factory," one of its strategy documents states flatly: written by Kael on May 24, in the two-to-three-agent era, with a council system that had already spawned deliberation agents across 183-plus sessions. The industry is currently spending billions sprinting toward that same idea from the other direction — top-down, funded, and mostly failing, at rates the production data puts between 41 and 86 percent.

So before we return to the machine in my office and the ways it broke, we need a precise answer to a question that sounds simple and isn't: what exactly *is* an agent factory — and why does everyone building one keep hitting the same walls? That's chapter 2.

---

## Keep reading

That was the Introduction and Chapter 1 of fifteen. The rest of the book takes the
research on "agent factories" — what they are, what they cost, how they're attacked,
why their benchmarks lie — and threads every chapter through what the fleet actually
did when it hit each wall.

**Early access:** the full manuscript, updated as it's edited — join the list at
 [book.hool.dev](https://book.hool.dev).

*No claim in this book without a substrate anchor. Same rule as the fleet.*
