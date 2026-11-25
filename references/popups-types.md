# Popup types and popup_template_data

A popup campaign shows a window (popup) or a block inside the page (weblayer, used for web forms) to visitors of
the shop. `get_popup_content` returns its `popup_template_data`. A save replaces it whole.

`get_popup_definitions` lists every value the display script accepts (objectives, layouts, element, action, rule,
and validation types). This text explains what they mean.

## objective

What the popup is for:
- `LEAD_GENERATION`, `SMS_LEAD_GENERATION`: collect an email or a phone number and subscribe it to interest lists.
- `DELIVER_MESSAGE`: a message, an offer, or a link, without a form.
- `CUSTOM_HTML`: the business writes the HTML itself. Read only through MCP. `WEBLAYER_NO_CODE` is not rendered.
- `PRIVACY_TEASER`: a cookie or privacy notice.
- `WEBLAYER_*`: the same as a block placed inside the page instead of a window. A weblayer also has `selector`
  (CSS selector of the place in the page), `location`, and `isExclusive`.

Before a launch, `LEAD_GENERATION`, `SMS_LEAD_GENERATION`, `WEBLAYER_LEAD_GENERATION`, `WEBLAYER_SMS_LEAD_GENERATION` need interest lists on every SUBSCRIBE action.

## popup

`{"type": ..., "screens": [{"elements": [...]}]}`. `type` is `SINGLE_COLUMN`.
The first screen is the form, the next ones are shown after `NEXT_SCREEN` (a thank-you screen).

Each element has a `type`, `payload`, `style` (CSS in camelCase,
values as strings such as "16px"), `locked` (the editor does not let the user delete it), and `actions`.
- `TEXT`: `payload.content`, plain text with line breaks. With `payload.link` (an https:// URL) the text is a link:
  `payload.target` `"_blank"` opens it in a new tab, `payload.closePopup` true closes the popup after the click.
  A call to action with a link ("Shop the sale") is a `TEXT` with a link, styled like a button
  (`backgroundColor`, `color`, `padding`, `borderRadius`).
- `IMAGE`: `payload.src`, an https:// URL, and the same `payload.link`, `target`, and `closePopup` as a `TEXT`.
  Every URL in a popup must be https://, also in CSS `url()` and `image-set()`; a style takes no CSS escapes.
- `INPUT`: `payload.key` (`email`, `phone`, `firstname`, `lastname`), `payload.placeholder`,
  `actions.onChange` = `[{"type": "UPDATE"}]`.
- `BUTTON`: `payload.content`, `payload.submit` true for the form button. A button has no link; it only runs its
  actions. `actions.onClick` runs in order:
  `{"type": "VALIDATE", "key": "email", "rules": ["EMAIL"]}`,
  `{"type": "SUBSCRIBE", "key": "email" or "phone", "payload": {"interests": [<interest list codes>]}}`,
  `{"type": "NEXT_SCREEN"}`.
  `interests` are interest list **codes** (`interest_list_code` from `interest_lists`), not ids.
  The display script ignores an action type it does not know, and `get_popup_definitions` lists the ones it knows.
- `FORM`: an extra form field. `payload.type` is `radio`, `checkbox`, `select`, `number`, or `text`. The answer is saved
  as a contact tag: `payload.tagGroup` (required, except the `subscribeCheckbox` consent box) and `payload.tagKey`
  (required for radio, select, number, text). A radio, select, or checkbox shows the choices in
  `payload.tagOptions`, each with the label shown and the tag value saved:
  `[{"label": "Female", "value": "female"}, {"label": "Male", "value": "male"}]`. A radio and a select have at
  least one choice. The question is a `TEXT` element above the field. Example:
  `{"type": "FORM", "payload": {"type": "radio", "tagGroup": "user", "tagKey": "gender", "tagOptions": [...]},
  "actions": {"onChange": [{"type": "UPDATE"}]}}`.

## teaser

The small bar shown before the popup opens: `{"content": "...", "buttons": {"confirm": "Yes", "dismiss": "No"}, "style": {...}}`.
`{"content": null}` = no teaser, the popup opens directly.

## settings, settingsMobile

Look of the window: `backgroundColor`, `color`, `fontFamily`, `fontSize`, `lineHeight`, `borderRadius`, `left`, `padding`.
`settingsMobile` overrides them on phones.

## options

- `dismissTimeoutDays`: days before a visitor who closed the popup sees it again. 30 by default, -1 = never again.
- `generalBan`: 1 = after this popup the visitor sees no other popup that day, -1 = no such ban.
- `closeToTeaser`: closing the popup shows the teaser again instead of hiding everything.

## rules

When and where the popup shows. Read popups://rules.

## Example: a newsletter signup with a 10 % discount

```json
{
  "objective": "LEAD_GENERATION",
  "teaser": {
    "content": "10 % off your first order?",
    "buttons": {
      "confirm": "Yes",
      "dismiss": "No"
    }
  },
  "popup": {
    "type": "SINGLE_COLUMN",
    "screens": [
      {
        "elements": [
          {
            "type": "TEXT",
            "payload": {
              "content": "Subscribe and get 10 % off your first order."
            },
            "style": {
              "fontSize": "16px",
              "fontWeight": "600"
            }
          },
          {
            "type": "INPUT",
            "locked": true,
            "payload": {
              "key": "email",
              "placeholder": "Your email"
            },
            "actions": {
              "onChange": [
                {
                  "type": "UPDATE"
                }
              ]
            }
          },
          {
            "type": "BUTTON",
            "locked": true,
            "payload": {
              "content": "Get the discount",
              "submit": true
            },
            "actions": {
              "onClick": [
                {
                  "type": "VALIDATE",
                  "key": "email",
                  "rules": [
                    "EMAIL"
                  ]
                },
                {
                  "type": "SUBSCRIBE",
                  "key": "email",
                  "payload": {
                    "interests": [
                      "newsletter"
                    ]
                  }
                },
                {
                  "type": "NEXT_SCREEN"
                }
              ]
            }
          }
        ]
      },
      {
        "elements": [
          {
            "type": "TEXT",
            "payload": {
              "content": "Thank you! The code is on its way to your inbox."
            }
          }
        ]
      }
    ]
  },
  "settings": {
    "backgroundColor": "#ffffff",
    "color": "#1d204c",
    "fontFamily": "Arial,Helvetica,sans-serif",
    "fontSize": "14px",
    "borderRadius": "6px"
  },
  "options": {
    "dismissTimeoutDays": 30,
    "generalBan": -1
  },
  "rules": {
    "operator": "AND",
    "query": [
      {
        "operator": "AND",
        "conditions": [
          {
            "type": "TIME_ON_SITE_SESSION",
            "payload": {
              "seconds": 10
            }
          }
        ]
      }
    ]
  }
}
```

The discount itself is not part of the popup. It is sent by an automation that starts when the visitor subscribes.
