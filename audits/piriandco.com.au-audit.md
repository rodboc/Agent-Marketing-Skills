# piriandco.com.au audit

Audited 18 October 2026. Method: crawled all 30 URLs in the sitemap (homepage, 3 products, 1 collection, 6 pages, 2 blog indexes and 17 articles), then parsed titles, meta, headings, JSON-LD, internal links, robots.txt, llms.txt and the policy pages. I could not query AI assistants, Search Console, Google Business Profile or third-party directories from this environment, so anything that needs those is marked "to verify".

Site profile: Shopify store (theme with Judge.me reviews), Sydney based studio selling custom illustrated pet portraits from a customer photo. Products: 1, 2 and 3 pet portraits as digital file, unframed print or framed print. Free delivery in Australia only.

## Scorecard

| Area | Status | Summary |
|---|---|---|
| Schema (full site) | Strong | Complete entity graph on every page. A few consistency fixes below. |
| Site architecture | Good, with gaps | Flat and clean. Seasonal page and two high-intent guides are under-linked. Thin hub pages. |
| AI SEO | Good foundation | Crawlable, answer-first content, FAQ blocks, agent-ready. Gaps in off-site presence and price consistency. |
| Directory submissions | Not started (to verify) | No sign of listings. Most of the skill's list is SaaS and does not apply. Local, review and gift channels do. |
| ASO | Not applicable | No mobile app found. |

---

## 1. Schema, full site

### What is already right
- Every page carries a single `@graph` with stable `@id`s: `OnlineStore` (#organization), `WebSite`, `WebPage`, `ImageObject`, `BreadcrumbList`, linked by reference rather than repeated.
- Organization is rich: legal name, ABN (`taxID`, `iso6523Code`), address, contact point, founders as linked `Person` nodes, `knowsAbout`, `areaServed`, `sameAs`, `hasMerchantReturnPolicy`, `hasShippingService`.
- Product pages use `ProductGroup` with 64 `hasVariant` `Product`s, per-variant `Offer`, `OfferShippingDetails`, SKU, size, colour, material, `AggregateRating` and `Review`.
- Articles use `BlogPosting` with author `Person`, `dateModified`, and linked `Blog`. Collection uses `CollectionPage` plus `ItemList`. Contact and About use `ContactPage` and `AboutPage`.
- All 30 pages: valid JSON, canonical present and self-referencing, OG and Twitter tags present, `lang="en"`.

### Fixes, in priority order

1. **Price drift between schema and copy (high).** The product pages show a sale: digital file "Was $29, now $18.90" (two pets $23.90, three $28.90). The schema correctly says $18.90. But all 11 gift guides, the Christmas page and the guides index quote the old prices ($29, $48, $67). Search and AI answers can quote either. Fix the copy to match, or if the sale is permanent, update the copy and drop the "Was" price. If the sale is temporary, add `priceValidUntil` to the offers and keep the was-price as a `priceSpecification` with `priceType` `https://schema.org/ListPrice`.
2. **Return policy schema vs actual policy (medium, to verify).** Schema says `MerchantReturnNotPermitted` with the label "Made to order, approved before printing". The refund policy actually offers a full refund before approval, and replacement or refund for damaged or faulty prints within roughly 30 days, plus Australian Consumer Law rights. Google Merchant surfaces may read "no returns". Consider `MerchantReturnFiniteReturnWindow` for the damaged/faulty case, or confirm that "not permitted" is deliberate. Keep schema and policy page saying the same thing.
3. **Review markup is partial (medium).** `reviewCount` is 10 on the one-pet product but only 5 `Review` objects are embedded. That is allowed, but check the rating and count match what Judge.me shows on the page, and that every review in markup is visible on the page. The two-pet and three-pet products have 3 and 1 reviews: fine, but the aggregate must stay honest as it grows.
4. **sameAs is thin (medium).** Only Instagram and Facebook. Add every real profile: Pinterest, TikTok, YouTube, Google Business Profile, ProductReview.com.au, Trustpilot, LinkedIn for the company, founder LinkedIn URLs on the `Person` nodes.
5. **Person nodes lack proof (low).** Perry Thomas and Rod Acevedo have name and job title only. Add `sameAs` (LinkedIn) and a short `description`, and make the byline visible on articles.
6. **Payload size (low).** The one-pet product page ships about 170 KB of JSON-LD (64 variants with full shipping blocks repeated per offer). Product HTML is 1.1 MB uncompressed (119 KB gzipped). Not a ranking problem, but you could reference shared `OfferShippingDetails` by `@id`, as you already do for returns, to cut the payload.
7. **Homepage name mismatch (trivial).** `<title>` says "Piri & Co"; `WebPage.name` says "Piri and Co". Pick one.
8. **FAQPage everywhere (informational).** Google no longer shows FAQ rich results (stopped May 2026), so this markup will not earn a SERP feature. It still describes the visible Q&A semantically and helps parsers. Keep it, but do not expect a rich result, and do not add more FAQ markup just for Google.

Nothing to add: no `HowTo` needed (retired), no sitelinks search box needed (retired). Worth adding if you produce them: `VideoObject` for any process video, and `ImageObject` licence details if you want the portraits to surface in image search with a licence badge.

---

## 2. Site architecture

### Current structure (from sitemap and nav)

```
Homepage (/)
├── Shop (/collections/all-portraits)              1 collection only
│   ├── Personalised pet portrait (/products/personalised-pet-portrait)
│   ├── Two pet portrait (/products/two-pet-portrait)
│   └── Three pet portrait (/products/three-pet-portrait)
├── How it works (/pages/custom-pet-portraits-australia)
├── FAQs (/pages/faqs)
├── Contact (/pages/contact)
├── Our Story (/pages/our-story)                  footer only
├── Your photos info (/pages/how-we-handle-your-photos)   footer only
├── Christmas order dates (/pages/christmas-order-dates)  not in nav or footer
├── Gift Guide (/blogs/guides)                    footer only, 11 articles
└── Tips (/blogs/tips)                            not in nav or footer, 5 articles
```

Depth is 2 clicks to everything, which suits a 30 page store. Header is three text links plus the product shortcuts. No dropdowns, no breadcrumbs visible in the header zone beyond the schema.

### Findings

1. **The seasonal money page is nearly orphaned (high).** `/pages/christmas-order-dates` has 1 inbound internal link (from one gift guide) and appears in neither the header nor the footer. It is the page that answers "when do I need to order for Christmas", and the cut-off is 4 December. Link it from an announcement bar, the header (as a seasonal item from October to December), the homepage, the product pages, and the footer.
2. **Highest intent guides are the least linked (high).** The cat lovers and dog lovers guides get 2 to 3 inbound links each. The other cluster guides get about 12. Partner, and "gift for someone who has everything" also sit at 3. These are the big head terms in the cluster. Add them to a "related gifts" module and cross-link from the sibling guides.
3. **Tips index is not linked from site-wide navigation (medium).** `/blogs/tips` only gets links from articles. Add "Guides" and "Tips" under one header item, "Gift guides and tips", or fold them into a single Journal.
4. **Hub pages are thin (medium).** Collection page 99 words, blogs index 191 words, contact 51, guides index 353. The collection page is the commercial hub and has almost no copy for search or AI to quote. Add 150 to 250 words of intro: what a portrait is, what is included, price range, turnaround, delivery.
5. **Only one collection (medium).** `/collections/all-portraits` is the sole collection. Opportunities that map to real search intent and the existing guides: Dog portraits, Cat portraits, Memorial portraits, Digital portraits, Framed portraits, Gifts under $100. Each can be a real collection with its own intro, using the existing products.
6. **Homepage and pillar compete for the same term (medium).** Homepage title: "Custom Pet Portraits Australia by Piri & Co". The pillar page URL is `/pages/custom-pet-portraits-australia` and its title is "Custom Pet Portrait from a Photo: Cost and How It Works". Decide which one targets "custom pet portraits Australia". Suggest: homepage = brand plus category ("Custom Pet Portraits from Your Photo | Piri & Co"), pillar = cost, timing, process. Check Search Console for cannibalisation before and after.
7. **Anchor naming mismatch (low).** Nav says "How it works"; URL and title say "custom pet portraits australia ... cost". Consider renaming the nav item "Prices and how it works", since price is the thing people look for.
8. **Titles over 60 characters (low).** 15 pages (11 articles, the pillar, 3 products). Worst: Memorial guide at 77, two-pets article at 71. They will truncate. Trim to 55 to 60, brand last.
9. **Image alt text (low).** 3 to 6 images per page have no alt (likely icons and cart elements). Check the product and article images specifically.
10. **Duplicate-ish URLs are handled.** Variant URLs use `?variant=`, canonicals are self-referencing, robots.txt blocks sort and filter traps. No action.

### Recommended additions (new pages)
- Price page or price table on the pillar: one row per product and format (already half there as "At a glance").
- Occasion pages: Mother's Day, Father's Day, Christmas, birthday, adoption day, memorial (exists as a guide).
- Gallery of finished portraits with breed names (supports image search and gives AI engines concrete examples).
- Comparison page: custom pet portrait from a photo vs commissioned artist vs generic personalised gifts (answers a real question, and gives you a page to be cited).
- Reviews page, fed by Judge.me, with the same reviews in markup.

Suggested header: Shop | Gift guides | How it works and prices | Christmas order dates (seasonal) | FAQs. CTA button: Start with a photo.

---

## 3. AI SEO

### Passing
- **Access:** robots.txt allows all user agents, no AI crawler blocks, nothing blocking `OAI-SearchBot`, `PerplexityBot`, `Claude-SearchBot`, `GPTBot`, `ClaudeBot` or `Google-Extended`. A custom user agent fetched every page with HTTP 200. (WAF and Cloudflare bot rules cannot be seen from here; check them in the Shopify and Cloudflare dashboards if you have custom rules.)
- **Agent readiness:** `/llms.txt`, `/agents.md`, `/.well-known/ucp` and the UCP/MCP endpoint exist (Shopify generates these). Product, price, variant, stock and shipping data are all server rendered and in structured data.
- **Extractable content:** guides run 770 to 1,180 words with about 8 to 10 H2s, "Key takeaways" blocks, budget tiers, and FAQs. The pillar page opens with a direct answer and an "At a glance" block (price, turnaround, delivery). Articles carry dates and named authors.
- **Entity basics:** consistent legal name, ABN, address and founders in markup.

### Gaps
1. **Price inconsistency (high).** See schema item 1. Conflicting prices across pages is the most likely cause of an assistant stating the wrong price.
2. **llms.txt contains no brand facts (medium).** The Shopify generated file is shopping-agent instructions, not a description of Piri & Co. Shopify controls this file, so rather than rely on it, put the facts on a normal page that AI search can quote: one "About Piri & Co / key facts" block on Our Story or the pillar (what it is, who makes it, where, price range, turnaround, delivery area, refund rules, how photos are handled). You already have most of it.
3. **Thin off-site presence (high, to verify).** Public web searches for the domain and brand returned nothing from or about the site. AI engines lean on third parties for "best X" answers. Priorities: Google reviews, ProductReview.com.au, Pinterest, Reddit participation in pet and gift threads, Australian gift guide and pet blog features, Instagram content that names the product consistently.
4. **Name ambiguity (medium).** "Piri" is often read as "Pirie" (a place and surname) in search. Keep "Piri & Co pet portraits" together in titles, alt text, and off-site bios so the entity resolves.
5. **No comparison or "vs" content (medium).** Prompts like "custom pet portrait vs commission an artist", "best personalised pet gift Australia", "is a digital pet portrait worth it" have no dedicated answer on the site.
6. **Freshness cues (low).** Articles have `dateModified` in markup, but check that a visible "Updated" date shows, and refresh the Christmas page and gift guides each season.
7. **Reviews live in a widget (low).** Confirm Judge.me output is in the HTML delivered to crawlers (the markup has 5 of 10 reviews, which suggests it is).

### Prompt set to run (I could not run these here)
Run each in ChatGPT, Perplexity, Gemini and Google AI Mode, three times, and log whether Piri & Co is cited or recommended:
1. custom pet portrait from a photo Australia
2. how much does a custom pet portrait cost in Australia
3. best personalised gift for a dog lover Australia
4. gift for someone who lost their pet Australia
5. can a pet portrait be made from a blurry photo
6. how long does a custom pet portrait take
7. last minute pet gift Australia digital
8. when to order a pet portrait for Christmas Australia
9. Piri & Co pet portraits review
10. is Piri & Co legit
11. pet portrait for two pets from different photos
12. best framed pet portrait Australia

Prompts 9 and 10 test your brand entity directly; wrong or missing answers there point at the pages to fix first.

---

## 4. Directory submissions

The `directory-submissions` skill is built for software launches (Product Hunt, AI tool and MCP directories, G2, Capterra). Almost none of that fits a Sydney DTC gift store. Applying its three rules:

- **Rule 1, foundation first:** met. Clear entity, address, ABN, policies, schema.
- **Rule 2, destination pages first:** mostly met (pillar, 11 gift guides, product pages). Improve hub copy before building links (architecture item 4).
- **Rule 3, positioning varies by directory:** use the three positioning lines at the end of this section.

### Skip (not a fit)
Product Hunt, BetaList, AI tool directories (TAAFT, Futurepedia), MCP and agent registries, G2, Capterra, SaaS and no-code directories, developer platforms (GitHub, GitLab, Replit).

### Do, in order

**Tier A, trust and local (do first):**
1. Google Business Profile. **Caution:** the address on the site is 383 George Street, Sydney, which I recognise as a shared workspace address. Google restricts profiles for coworking and virtual offices, and for businesses with no customer-facing premises. Check Google's eligibility rules before applying. If it is not eligible, skip it, and do not use a different address that is not real.
2. Bing Places and Apple Business Connect (same address caution).
3. ABN Lookup: confirm "Piri & Co" is registered as a business name for R&P Co Pty Ltd (ABR/ASIC).
4. Google Merchant Center free listings. The product, price, shipping and returns markup already supports it; this is probably the highest value item on the list.

**Tier B, reviews and reputation:**
5. ProductReview.com.au and Trustpilot, then link them in `sameAs`.
6. Google reviews via Merchant Center or Business Profile if eligible.
7. Judge.me can syndicate to Google Shopping; enable it if the plan allows.

**Tier C, discovery and backlinks that fit the product:**
8. Pinterest business profile with rich pins (product markup supports this). Strong for gift intent.
9. Etsy shop or similar marketplace listing only if you want a second sales channel; it is also a high authority backlink. Make sure the listing matches your policies.
10. Instagram and Facebook shops (already have profiles).
11. Australian local and general directories from the skill's Tier 9 that still accept free listings: Hotfrog, Cylex, Brownbook. Low value, low risk, skip any that charge or look spammy. Add Yellow Pages and True Local only if they are free and verified.
12. Behance or similar for the illustration process, only if you are willing to post real work.

**Tier D, editorial (higher value than any directory):**
13. Outreach for "best personalised pet gifts Australia" roundups, pet blogs, and gift guides. Pitch the memorial and last-minute digital angles.
14. Australian Made licensing: check whether printing in Australia qualifies under their rules before using the logo.

Do not buy bulk "directory submission" packages or submit to article and press-release spam directories (skill Tier 11 and 12). The gain is negligible and the risk is real.

### Positioning lines per directory type
- Local and general: "Piri & Co is a Sydney studio that turns a photo of your dog or cat into an illustrated portrait. Printed in Australia, free delivery."
- Review sites: "Custom illustrated pet portraits from your photo. You approve the portrait before anything is printed."
- Gift channels (Pinterest, roundups): "A personalised gift for pet lovers: digital portraits emailed within hours, framed prints delivered across Australia."

Tracker: use `.agents/skills/directory-submissions/references/submission-tracker-template.csv`. All statuses are currently "unknown"; I could not check existing listings.

---

## 5. ASO

Not applicable. No iOS or Android app, store links, or app deep links appear on any page, in markup, or in the footer. The Shop app and Shop Pay are Shopify features and are not listings you optimise. If an app is planned, run this audit once a store URL exists.

---

## Priority list

**This week**
1. Make prices consistent across schema, product pages, guides and the Christmas page (and set `priceValidUntil` if the sale is temporary).
2. Link the Christmas order dates page from the header or announcement bar, homepage and product pages. The 4 December cut-off is close.
3. Add "Gift guide" and "Tips" to site navigation, and link the cat lovers, dog lovers and partner guides from every sibling guide.
4. Resolve the return policy schema vs refund policy wording.

**This month**
5. Write intro copy for the collection page and the two blog indexes.
6. Trim 15 titles to 60 characters or less. Decide homepage vs pillar targeting.
7. Expand `sameAs`; add LinkedIn to the founder `Person` nodes.
8. Add a "key facts" block, then set up Merchant Center, Bing Places, ProductReview.com.au, Pinterest.
9. Run the 12 prompt AI visibility test and record a baseline.

**Next quarter**
10. Split into Dog, Cat, Memorial, Digital and Framed collections.
11. Publish a comparison page and a finished portrait gallery.
12. Earn a handful of editorial gift guide features.

Method note: counts and line items above come from the crawl on 18 October 2026. Re-run after changes to confirm.
