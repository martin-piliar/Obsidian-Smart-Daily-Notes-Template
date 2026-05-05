# Today's Notes
# Work
<%*
// --- Configuration ---
// Adjust these to match your vault structure.
const SECTION_NAME = "Work";                  // Section heading to look for (without the "# ")
const DAILY_NOTES_FOLDER = "";                // e.g. "Daily Notes/" or "Journal/" — leave empty for vault root
const FILENAME_FORMAT = "YYYY-MM-DD-dddd";    // Match your daily note filename format
const MAX_DAYS_TO_LOOK = 14;                  // How far back to search

// --- Logic ---
const currentNoteDate = moment(tp.file.title.split('-').slice(0, 3).join('-'), 'YYYY-MM-DD');
let checkDate = currentNoteDate.clone().subtract(1, 'days');
let fileFound = false;
let daysChecked = 0;

// Match the section by requiring a top-level "# " heading boundary,
// so sub-headings like "## Tickets" inside the section don't truncate the match.
const sectionRegex = new RegExp(`# ${SECTION_NAME}\\n([\\s\\S]*?)(?=\\n# |$)`);

while (!fileFound && daysChecked < MAX_DAYS_TO_LOOK) {
    const fileName = checkDate.format(FILENAME_FORMAT);
    const filePath = `${DAILY_NOTES_FOLDER}${fileName}.md`;
    const exists = await tp.file.exists(filePath);

    if (exists) {
        const content = await app.vault.adapter.read(filePath);
        const workSection = content.match(sectionRegex);
        if (workSection && workSection[1].trim().length > 0) {
            fileFound = true;
            tR += `Previous ${SECTION_NAME} Items (${checkDate.format('dddd')})\n`;
            tR += `![[${fileName}#${SECTION_NAME}]]`;
            break;
        }
    }

    checkDate.subtract(1, 'days');
    daysChecked++;
}

if (!fileFound) {
    const oldest = checkDate.format('YYYY-MM-DD');
    tR += `Previous ${SECTION_NAME} Items (none found in last ${MAX_DAYS_TO_LOOK} days, back to ${oldest})`;
}
%>