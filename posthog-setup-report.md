# PostHog post-wizard report

The wizard has completed a deep integration of PostHog event tracking across 8 HTML pages of the PickMeLabs static site. PostHog was already initialized via the CDN snippet on all 15 pages — this integration adds explicit `posthog.capture()` and `posthog.identify()` calls to track the key conversion events and user interactions. No new files were created; all changes are additive edits to the existing `<script>` blocks.

| Event name | Description | Files |
|---|---|---|
| `contact_form_submitted` | User submits the contact/inquiry form — the primary lead conversion event | `index.html`, `about.html`, `methodology.html`, `protein-powder.html`, `mattress.html`, `beauty.html` |
| `hero_cta_clicked` | User clicks "Check Your AI Visibility" in the homepage hero section | `index.html` |
| `offer_cta_clicked` | User clicks "Start with a conversation →" in the pricing/offer section | `index.html` |
| `nav_cta_clicked` | User clicks "Get in Touch" in the navigation bar | `index.html` |
| `research_experiment_clicked` | User clicks a research experiment card | `index.html`, `research.html` |
| `concept_card_clicked` | User clicks a concept card | `index.html`, `concepts.html` |
| `external_link_clicked` | User clicks an external research citation link | `methodology.html` |

On each contact form submission, `posthog.identify()` is also called with the submitted email as the distinct ID and `name`/`brand` as person properties, linking the session to a known lead.

## Next steps

We've built a dashboard and five insights to monitor user behaviour based on these events:

- [Analytics basics (wizard) Dashboard](https://us.posthog.com/project/454383/dashboard/1720794)
- [Contact Form Submissions](https://us.posthog.com/project/454383/insights/kIkpwlCr) — weekly trend of form submissions across all pages
- [CTA Engagement](https://us.posthog.com/project/454383/insights/HulidJu3) — daily hero, offer, and nav CTA clicks
- [Research & Content Engagement](https://us.posthog.com/project/454383/insights/KAoqyT3E) — daily research card and concept card clicks
- [Lead Conversion Funnel](https://us.posthog.com/project/454383/insights/vpzZkczF) — conversion from hero CTA click → form submission
- [Contact Forms by Source Page](https://us.posthog.com/project/454383/insights/f3wW5XOo) — form submissions broken down by `form_location` (homepage, about, methodology, experiment pages)

## Verify before merging

- [ ] Open each edited page in a browser, submit the contact form, and confirm `contact_form_submitted` appears in [PostHog Live Events](https://us.posthog.com/project/454383/activity/explore) with the correct `form_location` property.
- [ ] Click the hero CTA, offer CTA, nav CTA, a research card, and a concept card on `index.html` and confirm the corresponding events fire in Live Events.
- [ ] Confirm `posthog.identify()` is called correctly on form submit — check that a Person record appears in PostHog with the submitted email as the distinct ID.
- [ ] Since this is a static HTML site with no build step, confirm the added `<script>` blocks have no JS syntax errors by loading the pages in a browser devtools console and checking for errors.
- [ ] The Formspree forms submit via a full-page POST, so PostHog relies on `sendBeacon` to flush events on unload. Test on a real network connection (not just localhost) to confirm events land in PostHog after form submission.

### Agent skill

We've left an agent skill folder in your project at `.claude/skills/integration-javascript_web/`. You can use this context for further agent development when using Claude Code. This will help ensure the model provides the most up-to-date approaches for integrating PostHog.
