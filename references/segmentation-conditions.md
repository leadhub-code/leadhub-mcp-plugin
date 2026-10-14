# Segmentation conditions

A contact segment is the tree returned as `segment_query` by `get_campaign_targeting` and `get_segment`.
It is the same tree the Leadhub segment designer edits.

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
  Empty `segments` (or groups without blocks) means no restriction.
- `targetingBlocks`: blocks inside a group, joined with AND. `key` is the category (below).
- `values`: rules inside a block. Between two rules sits `{"key": "operator", "value": "and" | "or"}`.
- A rule's `key` is the condition. `set: false` marks a rule the user has not finished configuring.
- Conditions starting with `not_` are the negation of the same condition without the prefix.

Campaigns also list `interest_lists`. The contact must match the segment AND be subscribed to at least
one of those lists. The `in_interest_list` condition below is a separate, explicit rule.

## Common parameters

- `date_rules`: when the event happened. Empty = anytime.
  - Relative: `{"op": "lt", "value": {"value": 10, "unit": "hour"}}` = less than 10 hours ago (recently).
    `"op": "gt"` = more than N units ago. Units: minute, hour, day, month, year.
  - Absolute: `{"op": "gt", "value": {"absolute_value": 1740783600}}` = since that unix time (seconds);
    `"op": "lt"` = before it.
  - Between: two rules, `gt` and `lt`.
  - "Never" for activity dates is the `not_` condition without `date_rules`, e.g. `not_last_contact_date`.
  - Older rules may instead carry `operator`, `value`, `unit` directly on the rule; read them as one date rule.
- `value_rules`: numeric comparison, `[{"op": "gt", "value": 500}]`. Ops: lt, lte, eq, gte, gt.
  Between is a `gt` plus an `lt` rule. Prices are in the business primary currency. Empty = any value.
- `all_any`: "any" = at least one of the listed items, "all" = every listed item.
- `product_ids`, `categories`, `brands`: product ids from the product tools, category paths, brand names.
- `is_first_order`: on purchase conditions, only count the contact's first order.
- `campaign_id`: id from `list_campaigns`. `interest_list_id`: id from `interest_lists`.

## Categories and conditions

### knowledge_of_the_contact
- `known` / `unknown`: Leadhub knows the contact's email, name, surname, or phone, or the contact is anonymous.
- `known_for`: how long the contact has been known (first subscription, first order, or first identification), `date_rules`.
- `in_interest_list` / `not_in_interest_list`: `interest_list_id`; `in_interest_list` may have `date_rules`
  (subscribed within that time, used as the entry condition of welcome automations).
- `has_gender`: `gender` ("female" / "male"). Only for cs, sk, hu, ua, pl businesses.
- `has_nameday`: name day today. Only for cs, sk, hu, ua, ro, pl businesses.
- `geo_segmentation`: `areas[].points[]` of `{gps_lat, gps_lng, radius_km, type: "include" | "exclude"}`, `areas[].countries[]`.

### customer
- `not_a_customer`, `customer`, `returning_customer`: no order, at least one order, at least two orders.
- `first_purchase`, `last_purchase`: `date_rules`, optional `value_rules` on the order value.
- `average_purchase`, `orders_value_sum`: `value_rules` (money), optional `date_rules` for the order window.
- `orders_count_sum`: number of orders, `value_rules`, optional `date_rules`.
- `cancelled_orders_count_sum`: like `orders_count_sum`; Shoptet businesses only.
- `average_time_between_orders`: `value_rules` whose values are durations `{"value": 10, "unit": "day"}`.
- `rfm_segments`: `rfm_profile_id`, `rfm_segment_names` (see analytics://rfm and `list_rfm_profiles`).
- `rfm_segment_change`: moved between RFM segments. `rfm_profile_id`, `rfm_segment_from` / `rfm_segment_to`
  ("__any__" or a name), `rfm_segment_from_names`, `rfm_segment_to_names`, within `time_value` `time_unit`.

### behavior
Activity:
- `viewed_pageview`: visited URLs. `value` (list of URL patterns), `all_any`, `date_rules`.
- `last_contact_date`, `last_email_activity_date`, `last_web_activity_date`: last activity of any kind
  (order, web, or email), in email, on the web. `date_rules`. `not_*` without `date_rules` = never.

Purchases (`date_rules`, `is_first_order`):
- `bought_any_products` / `not_bought_any_products`: `product_ids`, `all_any`.
- `bought_from_any_category` / `not_bought_from_any_category`: `categories`.
- `bought_from_any_brand` / `not_bought_from_any_brand`: `brands`.
- Older segments may use `bought_products`, `bought_from_categories`, `bought_from_brands` (and `not_*`); same meaning.

Viewed products (`date_rules`):
- `viewed_any_products` / `not_viewed_any_products`: `any: true` = any product; otherwise `product_ids`.
- `viewed_from_any_category` / `not_viewed_from_any_category`: `categories`.
- `viewed_from_any_brand` / `not_viewed_from_any_brand`: `brands`.
- Older segments may use `viewed_products` / `not_viewed_products` with `product_ids`.

Running out (predicted from purchase history and product consumption time):
- `running_out_products`, `running_out_products_from_categories` (`categories`),
  `running_out_products_from_brands` (`brands`), and their `not_*`.
  `min_remaining_days`, `max_remaining_days`, `consumption_priority` (order of sources: "preset", "user", "global").

Email campaigns (`campaign_id`):
- `received_campaign`, `opened_campaign`, `clicked_campaign`, and their `not_*`.

SMS campaigns (`campaign_id`, `date_rules`):
- `received_sms_campaign`, `clicked_sms_campaign`, and their `not_*`. Only where SMS is enabled.

Popup campaigns (`campaign_id`, `date_rules`):
- `viewed_popup_campaign`, `submitted_popup_campaign`, `closed_popup_campaign`, and their `not_*`.

### tags
Custom events and attributes sent to Leadhub.
- `custom_tags`: tagged with `tag_name`. `any_last` ("last" = test the latest tag of that name,
  "any" = any of them; with "any", `freq` = tagged at least that many times), `date_rules`.
  Optional value test: `tag_key`, `tag_operator`, `tag_value`. Operators: num_lt, num_lte, num_eq, num_ne,
  num_gte, num_gt, str_eq, str_ne, bool_eq, dt_* (dates), exists.
- `not_custom_tags`: never tagged with `tag_name` (within `date_rules` when set).
- System tags such as `lh_system_shoptet_customer_group`, `lh_system_shoptet_product_review`,
  `lh_system_shoptet_store_review` come from integrations and use the same fields.

### cart
- `active_cart_products` (`product_ids`, `all_any`), `active_cart_categories` (`categories`),
  `active_cart_brands` (`brands`), and their `not_*`.
- `active_cart_total_value`: `value_rules` (money).
- `active_cart_last_modified`: `date_rules`.

### search
Match contact fields by text. `value` is a list of strings.
- `fulltext`, `firstname`, `lastname`, `email_addresses`, `phones`, `shipping_addresses`,
  `shipping_addresses_street`, `shipping_addresses_city`, `shipping_addresses_country`,
  `shipping_addresses_country_code`, `shipping_addresses_zip`.

### random_distribution
- `random_distribution_abn`: splits contacts into random groups. `distribution` (fractions, e.g. [0.25, 0.25, 0.5]),
  `group_id` (index of the group this rule keeps), `salt` (same salt = same split).

## Not covered here

- Popup display rules (exit intent, time on page, URL) are configured on the popup, not in the segment.
- Programmable automations have their own triggers and flow conditions.
- These tools are read-only. Changes are made in Leadhub.
