# Today's Notes
# Work
# <%*
const currentNoteDate = moment(tp.file.title.split('-').slice(0,3).join('-'), 'YYYY-MM-DD');
let checkDate = currentNoteDate.clone().subtract(1, 'days');
let fileFound = false;
const maxDaysToLook = 14;
let daysChecked = 0;

while (!fileFound && daysChecked < maxDaysToLook) {
    const fileName = `${checkDate.format('YYYY-MM-DD')}-${checkDate.format('dddd')}`;
    const filePath = `${fileName}.md`;  // You can add your folder path here
    const exists = await tp.file.exists(filePath);
    
    if (exists) {
        const content = await app.vault.adapter.read(filePath);
        const workSection = content.match(/# Work\n([\s\S]*?)(?=\n#|$)/);
        
        if (workSection && workSection[1].trim().length > 0) {
            fileFound = true;
            tR += `Previous Work Items (${checkDate.format('dddd')})`;
%>
![[<%*
            tR += `${fileName}#Work`;
%>]]
<%*
            break;
        }
    }
    
    checkDate.subtract(1, 'days');
    daysChecked++;
}

if (!fileFound) {
    tR += "Previous Work Items (None Found)";
}
%>