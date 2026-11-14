<%*
const view = app.workspace.getActiveViewOfType(tp.obsidian.MarkdownView);
if (!view) {
    new Notice('No active Markdown editor found.');
    return;
}

const editor = view.editor;
const selectedText = editor.getSelection();

if (!selectedText || selectedText.trim() === '') {
    new Notice('No text selected. Please select text containing headings.');
    return;
}

const lines = selectedText.split('\n');
const processedLines = [];
let demotedCount = 0;
let hasMaxLevel = false;

for (let line of lines) {
    const match = line.match(/^(#{1,6})\s+(.*)$/);
    if (match) {
        const level = match[1].length;
        if (level >= 6) {
            hasMaxLevel = true;
            processedLines.push(line);
        } else {
            processedLines.push('#' + match[1] + ' ' + match[2]);
            demotedCount++;
        }
    } else {
        processedLines.push(line);
    }
}

if (demotedCount === 0) {
    new Notice(hasMaxLevel ? '⚠️ All headings are at maximum level (######).' : 'No headings found in selection.');
    return;
}

const from = editor.getCursor('from');
const newText = processedLines.join('\n');

editor.replaceSelection(newText);

const newLines = newText.split('\n');
const to = {
    line: from.line + newLines.length - 1,
    ch: newLines.length === 1 ? from.ch + newLines[0].length : newLines[newLines.length - 1].length
};

setTimeout(() => {
    editor.setSelection(from, to);
    editor.focus();
}, 50);

new Notice(`✅ Successfully demoted ${demotedCount} heading${demotedCount > 1 ? 's' : ''}!`, 3000);
%>