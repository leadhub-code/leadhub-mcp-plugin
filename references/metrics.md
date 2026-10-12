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
