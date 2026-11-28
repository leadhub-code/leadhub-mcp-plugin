# Segmentation conditions

A contact segment is the tree returned as `segment_query` by `get_campaign_targeting` and `get_audience`.
It is the same tree the Leadhub segment designer edits. `list_segment_conditions` lists the conditions
and parameters a business can use. `validate_segment_query` checks a tree before you propose it.
A `custom_tags` rule needs the exact tag name: `list_contact_tags` lists the tags the contacts have,
and with include_contacts_count how many contacts have each.

## Tree

```json
{"segments": [
  {"targetingBlocks": [
    {"key": "customer", "values": [
      {"key": "returning_customer", "set": true},
      {"key": "operator", "value": "or"},
      {"key": "orders_value_sum", "set": true,
       "value_rules": [{"op": "gt", "value": 500}],
       "date_rules": [{"op": "lt", "value": {"value": 30, "unit": "day"}}]}
    ]}
  ]}
]}
```

- `segments`: groups joined with OR. A contact matches if they match any group.
  Empty `segments`, a group without blocks, or a block with empty `values` means no restriction.
- `targetingBlocks`: blocks inside a group, joined with AND. `key` is the block of the conditions inside it.
- `values`: rules inside a block. Between two rules sits `{"key": "operator", "value": "and" | "or"}`.
  One block uses one operator.
- A rule's `key` is the condition. `set: false` marks a rule the user has not finished configuring; it is ignored.
- Conditions starting with `not_` are the negation of the same condition without the prefix.
  Never propose or save these `not_` forms, even when `list_segment_conditions` gives one as a negation_key:
  `not_known_for`, `not_first_purchase`, `not_last_purchase`, `not_average_purchase`, `not_orders_value_sum`,
  `not_orders_count_sum`, `not_cancelled_orders_count_sum`, `not_average_time_between_orders`,
  `not_active_cart_total_value`, `not_active_cart_last_modified`. The segment designer cannot show them, so a
  segment that has one does not open in Leadhub. Use the condition without `not_` with the opposite value or
  date rule where that means the same, or tell the user the negation is not available. Every other `not_` form,
  such as `not_viewed_any_products` or `not_custom_tags`, works in the designer.

Campaigns also list `interest_lists`. The contact must match the segment AND be subscribed to at least
one of those lists. The `in_interest_list` condition is a separate, explicit rule.

## Reading saved segments

Segments saved in audiences and campaigns can use names and rule shapes the designer no longer writes. Read them, but propose the current ones.

- Older condition names are still evaluated. `validate_segment_query` warns about each one and names the current condition.
- Older rules carry `operator`, `value`, and `unit` directly on the rule instead of `date_rules` / `value_rules`.
  How to read them depends on the condition:
  - A value condition (money or a count, such as `orders_value_sum`, `average_purchase`, `active_cart_total_value`):
    one value rule `{"op": operator, "value": value}`.
  - `average_time_between_orders`: one value rule whose value is the duration `{"value": value, "unit": unit}`.
  - A date condition: one date rule `{"op": operator, "value": {"value": value, "unit": unit}}`. `operator`
    "anytime" or "never" means no date rule. "interval" means between `from_date_s` and `to_date_s` (unix seconds).

## Anonymized calls

When the call pseudonymizes PII, each string in the `value` of a search condition (`fulltext`, `firstname`, `lastname`,
`email_addresses`, `phones`, `shipping_addresses*`) is a pseudonym (`pii:email:...~`, `pii:phone:...~`, `pii:name:...~`,
`pii:address:...~`, `pii:text:...~`). The condition `key` and the number of values stay.

## Not covered here

- Popup display rules (exit intent, time on page, URL) are configured on the popup, not in the segment.
- Programmable automations have their own triggers and flow conditions.
- create_audience and update_audience save a segment as an audience. Ad syncs of an audience are set in Leadhub.
