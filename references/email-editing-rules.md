# Editing a campaign email

`get_campaign_email_for_editing` returns an email the way the Leadhub email editor keeps it for editing:
`subject`, `preheader`, and `simplified`, the list of elements with their ids. `edit_campaign_email` changes it
with delta operations that address those ids. `get_email_editing_guide` describes every operation and element
type. Read it before the first edit.

## How an edit is saved

- Send all operations of one change in one `edit_campaign_email` call, with the `revision` and `revision_id` you
  read as `base_revision` and `base_revision_id`. The operations are applied together. When one of them fails,
  nothing is saved and the error says why. Fix the operations and send all of them again.
- When the email changed after you read it, the call fails. Read the email again and build the operations on
  the current content.
- A successful call saves a new revision in the email editor. It does not launch, schedule, or send a campaign.
  `revision_effect` in the result says when recipients get the change. Tell the user.
- Every revision stays in the editor history. `list_campaign_email_revisions` lists them, and
  `restore_campaign_email_revision` saves an earlier one again as the newest revision.
- Check the result with `get_campaign_email_content` and `use_newest_editor_revision` true, the rendered email, before you say
  the change is done. When the change affects how the email looks (layout, images, colors, buttons), also look at
  it with `get_campaign_email_screenshot`: a broken layout or a missing image shows only there.
- A preview shows a product recommendation with the sample product saved in the email. To see it with other
  products, pass them in `sample_product_ids` of `get_campaign_email_screenshot`. Never edit the email only to
  change what a preview shows.

## Not available here

- Generating or editing pictures with AI (the `process_image` operation). Use image URLs that already exist:
  from the template, from the user, or from a page the user pointed to. The editor copies an external image to
  the Leadhub CDN when it saves the email.
- The `question` operation. Ask the user in the conversation instead.
- The content of a shared `BOX_TEMPLATE` (an element with a `templateId`). The same block appears in other emails,
  so its inner elements are read-only. Tell the user which parts you could not change.

## Minimal changes

- Change only what the user asked for. An `update` carries only the changed properties.
- Keep the ids of existing elements. A new element gets a new 6-character hex id that is not used in the email.
- Keep the HTML structure, the styles, and the personalization variables in `{{ ... }}` unless the user asks
  to change them.

## Removing a container removes its content

`remove` on a container (`BOX_COLUMNS`, `BOX_THREE_COLUMN`, `BOX_BASIC`, `BOX_TEMPLATE`, ...) deletes everything
inside it. Before you remove one, read its children:

- The container and everything in it should go: remove it.
- It also holds something the user did not ask to change (a hero image, body copy, a button): do not remove
  the container. Remove only the children you were asked to delete, or `move` the content to keep out of it first.
- "Replace the header / footer / section" is the risky case. Those blocks often wrap more than their name says.
  List what is inside before deleting. "Leave the rest unchanged" is a hard limit on `remove`.
  When you cannot tell whether a child belongs to the part being replaced, ask the user.

## Style of new elements

A new element copies the style of the elements already in this email.

- A new `ELEMENT_TEXT` or `ELEMENT_BUTTON` reuses the font family of the text around it. Always set the font.
  An element without one renders in a serif default (Times New Roman). Never bring a serif font into a sans-serif
  email or the other way round. A block you build (product cards, columns, a footer) uses one font.
- A new button reuses `backgroundColor`, `borderRadius`, `width`, `fontSize`, `fontFamily`, and `textColor` of an
  existing button. Heading sizes and text colors follow the same rule.
- Concrete values from the user are binding. Use `#5ba327` and a radius of 18 exactly as given.
- "Unify the buttons" means every `ELEMENT_BUTTON` in the email, including those in product cards. Buttons inside
  a shared `BOX_TEMPLATE` and buttons drawn into an image cannot change. Name them to the user.
- Letter case of a button label is part of the design. A rewritten or translated label keeps the case style of
  the original: an ALL CAPS button stays ALL CAPS. A new button copies the case of the existing ones. The user
  decides the wording, the template decides the case, unless the user asks to change the case.

## Replacing images

- Use only image URLs from the user, the template, or a page the user named. Never construct an image URL.
  When the user names a product or a picture without its URL, keep the current image.
- Keep the aspect ratio of the image you replace. Inside `BOX_COLUMNS` all column images share one aspect ratio.
- Do not pick an image much smaller than the slot. It upscales and blurs. Use the largest variant available.
- Use a different image in every slot, and avoid one that is already visible elsewhere in the email.
  A picture of the old topic next to new text must not stay; leave that slot without an image or ask the user.

## Product cards

- A product element always has its product template. Add it as a `BOX_TEMPLATE` with both `templateId` (the
  product template) and `productId` (the product id from the feed), never as an `ELEMENT_PRODUCT` with a
  `productId` only. Without the template it renders without the product image. Reuse the `templateId` of a
  product card that is already in the email. When there is none, ask the user which product template to use.
- A product card is one unit. Swapping its product changes the image, its `alt`, the link, the product name, and
  the price when the card shows one, all in the same call.
- In a product section you build by hand (`BOX_COLUMNS` with an image, a name, and a button per column), every
  column has the same elements in the same order, the images share one aspect ratio, and the columns box has
  `imageMaxWidth`: (body width - body padding) / columns - 2 x cell padding, about 170 for a 600px email with
  three columns. Prefer the business's own product widget or a shared block, which keep the product data current.

## Translating an email

- Translate `ELEMENT_TEXT`, `ELEMENT_BUTTON`, the subject, and the preheader with `update`, `update_subject`, and
  `update_preheader`. Keep the HTML and the styles, change the text and the `alt` attributes. Use the typography
  of the target language.
- Do not translate the shop name, brands, product placeholders, variables in `{{ ... }}`, or URLs.
- Text drawn into images cannot be translated here. Tell the user which images still show the original language.
