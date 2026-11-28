# Popup display rules

`rules` in popup_template_data say on which pages and when a popup shows. They are about the visit, not about who
the visitor is. Who sees it (segment and interest lists) is set on the campaign.

```json
{"operator": "AND", "query": [
  {"operator": "AND", "conditions": [{"type": "URL_CONTAINS", "payload": {"value": "/product/"}},
                                    {"type": "TIME_ON_PAGE", "payload": {"seconds": 15}}]},
  {"operator": "OR", "conditions": [{"type": "EXIT_INTENT", "payload": {}}]}
]}
```

The conditions of a group combine with the group `operator`; the groups combine with the top `operator`.
`"query": []` shows the popup on every page as soon as the visitor arrives.

## Rule types

| type | payload | shows when |
| --- | --- | --- |
| `URL_CONTAINS` | `{"value": "<text>"}` | the current URL contains the value |
| `URL_NOT_CONTAINS` | `{"value": "<text>"}` | the current URL does not contain the value |
| `URL_MATCH` | `{"value": "<text>"}` | the current URL is the value |
| `URL_NOT_MATCH` | `{"value": "<text>"}` | the current URL is not the value |
| `URL_MATCH_REGEX` | `{"value": "<text>"}` | the current URL matches the regular expression |
| `URL_NOT_MATCH_REGEX` | `{"value": "<text>"}` | the current URL does not match the regular expression |
| `URL_CONTAINED` | `{"value": "<text>", "hours": <number, optional>}` | the visitor visited a URL containing the value within the last `hours` (any time without it) |
| `URL_NOT_CONTAINED` | `{"value": "<text>", "hours": <number, optional>}` | the visitor did not visit a URL containing the value within the last `hours` (any time without it) |
| `PAGEVIEW_COUNT_SESSION` | `{"count": <integer>}` | the visitor has seen at least this many pages in this visit |
| `TIME_ON_SITE_SESSION` | `{"seconds": <number>}` | the visit lasts at least this long |
| `TIME_ON_PAGE` | `{"seconds": <number>}` | the visitor is on the current page at least this long |
| `EXIT_INTENT` | `{}` | the visitor moves to leave the page |
| `CART_VALUE` | `{"operator": <eq, neq, gt, gte, lt, lte>, "value": <number>}` | the cart value, in the shop currency |
| `DISPLAY_WIDTH` | `{"operator": <eq, neq, gt, gte, lt, lte, btw>, "value": <pixels>}` | screen width; `lt` 768 = phones, `btw` takes `[min, max]` |
| `GLOBAL_VARIABLE` | `{"path": "<window.path>", "operator": <def, undef, eq, neq>, "value": <eq, neq>}` | a page variable |

`URL_MATCH` compares the whole URL, `URL_MATCH_REGEX` takes a regular expression. One condition holds one value;
for several URLs add several conditions to an OR group.
