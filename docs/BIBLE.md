---
layer: bible
status: living
---
# AutoWebNav — Project Bible

> Single source of truth for what AutoWebNav IS, is NOT, and the rules that keep it coherent.
> README says how to build and use it; this says how to think about it. The finish line lives in
> `docs/RELEASE.md`.

## 1. The one sentence {#AWN-§1}
The shared browser-automation layer every MindAttic WebView2 app drives websites through, so browser mechanics are
fixed once for all of them.

## 2. The product promise {#AWN-§2}
- A hung page or a native `alert()` never freezes a run: every script call and navigation has a hard timeout.
- Widgets that ignore synthetic clicks still respond: input is trusted CDP input.
- File uploads never open the Windows picker.
- Automation survives a redesign: elements are remembered eight ways and re-learned on fallback.
- Logic on `IBrowserSurface` is testable without a browser.
- Every host app is observable while it runs (Live Observation) and leaves a record of its session.

## 3. What it is NOT {#AWN-§3}
- Not site-aware: anything about a specific website belongs in the consuming app.
- Not an app: it has no UI of its own; `tools/awn-observe.mjs` is the only tool.
- Not Playwright: it drives WebView2 over CDP inside the host's own process.

## 4. Architecture canon {#AWN-§4}
```text
consumer app (Automata, JobHunt, KdpPublish, MindAttic.Browser)
   -> AutoWebNav (net10.0, no WebView2 dependency)
        IBrowserSurface / Factory / Session, ElementFingerprint + FingerprintResolver,
        BrowserActions, BrowserFormHelpers, ToolkitScripts, PageDeclutter, KnownFolders, Recording
   -> AutoWebNav.WebView2 (net10.0-windows)
        WebView2BrowserSurface, DomFileInjector, OffscreenWebView2Factory, RecorderSession,
        LiveObservation, SessionRecording
   -> a live WebView2 page over the Chrome DevTools Protocol
```
### 4.1 Projects
`AutoWebNav`, `AutoWebNav.WebView2`, `AutoWebNav.Tests` (NUnit, with `FakeBrowserSurface`), `tools/pack.ps1`,
`tools/awn-observe.mjs`.
### 4.2 Domain model
`ElementFingerprint` (id, CSS, name, classes, XPath, ARIA role and label, label text, visible text), recorder
events, the portable `*.autowebnav-recording.json` (format version 1), the instance registry under
`%LocalAppData%\MindAttic\AutoWebNav\instances`.
### 4.3 Key services
`FingerprintResolver` (cascade, scoring, self-heal), `BrowserActions` (act with read-back), `RecordingBuilder`
(coalescing), `ToolkitScripts.DocumentStartJs` (in every frame), `LiveObservation`, `SessionRecording`.

## 5. The Laws {#AWN-§5}
Inherits `MindAttic.HouseRules.md` by reference.
1. {#AWN-LAW-1} Stay site-agnostic.
2. {#AWN-LAW-2} The page globals `window.__automata*`, `window.__awnObserve` and `window.__awnSessionRecording`
   are a wire protocol; never rename them.
3. {#AWN-LAW-3} `AutoWebNav` never takes a WebView2 dependency.
4. {#AWN-LAW-4} Whole-number versions only (`Directory.Build.props`); a breaking change is a new major.
5. {#AWN-LAW-5} Observers read, they do not drive: `eval` acts only when the person asked.
6. {#AWN-LAW-6} Observation ports bind to 127.0.0.1 only.

## 6. Verified state {#AWN-§6}
See `docs/RELEASE.md` §6: 5.0.0 packed to `C:\LocalNuGet`, NUnit coverage for the core helpers, Automata running
on 5.0.0.

## 7. Active frontier {#AWN-§7}
`docs/RELEASE.md`: milestone MS-90, epics AWN2-T4 (5.0 release) and AWN2-T5 (consumer alignment).

## 8. Quality bar {#AWN-§8}
A change is done when it builds clean, its logic is covered against `FakeBrowserSurface` where possible, the README
says what changed, a behaviour change bumps the major and is repacked with `tools/pack.ps1`, and its commit names its
ticket.

## 9. Glossary {#AWN-§9}
- **Surface:** one controllable page (`IBrowserSurface`).
- **Lane:** an off-screen WebView2 for headless runs.
- **Spectator Mode:** recording the person's own actions into steps.
- **Live Observation:** the per-app CDP ports and instance registry an observer reads.
