# What a business can set up in Leadhub

This page describes what the Leadhub web app can do, so a proposal is not limited to what the analytics tools
measure. Combine it with what the data of the business shows and with your own ideas.

Some proposals can be made with the MCP write tools: audiences, popups, RFM profiles, and the email of a
campaign with `edit_campaign_email` when the user asks for it. The user sets the rest up in the Leadhub web app
at `https://app.leadhub.co/businesses/<business_id>/<page>`.
Whoever sets it up keeps campaigns, popups, and audience syncs as drafts or paused, and launches, schedules, sends,
or syncs only after the user confirms that step: a launched popup is shown to the website's visitors, a launched
campaign emails or texts real customers, and a sync sends the audience to the ad platform.
Pages below are given as `<page>`. New campaigns and ready-made templates start from "+ Create new" on `dashboard`.
A proposal the user can act on usually says what to set up and where, who it is for (segment conditions
as rule JSON from segmentation://conditions), and how to tell whether it worked.

Some features are turned on per business. `list_businesses` returns them in `features`. When one is missing:
- `sms`: the user turns SMS on in the web app by finishing the SMS onboarding (`sms-onboarding/campaigns`).
- `programmable_automation`, `programmable_automation_sms`: beta features. Leadhub support may let the business
  into the beta.
- `popup_custom_html`, `facebook_ads`: Leadhub support turns them on.

What the tools cannot see: `list_campaigns` returns email, SMS, and popup campaigns only. A
`programmable-automation` flow, a `targeted-ad` campaign, audience syncs, and connected ad accounts are not
visible. Before proposing one of those as missing, ask the user whether it already runs.

## Email

- Newsletter `targeted-emailing`: one send to a segment and interest lists, now or scheduled.
- A/B newsletter `targeted-emailing-ab`: the A/B switch on a newsletter. Variants A and B differ in subject,
  sender, or content. Leadhub sends them to a sample, and after a set number of hours sends the winner to the rest,
  picked by open rate, click rate, order count, order value, or lowest unsubscribe rate.
- Facebook and Instagram promotion of a newsletter: on the newsletter, a Facebook post can be promoted to its
  recipients who did not open, opened, opened without a click, or clicked, with a budget and duration.
  Needs a Facebook ad account, page, and pixel connected in `settings/facebook-settings`.
- Automation `incremental-emailing`: one email to each contact when they enter a segment. The segment is
  re-evaluated continuously; "send once per" limits repeats. A series is several automations with different
  date conditions, or a `programmable-automation`.
- Ready-made templates in "+ Create new". Each creates a campaign group with its emails, segments, and for some
  a popup, which the user then edits. The business sees the templates available in its language.
  - `TXT_WELCOME`: welcome email to new subscribers.
  - `TXT_FIRST_ORDER`: discount on the first purchase. A popup collects the email and an automation sends the code.
  - `TXT_CONTESTV`: voucher competition. A popup turns visitors into subscribers.
  - `TXT_ABANDONED_CART`: cart left without an order; the email shows the cart products and recommendations.
  - `TXT_VIEWED_PRODUCTS`: products viewed repeatedly without a purchase.
  - `TXT_RUNNING_OUT_PRODUCTS`: replenishment when a bought consumable is about to run out.
  - `TXT_RETENTION`: thank-you with a discount code after a purchase (30 days and 1 year after), toward the next order.
  - `TXT_NAME_DAY`: name day greeting.
  - `TXT_NEWSLETTER`, `TXT_SEASONAL_NEWSLETTER`: newsletter starters (promotions; Black Friday, Christmas).
- Double opt-in email (`double_optin`), set in `settings/double-opt-in`.
- Email editor: text, image, button, columns, divider, spacer, menu, HTML, YouTube video image, countdown timer,
  product boxes from the feed, and dynamic product blocks: cart products, last order, viewed products,
  running-out products, automation products, recommended products. A block can show only to a segment.
  Personalization: greeting by first or last name in the vocative, gender-specific text, a tag value, a discount code.
  Reusable blocks and design presets live in `library`.
- Discount codes (`settings/discount-codes`): a source is an imported stack of codes, a URL generator, or the
  Shoptet generator. Email and SMS insert one code per contact. A stack can run out.
- Campaign settings: schedule, automated UTM tagging, test mode for automations, a dependency on another campaign,
  senders (`settings/senders`), sending domain (`settings/domain`).
- Measure: `email_channel_statistics`, `email_campaign_performance`, `email_campaign_type_statistics`,
  `email_link_statistics`, `get_campaign_email_content`.

## SMS (feature `sms`)

- Newsletter `targeted-sms` and automation `incremental-sms`, the SMS counterparts of the email ones.
- Templates `SMS_FIRST_ORDER`, `SMS_RETENTION`, `SMS_NAME_DAY`, `SMS_NEWSLETTER` are offered in `sms-onboarding/campaigns`,
  not in "+ Create new". They often pair with the email version (for example an SMS to those who did not open).
- Senders in `settings/sms-senders`. SMS goes only to numbers subscribed to an SMS interest list.
  Numbers come from an SMS lead-generation popup, orders, and imports.
- Measure: `sms_channel_statistics`, `sms_campaign_performance`, `sms_campaign_type_statistics`.

## Programmable automation (feature `programmable_automation`)

`programmable-automation` is a flow builder for journeys a single segment automation cannot express.
Its email and SMS steps are campaigns of type `transactional_emailing` and `transactional_sms`
(the "programmable" category in analytics://campaign-categories).
- Triggers: API call, segment entry (scheduled or as contacts arrive), order state change
  (states mapped in `settings/order-states-mapping`), interest list subscription, popup submitted,
  name day, running-out products, viewed product, cart activity.
- Steps: send email, wait (a delay, or until a day and time window), condition, A/B split, add tag,
  set variable, webhook. Send SMS needs features `sms` and `programmable_automation_sms`.
  Subscribe to or unsubscribe from an interest list is turned on separately by Leadhub support.
- Use it for multi-step series: a welcome series, post-purchase follow-up after an order state change,
  abandoned cart email then SMS, VIP onboarding after an RFM segment change.

## Popups and web forms

`popup` campaigns show on the shop website, as an overlay popup or as a "weblayer" embedded in the page
(an inline web form).
- Objectives: lead generation (email), SMS lead generation (feature `sms`), deliver a message
  (announcement or discount code), custom HTML (feature `popup_custom_html`), and the weblayer versions of these.
  A privacy center teaser exists for businesses with the privacy dashboard.
- Form fields: email, phone, first name, last name, birthday, consent checkbox, button; an optional teaser tab.
  A new lead joins the interest lists set on the popup (email lists, SMS lists for phone numbers),
  which can start a welcome automation.
- Display rules: exit intent, time on site or on page, page views in the session, URL contains, matches, or
  matches a regex (and the negations), URLs visited earlier, cart value, screen width, a page variable.
  Plus segment targeting (for example hide from subscribers, another offer to returning customers),
  how soon a visitor sees it again, priority over other popups, auto close, and a start and end date.
- Web effects (`settings/web-effects`): a tab title on exit intent, falling snow on the site or a section.
- Measure: `popup_campaign_stats`, `subscriber_statistics`; segment conditions `viewed_popup_campaign`,
  `submitted_popup_campaign`, `closed_popup_campaign`.

## Ads and audiences

- An audience (`list_audiences`) can be synced as an ad audience: Meta (Facebook, Instagram), Google Ads
  (Google, YouTube, Gmail), or Sklik (Seznam.cz).
  Connect the account first in `settings/facebook-settings`, `settings/google-ads-settings`, or
  `settings/sklik-settings`; Google Ads also needs the consent settings there.
  Typical audiences: lookalike seed of best customers (RFM), retarget cart abandoners, exclude recent buyers,
  win back lapsed customers.
- Facebook Lead Ads: leads from a Facebook lead form are subscribed to an interest list (`settings/facebook-lead-ads`).
- Facebook ad campaign `targeted-ad` (feature `facebook_ads`), with an objective: find new customers (lookalike or
  interest audiences), collect leads with lead ads, bring existing customers back (custom audiences), or
  remarket to web visitors (dynamic product ads or pixel audiences).

## Contacts and data

- Segments (`audiences`): saved conditions reused by campaigns, popups, and audience syncs. See segmentation://conditions.
- Interest lists (`settings/interest-lists`): email or SMS subscription lists; campaigns send only to their subscribers.
- RFM profiles and segments (`analytics/rfm-analysis`): see analytics://rfm. Usable as segment conditions.
- Custom tags and events sent by the shop or the API: segment condition `custom_tags`.
- Contact import (`settings/import-contacts`), export (`export`), product feeds (`settings/feeds`),
  tracking pixel (`settings/pixel`), shop integrations (Shoptet, Shopify, Shoper, Eshop-rychle), and import of
  historical orders from many shop platforms or a file.

## Analytics pages

`analytics/orders`, `retention`, `products`, `web-traffic`, `emailing`, `sms`, `subscribers`, `rfm-analysis`.
The MCP analytics tools return the same data; use them to find the gap a proposal closes.

## Examples of gaps

Patterns that often point to an opportunity. They are examples, not a checklist: look at what this
business's data shows, and propose what fits it, also when it is not listed here.

- No running `incremental-emailing` for abandoned cart, viewed products, welcome, or first-purchase discount
  (`list_campaigns`, `get_campaign_targeting`): propose the template. Ask first whether a programmable automation covers it.
- Few new subscribers (`subscriber_statistics`) or no active popup: a lead-generation popup with an incentive
  (`TXT_FIRST_ORDER`, `TXT_CONTESTV`).
- Low repeat purchase (`repeat_purchase_cycle`, `first_purchase_cohorts`): retention and running-out automations.
- Many at-risk or churned customers (`get_rfm_analysis`): win-back email or SMS to that RFM segment, ad audience.
- High traffic without purchase (`session_statistics`, `journey_stats`): browse and cart abandonment,
  exit-intent popup, retargeting audience.
- Newsletters with a low open rate: an A/B subject test, an SMS or Facebook promotion to those who did not open.
- Email-only business with the `sms` feature: SMS for time-critical sends (cart, sale end).
