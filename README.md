![UI](UI.png)

## Requirements

- Python 3.x (tkinter ships with the standard installer — no `pip install` needed)

## 1. Run the UI

```bash
python logbook_ui.py
```

## 2. Build your entries

1. **Days** — number of logbook rows. **Start weekday** — weekday of day 1.
   Click **Rebuild rows** to regenerate the table.
2. Per row, set **Clock In** / **Clock Out** (time text + `am`/`pm` selector),
   **Activity**, and **Description**.
3. Per-row flags:
   - **Off** — mark the day OFF. Fields disable; the script clicks the Off
     button for that row.
   - **Skip** — leave the day completely untouched. The row's button is not
     clicked at all (nothing filled, no Off). Use it for days already filled
     correctly that you don't want changed. Disables the rest of the row.
4. Shortcuts:
   - **Set all time** → fill In/Out + am/pm → **Apply times to all**.
   - **Set all text** → fill Activity/Description → **Apply text to all**.
   - Both skip Off and Skip rows. Tweak individual rows afterward.
   - **Reset All** → confirm popup → restores every row to default times,
     blank text, and clears all Off/Skip flags.
5. *(Optional)* **Save JSON…** to keep your values, **Load JSON…** to reload them later.

> Row order must match the row order on the logbook page (top to bottom).

## 3. Generate the script

Click **Generate script.js + copy**. This:

- writes `script.generated.js` next to the UI, and
- copies the full script to your clipboard.

## 4. Run it on the target site

1. Open the logbook page in your browser, on the month/view that shows the rows.
2. Open DevTools console: `F12` (or `Ctrl+Shift+J`), go to the **Console** tab.
3. If the console asks, type `allow pasting` and press Enter.
4. Paste (`Ctrl+V`) the script and press **Enter**.

The script grabs **every** day button in page order (`.detailsbtn`) — both
empty (blue) and already-filled (orange) — and maps `buttons[i]` to
`entries[i]`. For each row it either fills + submits, marks OFF, or (for **Skip**
rows) does nothing and moves on.

## How button matching works

- One combined array in document order, so index `i` always lines up with day
  `i` regardless of button color. This is why the row order in the UI must match
  the page top-to-bottom.
- Filling a day that's already filled (orange) just **overwrites** it — no extra
  flag needed.
- **Skip** is the only way to leave a day alone; its button is never clicked.

## Notes

- Assumes exactly one `.detailsbtn` per day, in day order. If a filled day
  rendered no button, indexes would shift — Skip the affected rows or use
  matching day count.
- Time values are always well-formed (`09:00 am`) because am/pm is a selector,
  not free text.
