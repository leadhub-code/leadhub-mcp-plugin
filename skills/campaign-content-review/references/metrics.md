# Analytics metrics

Money returned by the analytics tools is in the business primary currency from `list_businesses`.
It is not in cents. `get_campaign_statistics` is the exception: its `*_revenue_cents` fields are cents,
and it is a different dataset (see campaign categories).

Every rate the analytics tools return is percent from 0 to 100. 12.5 is 12.5%.
That covers the `email_*` and `sms_*` tools, the campaign stats tools (`targeted_emailing_campaign_stats` and
the like), and `overall_return_rate_pct`. `revenue_share` is the same kind of percent: attributed revenue
divided by all order revenue, times 100.

Engagement is not the same act on both channels. On email, engagement is an open. On SMS it is a click.
`engagement_rate` follows that. Email also has `open_rate`. SMS does not.

`attributed_revenue` is revenue the attribution model assigns to the channel. `orders_revenues` from
`order_statistics` is all order revenue, whether or not a message was involved.

`attributed_revenue_per_thousand_sent` is attributed revenue divided by sends, times 1000.
The send count comes from message statistics, not from the attribution call.

Subscriber `net change` over one period is `subscribers_count` minus `subscribers_count_before`.
Over a range it is the last period's size minus the first period's `subscribers_count_before`.
`subscribed_count` and `unsubscribed_count` are people who joined and left during the period.
Unique counts across several lists are not the sum of the lists.

PII of contacts is separate from these figures. It is pseudonymized by default. A call gets the real
values with `pseudonymize_pii` set to false, when the task needs them. A business can fix the choice with
`mcp_pii_pseudonymization`: always (pseudonyms in every call) or never (real values in every call); the argument is then
ignored. A pseudonym such as
`pii:email:01234567:NqkeYo6GN56EzfBUkO-JKd36_g76nQ6tEUAhjp1VYM6D~` replaces each text value of contact search conditions in a segment
(`email_addresses`, `phones`, `firstname`, `lastname`, addresses, `fulltext`), and the value of
each query parameter other than utm_* and the fragment on clicked links. The same value has the
same pseudonym in every call for the business, so pseudonyms can be counted and compared.
Only Leadhub can turn a pseudonym back into the value; do not try to decode or construct one.
Counts, rates, campaign names, campaign ids, and the sender of a campaign stay either way.
