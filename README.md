# AccessCity AI — Smart Accessibility & Civic Platform

An owner-private social impact / GovTech pilot for reporting accessibility barriers, planning candidate journeys on a checked route network, and tracking municipal resolutions. React UI with a Vinext/Cloudflare Worker API, durable D1 records, and private R2 photo/video evidence. Hosting access is controlled by Sites; no public audience is enabled.

## Included

- ChatGPT sign-in and display-name profile registration. The first profile is the pilot organization manager; subsequent profiles are community reporters. Intended for this owner-only pilot, not an open multi-organization rollout.
- Reporting by category, priority, address, latitude and longitude; optional browser geolocation and up to four media uploads (20 MB each).
- Search, status/category/authority filters, personal reports, schematic geographic issue map with precise OpenStreetMap links, municipal tracking and analytics. Managers can record a responsible authority in the immutable activity history; this does not send an official complaint or imply a municipal partnership.
- Accessible-route planning on manager-recorded waypoints and path geometry. Filters include recorded step-free access, minimum width, maximum slope, firm/paved surface and date checked. Nearby unresolved reports (within 25 m) are excluded by default, including Awaiting verification. Candidate paths remain conditional on the quality and freshness of recorded data. Demo campus data is explicitly illustrative, with GPS and GPX disabled. Live paths support written/read-aloud directions, browser-only location and nearest-segment guidance, and GPX export; there is no live street-routing engine or turn-by-turn navigation. Managers must add checked route data before live routing can be used.
- Organization responses progress through Reported → Acknowledged → In progress → Awaiting verification.
- A non-manager community account confirms a fix or reopens a barrier. Managers cannot verify; no direct manager transition to Resolved. Each new resolution attempt can be checked again; immutable activity events record the history. Concurrent stale status mutations are guarded by the issue's prior status and update timestamp.
- Persistent in-app status notifications, refreshed every 30 seconds. No email or SMS is sent. No outside organization is automatically contacted.
- Read aloud with speed, pause/resume/stop and form read-back; dictation across writing fields; voice navigation, numbered controls, field/option entry and two-step confirmation before form submission or verification. A keyboard command box provides the same command path when recognition is unavailable. These use browser Web Speech APIs with feature detection and error messages. Microphone permission is required. The browser may send speech to its recognition provider; AccessCity AI does not store those audio recordings.
- Saved adaptive preferences: text size from 100% to 200% across pages and dialogs, high contrast, spacing, touch targets, reduced motion, simple layout. Keyboard focus trapping, Escape to dismiss, semantic controls, screen-reader labels, skip link, status announcements.
- Sign-language complaint mode, photo/video uploads, in-browser camera-only signed-statement recording (up to 90 seconds), local preview before attaching, and manager-uploaded human-signed guides with written transcripts. Signed mode requires a video, verified against its stored server MIME metadata. No automatic sign-language translation and no preinstalled signed guide.

## Requested technology versus hosting

The requested target stack was React, FastAPI, PostgreSQL/PostGIS and cloud storage. This private deployment uses React, Workers, D1 and R2 because Sites does not host a Python/PostgreSQL server. FastAPI/PostGIS has not been implemented. The coordinate map has a schematic background, not live street-map tiles. Multi-organization authorization, email/SMS delivery, production moderation, and a reviewed signed guide remain follow-up work.

## Development and checks

Use the Sites skill's setup, install, build, preview and publication helpers. Do not deploy this source to a public audience without reviewing the private-pilot first-account manager bootstrap.

```
node scripts/test-accessreport.mjs
node scripts/test-accessibility.mjs
node scripts/test-route-planning.mjs
node scripts/test-atelier.mjs
node node_modules/typescript/bin/tsc --noEmit
node /root/.codex/plugins/cache/openai-curated-remote/sites/1.1.2/scripts/build-site.mjs
```

The API integration checks execute the real route handlers using an in-memory SQLite D1 adapter and R2 adapter. They verify schema SQL, registration, roles, media storage/read access, validation, status transitions, manager verification denial, reopening/reverification, history and notifications, and signed-guide publishing. They do not certify the hosted storage service or real microphone/audio output.

For local D1, apply each migration once using the starter's Wrangler local migration procedure. Hosted publication applies migrations independently. Do not commit runtime databases, evidence, credentials or environment secrets.

The hosting manifest and Site source repository retain project identity and deployment history.

Accessibility QA: verified 200% text settings with no horizontal page or settings overflow at the preview desktop viewport; exercised page navigation and numbered control activation through the shared command handler. Real speech recognition, voice output, camera capture, and testing with people using assistive technologies require device/user testing. No WCAG conformance certification is claimed. Voice navigation commands use English; dictation and requested device voices can use English, Tamil, or Hindi where available.

Civic QA includes route-network authorization and validation, geometry persistence and withdrawal, authority-assignment history, shortest-path preferences, unresolved/awaiting-verification barrier exclusion, resolved-path availability, stale/withdrawn segment filtering, reverse traversal, and GPX escaping. Geolocation guidance still requires testing on a real device and a surveyed live network.

## Civic Atelier design and implemented flows

- Forest Ink `#172522`, Porcelain `#F4F1E9`, Signal Lime `#D5EF79`, Terracotta `#C66C48` (with darker text/icon variants), and Mist `#DCE4DF`.
- Manrope editorial headings and Inter interface text. Four-pixel controls, restrained borders and motion, visible keyboard focus, warm citizen portal, and a compact dark organization command center.
- Citizen home uses live workspace counts or explicitly labelled examples. No invented testimonials, partnerships or real-world impact statistics.
- Geographic explorer supports keyboard/pointer pan, zoom, selectable cases and a case-list alternative. OpenFreeMap vector street maps, rendered with MapLibre GL JS, load by default, request only the visible viewport, retain browser cache defaults and visible OpenFreeMap/OpenMapTiles/OpenStreetMap attribution, and fall back to coordinate positions if unavailable. The empty map opens on Chennai, explicitly labelled as the default view. Use my area requests browser geolocation on demand; it does not create a case or persist the device position. The home map offers report creation and labelled example pins when there are no live cases. This is not a place-accessibility certification dataset.
- Four-step complaint composition: Location, Details, Evidence, Review. Drafts are account-owned and stored in D1, with owned-file validation. Publishing clears only the draft revision that was submitted, preserving a newer saved draft.
- Managers may attach their owned photo/video evidence to case updates. The citizen before/after section requires a community-resolved issue, original photographic evidence, and photographic organization evidence from an Awaiting verification event.
- Command center includes a map-linked incident register and a user-adjustable pilot first-response target. The target is not an official municipal SLA.
- Intelligence workspace is explicit about service availability: image model analysis is not connected; duplicate candidates use word overlap and proximity; review order uses reported priority plus capped age. These rules never merge cases or make civic decisions automatically. Browser speech and recorded-network routing are labelled with their actual capabilities.

The existing URL is retained to preserve project identity and data. Product/site title and interface branding are AccessCity AI. Existing browser preference keys remain unchanged so the rebrand preserves saved accessibility preferences.

### Hero asset and generation record

Final project asset: `public/images/civic-atelier-hero.png` (absolute checkout path `/workspace/sites/accessreport/public/images/civic-atelier-hero.png`). Created with the built-in image generation tool, not the CLI. The scene is concept photography and is labelled as such in the interface; it is never used as complaint evidence.

Final prompt:

> Use case: photorealistic-natural. Asset type: full-width editorial hero photograph for AccessCity AI, a premium accessibility-first civic application with an architectural-studio visual direction. Create one original wide landscape photograph, no UI, no text, no logo, no watermark. Scene: a contemporary public cultural building in an Indian city, warm limestone facade, concrete terraces, mature shade trees, an inviting clearly step-free gently sloped pedestrian ramp with practical handrails. A real-looking adult wheelchair user independently travels along the ramp, seen naturally from behind at mid distance, calm everyday urban scene with a few distant pedestrians. Composition: architecture and ramp create precise diagonal lines; wheelchair user toward right-middle; leave the left third quiet with shaded foliage and dark architectural details suitable behind white editorial text. Natural warm late-afternoon daylight, credible materials, understated film grain, documentary architectural photography mood, no science fiction. Color harmony: forest-ink shadows, warm porcelain stone, muted terracotta paving and natural olive foliage. No inspirational posing or exaggerated disability imagery. This is concept photography, not an actual complaint case or testimonial. Aspect ratio approximately 2:1 landscape.

Civic Atelier verification: API tests cover private drafts, stale-draft preservation, and resolution-evidence ownership. Duplicate review rules are tested without mutating reports. Critical foreground/background pairs meet 4.5:1 in automated contrast checks. Browser checks verified citizen/command layouts, case previews and 200% text with no horizontal document/settings overflow at the tested desktop viewport. This is not a full WCAG 2.2 AA audit. Testing with assistive-technology users, actual speech/camera/GPS and a real surveyed route network remains necessary before a public rollout.

Map-provider implementation note: the direct OpenStreetMap raster endpoint returned policy-block images in preview, so it is no longer requested. The map uses the documented OpenFreeMap Liberty style (`https://tiles.openfreemap.org/styles/liberty`) and a bundled MapLibre worker. It does not rely on a CARTO key or a bypass of the OpenStreetMap tile service.

Local map verification: TypeScript passes. Browser QA verified zero live cases, five clearly labelled example pins, pin previews, and case history. The cloud preview lacks WebGL2, so street rendering could not be visually verified there; this browser receives an explicit coordinate fallback and an external area link. Device geolocation was not granted during QA.

### Chennai responsibility routing and official handoff
Reports record place type and explicitly selected municipal jurisdiction. Public municipal places in Chennai suggest GCC maintenance/engineering; transport, education, hospital and private-building cases suggest the relevant operator/facilities team; unclear ownership enters civic coordinator review. Geographic coordinates do not prove jurisdiction or ownership. Managers can revise routing with audit history. No authority partnership or responder membership is claimed.

Official GCC grievance portal (`https://gccservices.in/pgr`) and 1913 verified via GCC's online-services page on 1 November 2026. Users review/copy a text summary, submit themselves via the official channel, then attest to and record a reference. No complaint/evidence is transmitted automatically, no API credential is configured, and official status is not synced. External references are explicitly user-reported and never advance case status. Reporter/manager authorization is enforced server-side. Changed place type/jurisdiction clears the current reference while preserving its audit event. Existing cases without routing metadata need coordinator review. New D1 migration: `0004_amused_sheva_callister.sql`.

Validation: API tests cover all routing types, jurisdiction guards, owner/manager handoff permissions, required reference attestation, rerouting history, reference reset, and unchanged case status. Browser checks cover the organization directory and example-case restrictions. No real government complaint was submitted during testing.
