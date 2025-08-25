# AI Sales Assistant – Ridealongs (Demo)

This repo contains two self-contained HTML demos for the **AI Sales Assistant – Ridealongs** configuration and briefing experience. Both versions are static (no backend) and use Tailwind via CDN, Google Fonts (Inter), and Material Icons.

## Files

- `ai-sales-assistant-ridealongs.html`  
  Base Ridealongs demo with appointments list, modal briefing view, and speech synthesis for briefings.

- `ai-sales-assistant-ridealongs-with-source.html`  
  Extends the base demo by displaying the **lead source** for each appointment (e.g., AI Webchat, Website Form, Phone Call, Referral) with contextual icons and TTS narration that mentions the source.

## Quick Start

1. Download one of the HTML files.
2. Open it directly in any modern browser (Chrome, Edge, Firefox, Safari).
3. Toggle **Ridealongs** to reveal action buttons:
   - **Call Your Agent**: Text-to-speech (TTS) summary of all upcoming appointments.
   - **View Upcoming Appointments**: Opens the modal to view and select a specific appointment for a detailed briefing.
4. Click an appointment card to populate the **Briefing** pane. Use **Play Briefing** to hear the AI read it aloud.

> No build step required. All assets load from CDNs. Internet connection is required for Google Fonts and Material Icons; Tailwind is also loaded via CDN.

## Features

- **Interactive modal** with responsive layout (list + briefing).
- **Speech Synthesis (TTS)** for appointment summaries and per-briefing narration.
- **Lead Source badges** (in the `with-source` version) with icon mapping.
- **Animated UX flourishes** (fade in, pulsing speaking indicator).
- **Graceful fallbacks** and hardening for null elements and unsupported APIs.

## Notable Implementation Details / Fixes

- Added a small utility for the custom background color:
  ```css
  .bg-ai-bg { background-color: var(--ai-bg); }
  ```
  This enables the `bg-ai-bg` class (Tailwind doesn’t know custom CSS variables by default).

- Replaced the nonstandard Material icon `checklist` with `fact_check` for consistent rendering across browsers.

- Guards for `speechSynthesis` API:
  ```js
  if (!('speechSynthesis' in window)) return;
  ```
  Ensures older/locked-down browsers won’t throw errors.

- Defensive event binding (null checks) to avoid runtime errors if elements are missing.

## Data Model (Demo)

Data is generated in-browser for demo purposes:

```js
{
  id: 'appt-1',
  title: 'Walkthrough & Quote',
  time: Date,
  attendees: [{ name: 'The Miller Family', address: '123 Maple St, Saskatoon' }],
  type: 'Quote' | 'Service',
  source: 'AI Webchat' | 'Website Form' | 'Phone Call' | 'Referral' // (with-source version only)
}
```

To wire up real data, replace the in-memory `RidealongAgent` class with calls to your backend or Vendasta APIs and map the results to this shape.

## Customization

- **Icons**: Update the mapping in `getSourceIcon` (with-source version) to reflect your own lead sources.
- **Styling**: Tweak CSS variables in `:root`/`body`:
  ```css
  --ai-primary: #7154ff;
  --ai-secondary: #00a3ff;
  --ai-bg: #f8f9fa;
  ```
- **Speech**: Change the TTS copy in `generateBriefingSpeech` and `generateAllAppointmentsSpeech` for your brand voice.

## Accessibility Notes

- **Keyboard**: Press `Esc` to close the modal. Focus management is minimal in this static demo—consider trapping focus within the modal and returning focus to the trigger for production.
- **TTS**: Users who rely on screen readers may prefer native SR output over TTS playback. Provide a mute/disable control if you integrate this into production.

## Browser Support

- Modern browsers (latest Chrome, Edge, Firefox, Safari).  
- Speech Synthesis support varies; the demo degrades gracefully if unavailable.

## Running Offline

- The files will open, but fonts, icons, and Tailwind (CDN) won’t load without internet.  
- For full offline support, self-host or bundle Tailwind, Inter, and Material Icons locally.

## Known Limitations

- All data is stubbed (no persistence).
- No routing/state management beyond vanilla DOM manipulation.
- No i18n or timezone handling beyond the browser default.

## License

Choose a license suitable for your organization or internal use. (MIT is common for demos.)

---

**Maintainer Tips**  
- Keep the two variants side-by-side while testing changes.  
- If you later bundle assets, verify the `.bg-ai-bg` utility remains defined after your build step.  
- Tailwind JIT builds may purge unused classes—ensure any dynamic class names are safelisted.
