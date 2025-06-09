<%*
// Obsidian Templater Tool: Demote Headings
// This tool converts selected headings to sub-headings (adds one more #)
// Usage: Select text containing headings, then run this template

// Get the selected text
const selectedText = tp.file.selection();

// Check if there's selected text
if (!selectedText || selectedText.trim() === '') {
    new Notice('No text selected. Please select text containing headings to demote.');
    return;
}

// Function to demote headings
function demoteHeadings(text) {
    const lines = text.split('\n');
    const processedLines = [];
    let hasMaxLevelHeadings = false;
    let hasChanges = false;
    
    for (let line of lines) {
        // Check if line is a heading (starts with #)
        const headingMatch = line.match(/^(#{1,6})\s+(.*)$/);
        
        if (headingMatch) {
            const currentLevel = headingMatch[1].length;
            const headingText = headingMatch[2];
            
            // Check if already at maximum level (6 #s)
            if (currentLevel >= 6) {
                hasMaxLevelHeadings = true;
                processedLines.push(line); // Keep unchanged
            } else {
                // Add one more # to demote the heading
                const newHeading = '#' + headingMatch[1] + ' ' + headingText;
                processedLines.push(newHeading);
                hasChanges = true;
            }
        } else {
            // Not a heading, keep as is
            processedLines.push(line);
        }
    }
    
    return {
        text: processedLines.join('\n'),
        hasMaxLevelHeadings,
        hasChanges
    };
}

// Process the selected text
const result = demoteHeadings(selectedText);

// Show warnings if needed
if (result.hasMaxLevelHeadings) {
    new Notice('⚠️ Warning: Some headings are already at maximum level (######) and cannot be demoted further.', 5000);
}

if (!result.hasChanges && !result.hasMaxLevelHeadings) {
    new Notice('No headings found in the selected text.', 3000);
    return;
}

if (result.hasChanges) {
    // Replace the selected text with the processed text
    const editor = app.workspace.activeLeaf.view.editor;
    editor.replaceSelection(result.text);
    
    const changeCount = (result.text.match(/^#{2,6}\s+/gm) || []).length - (selectedText.match(/^#{2,6}\s+/gm) || []).length;
    new Notice(`✅ Successfully demoted ${changeCount} headings!`, 3000);
} else {
    new Notice('No changes made - all headings were already at maximum level.', 3000);
}
%>