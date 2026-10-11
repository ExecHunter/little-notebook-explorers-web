# Explore Near You — LNE implementation foundation

Status: planned implementation, not deployed. One brand: Little Notebook Explorers. Do not revive LocalBabyClasses.

## Product
Parent-facing discovery of nearby LNE adventures and third-party children's activities. Keep children's adventure gameplay separate from business directory and parent enquiries. Preserve existing LNE pages and branding.

## MVP experience
- Entry point on the LNE website: **Explore Near You**.
- Search by postcode or town; configurable radius, default 10 miles, with location lookup only after explicit consent.
- Filters: age bands, category, free/paid, day, distance; results include LNE adventures and local classes with distinct types.
- Categories: LNE adventures; football/sport; dance/music; arts/messy play; baby/toddler; swimming; theatre/drama; outdoor activities.
- SEO-friendly canonical pages for towns, categories, and business profiles. No invented or placeholder businesses presented as real.
- Adventure page: "More family fun nearby". Class profile: "LNE adventures near this class".
- External booking links, not on-site class booking initially.
- Clear labels for sponsored placement and for independently verified/unverified listings.

## Data model (proposed)
`activity_providers`: id, name, description, website, email_private, phone_public, claim_status, owner_user_id, created_at, updated_at.
`activity_listings`: id, provider_id, slug, title, category, min_age_months, max_age_months, description, postcode, town, latitude, longitude, address_public, schedule_text, price_text, booking_url, listing_status, last_verified_at, created_at, updated_at.
`listing_media`: id, listing_id, media_type, storage_path, alt_text, moderation_status.
`listing_claims`: id, listing_id, claimant_user_id, status, evidence_reference, reviewed_at.
`listing_plans`: provider_id, tier (free/plus/featured), billing_status, stripe_customer_id, stripe_subscription_id.
`discovery_events`: event_type, anonymous_session_id, listing_id, adventure_id, search_area_coarse, timestamp; do not log children's precise location.

## Security, quality and moderation
- Parent-managed search; no child accounts required for browsing; UK Children's Code and GDPR by design.
- Avoid storing children's names or exact location. Do not expose provider private emails.
- Business owners may edit only claimed listings. Admin moderation before publication of claims, reviews, uploaded photos, or major changes.
- Use existing backend/auth infrastructure where available; do not provision a parallel database without inspection.
- Deduplicate imported providers; allow correction and removal; respect source terms and rights to photos/testimonials.
- Postcode lookup geocodes server-side with rate limits and caching; avoid disclosing search postcodes in analytics.
- Accurate map/distance results and accessible mobile layouts.

## Rollout
1. Inspect existing website/app structure, shared brand components and data layer.
2. Build navigation, discovery landing page, filters, listing cards, empty states and links to adventures.
3. Add secure admin CRUD and curated listings in Surrey/Hampshire; no fictional live listings.
4. Add provider claim workflow and business self-service editing.
5. Measure searches, listing views, outbound enquiry clicks, and cross-discovery into adventures.
6. Only after traction, test affordable enhanced listings and clearly labelled sponsored placement. Suggested £2.99/mo or £29.99/year Plus, £4.99/mo Featured are hypotheses, not locked prices.

## Release guardrails
- Work in feature branch, test responsive layouts and existing LNE navigation.
- No automatic deployment or production database migration before review.
- No Stripe billing changes during the first discovery MVP.
