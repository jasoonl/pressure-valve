# Pressure Valve — anger venting bot

## Concept
A one-screen web app where you vent at whatever's making you mad. A chat bot ("the Valve")
answers in the style you pick, a pressure gauge climbs as you rant, and when you're done you
hold a release valve to blow the whole rant away as steam. Nothing is saved.

Metaphor: blowing off steam. Visual world = boiler-room hardware: a 0–150 PSI pressure gauge
with a redline, a brass handwheel release valve, a stamped spec plate. Real units (PSI) and
gauge conventions (tick marks, redline zone, damped needle) are the subject-specific detail.

## Flow
1. Opens in a working state: bot greeting in the chat, gauge at 0 PSI ("COLD"), target field
   with quick chips ("Mad at: ____").
2. User types rants. Each message:
   - scores an intensity (length, ALL CAPS ratio, !!!, stretched letters, angry words, swearing)
     → adds PSI to the gauge (clamped 4–45 per message, max 150)
   - gets a bot reply in the chosen mode
3. Above 120 PSI the gauge hits REDLINE and the valve pulses.
4. Hold the valve (pointer or keyboard, ~1.2 s, progress ring fills) → steam particle burst,
   user's messages evaporate, needle sweeps to 0, summary card (peak PSI, messages, words,
   caps outbursts) + "it's gone, nothing was saved".
5. Post-vent options: vent about something else / talk it through (switches to Cool me down).

## Reply modes
- **Take my side** — loudly indignant on your behalf, funny not cruel.
- **Just listen** — tiny acknowledgments, keeps you going.
- **Cool me down** — one line of validation, then perspective / what's in your control.

## Engine
- Primary: `sample` capability (Claude on the viewer's own account), `modelTier: "quick"`,
  `cache: false`, streaming into the bubble, Stop not needed (replies are short).
  Standing rules go in a leading user turn; history = last 20 turns.
- Fallback: built-in scripted replies (mode × intensity band, shuffle-bag, {target} filled in)
  whenever `sample` is null, declined, disabled, or refuses. Small "built-in reply" tag.
- Errors: not_granted / sampling_disabled / not_declared / capability_* → offline mode for rest
  of view with a one-line notice. rate_limited → tell user to wait. upstream_error → keep
  partial, offer resend. refused → scripted reply.

## Safety
- Bot rules: no slurs, no cruelty about identity, never encourage revenge or violence against a
  real person; cartoonish exaggeration OK. Won't help draft an angry message to send right now.
- Self-harm / harming others: local phrase check pins a support card (988 call/text, Crisis
  Text Line: text HOME to 741741) and those messages add no PSI. Claude's rules also tell it to
  drop the venting voice and respond with care. Scripted fallback has a care reply.
- Privacy line: the page doesn't save anything; closing it clears everything.

## Layout
- Desktop (≥ 860px): left instrument panel (spec plate, gauge, readout, valve, mode switch),
  right chat column (target bar, messages, composer).
- Phone: compact strip on top (small dial + readout + valve), chat fills the rest, composer at
  bottom. No horizontal scroll, 16px gutter.

## Design tokens
- Type: Big Shoulders Display (stamped industrial labels / title), Atkinson Hyperlegible
  (body/chat), IBM Plex Mono (PSI readouts).
- Light: cool steel ground, navy-ink text, brass hardware, enamel teal primary, heat red redline.
- Dark: deep slate-teal ground, same brass/heat accents tuned for contrast.

## Test plan (headless Chromium, mocked `window.claude`)
1. Offline mode (`use` → null): send messages, gauge rises, scripted replies, chips set target.
2. AI mode (mock streaming sample): reply streams in, history/rules turn shape is valid
   (starts/ends on user), mode text reaches the rules.
3. Error paths: not_granted → offline notice; rate_limited → notice; refused → scripted.
4. Crisis phrase → support card, no PSI added.
5. Hold-to-vent via pointer and via keyboard; short press does nothing; summary card appears;
   gauge returns to 0.
6. 400px width: no horizontal overflow; dark + light screenshots; console clean.

## Apple restyle (v2)
- System fonts (SF on Apple devices), no web fonts. Large titles tracked tight, small captions tracked open.
- iOS color system: grouped background, white/#1C1C1E cards, hairline separators, system blue for the user's bubbles, system red/orange/yellow only for pressure.
- Gauge: sleek 240-degree arc that fills cool-to-hot with a glass knob, big rounded PSI number in the middle. Redline is a thin outer arc.
- Valve: frosted circular button, thin line-art wheel, progress ring.
- Reply style: iOS segmented control with a one-line caption.
- Chat: iMessage-style bubbles; target bar and composer are frosted glass bars that stay put while messages scroll underneath.
- Composer: pill field with a round arrow-up send button.
- Panel is an inset rounded card on desktop, a compact strip on phones.
- Keep every id/class the script uses, keep all behavior, rerun the whole test suite.
