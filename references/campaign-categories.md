# Campaign categories

The emailing and SMS analytics tabs group campaign types like this.

Email:
- newsletters: targeted-emailing, targeted-emailing-ab
- automations: incremental-emailing
- technical: double_optin
- programmable: transactional_emailing

SMS:
- newsletters: targeted-sms
- automations: incremental-sms
- programmable: transactional_sms

Pass those type strings as `campaign_types`. Pass a group to `*_campaign_type_statistics` as
`campaign_type_groups`, for example {"newsletters": ["targeted-emailing", "targeted-emailing-ab"]}.
Null `campaign_types` means every type of that channel.

`targeted_emailing_campaign_stats`, `incremental_emailing_campaign_stats`, and `get_campaign_statistics`
are not these tabs. They load a campaign's own stats (lifetime for a targeted send) from another
service, filtered by when the campaign was scheduled. Use the `email_*` and `sms_*` tools to answer
"what did email do in this period".
