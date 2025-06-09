# Obsidian-Templater-Tool-Demote-Headings


**Features:**
- **Demotes headings**: Converts `#` to `##`, `##` to `###`, etc.
- **Maximum level protection**: Shows a warning when headings are already at level 6 (`######`)
- **Smart selection handling**: Works with any selected text containing headings
- **User feedback**: Provides notifications for success, warnings, and errors

**How to use:**
1. Save this code as a Templater template (e.g., in your templates folder as `Demote Headings.md`)
2. Select the text containing headings you want to demote
3. Run the template using Templater's hotkey or command palette

**What it does:**
- Scans each line in the selected text
- If a line starts with 1-5 `#` symbols, it adds one more `#`
- If a line already has 6 `#` symbols, it shows a warning and leaves it unchanged
- Replaces your selection with the processed text
- Shows appropriate notifications based on the results

The tool handles edge cases like empty selections, text without headings, and mixed content with both regular text and headings.
