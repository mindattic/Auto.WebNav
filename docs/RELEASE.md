---
layer: release
status: living
---
# AutoWebNav — Release charter

> The one finish line this repo is working toward. Every open ticket in this repo sits under one of the epics
> below, and every epic is part of the milestone. Work that does not fit is out of scope until this milestone is
> reached (owner, 2026-11-27, MAT-T319).

## 1. What this is
The one shared .NET 10 browser-automation library for MindAttic's WebView2 apps: hard timeouts, trusted CDP input,
dialog-free file upload, a self-healing element resolver, recording, Live Observation and always-on session
recording. Consumed as two NuGet packages by Automata, JobHunt, KdpPublish and MindAttic.Browser.

## 2. The milestone: AutoWebNav 5.0 {#milestone}
- **Milestone:** MS-90, "Auto.WebNav 5.0: stable, documented, consumers know how to move"
- **Published means:** a finished internal library. `AutoWebNav` 5.0.0 and `AutoWebNav.WebView2` 5.0.0 on the
  `C:\LocalNuGet` feed (packed by `tools/pack.ps1`), tagged `v5.0.0`.
- **The natural stopping point:** the library already does everything its consumers use. 5.0 is done when the
  README describes 5.0.0 (it still says 4.0.0), the upgrade path between majors is written down, one command shows
  which consumer is on which version (Automata is on 5.0.0; JobHunt, KdpPublish and MindAttic.Browser on 4.0.0),
  and the release is tagged. Moving each consumer to 5.0.0 is that consumer's own ticket.

## 3. Done when (acceptance checks)
1. build — `AutoWebNav.slnx` builds clean on main.
2. exists — `C:\LocalNuGet\AutoWebNav.5.0.0.nupkg` and `AutoWebNav.WebView2.5.0.0.nupkg`.
3. exists — `docs/UPGRADING.md`.

## 4. Epics, in priority order
| Priority | Epic (ticket) | What it delivers | Features |
|---|---|---|---|
| 900 | AWN2-T4 5.0 release | 5.0.0 documented and tagged | AWN2-T6 Docs current (990; task AWN2-T10), AWN2-T7 Release tag (980; task AWN2-T11) |
| 800 | AWN2-T5 Consumer alignment | Consumers can see and close the version gap | AWN2-T8 Upgrade guide (890; task AWN2-T12), AWN2-T9 Version report (880; task AWN2-T13) |

## 5. Out of scope (not before this milestone)
- Recording inside a closed shadow root, and attaching a file inside a cross-origin frame (the browser does not
  expose what either needs).
- Anything that knows about a specific website: it belongs in the consuming app.
- A public nuget.org release.
- Upgrading the consumers themselves (their repos' tickets).
- Extra unit-test coverage for already-working helpers, and open-ended bughunt or cleanup sweeps.

## 6. Current verified state
- Library extracted from Automata (9685ff7); Live Observation and SessionRecording (4d6ad24, 3.0.0); session
  recordings through MindAttic.Export (b1642b5, 4.0.0); off-screen lanes wire `NewWindowRequested` (296b774,
  AWN-T15, 5.0.0).
- NUnit coverage for BrowserActions, resolver, recording, declutter, known folders (AWN-T2, AWN-T3); bughunt fixes
  (AWN-T23, AWN-T24).
- 1.0.0 to 5.0.0 packages present in `C:\LocalNuGet`.

## 7. Decision record
- 2026-11-27 — charter drafted from README/AGENTS/pack.ps1/consumer csproj files/tickets; Legion vote: Approve, 9/9
  voters, plurality quorum reached, consensus 1.0; no dissent.
- Triage: 8 open (7 under the old repo key AutoWebNav, 1 under Auto.WebNav) → done 0, duplicate 1 (AWN-T12
  re-filed as AWN2-T10), wont-do 7 (AWN-T4, T5, T6 test nice-to-haves; AWN-T9, T10, T11, AWN2-T3 sweeps), kept 0;
  4 new tasks (AWN2-T10..T13).
