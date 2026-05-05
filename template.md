# Today's Notes
# Work
<%*
// --- Configuration ---
// Adjust these to match your vault structure.
const SECTION_NAME = "Work";                  // Section heading to look for (without the "# ")
const DAILY_NOTES_FOLDER = "";                // e.g. "Daily Notes/" or "Journal/" — leave empty for vault root
const FILENAME_FORMAT = "YYYY-MM-DD-dddd";    // Match your daily note filename format
const DATE_FORMAT = "YYYY-MM-DD";             // ISO date prefix used to parse the current note's date
const MAX_DAYS_TO_LOOK = 14;                  // How far back to search

// --- Logic ---
const currentNoteDate = moment(tp.file.title, FILENAME_FORMAT);
let checkDate = currentNoteDate.clone().subtract(1, 'days');
let fileFound = false;
let daysChecked = 0;

// Escape any regex metacharacters in SECTION_NAME (e.g. "Work (Team A)", "C++").
const escapedSection = SECTION_NAME.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
// Match the section by requiring a top-level "# " heading boundary,
// so sub-headings like "## Tickets" inside the section don't truncate the match.
const sectionRegex = new RegExp(`# ${escapedSection}\\n([\\s\\S]*?)(?=\\n# |$)`);

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
    const oldest = currentNoteDate.clone().subtract(MAX_DAYS_TO_LOOK, 'days').format(DATE_FORMAT);
    tR += `Previous ${SECTION_NAME} Items (none found in last ${MAX_DAYS_TO_LOOK} days, back to ${oldest})`;
}
%>