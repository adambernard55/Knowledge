# Knowledge Base Contents

```dataviewjs 
const pages = dv.pages('"Knowledge"')
    .where(p => p.file.name === "index" && p.file.folder !== "Knowledge")
    .sort(p => p.file.folder);

// Group by first-level folder
const grouped = {};
for (let page of pages) {
    const parts = page.file.folder.split('/');
    
    // Skip if deeper than 3 parts
    if (parts.length > 3) continue;
    
    const firstLevel = parts[1];
    
    if (!firstLevel) continue;
    
    if (!grouped[firstLevel]) {
        grouped[firstLevel] = [];
    }
    grouped[firstLevel].push(page);
}

// Display grouped results
let isFirst = true;
for (let [firstLevel, subPages] of Object.entries(grouped).sort()) {
    if (!firstLevel || firstLevel === 'undefined') continue;
    
    // Only display if there's actually a top-level index
    const topLevelIndex = subPages.find(p => p.file.folder === `Knowledge/${firstLevel}`);
    
    // Skip this entire group if there's no top-level index
    if (!topLevelIndex) continue;
    
    if (!isFirst) {
        dv.el("hr", "");
    }
    isFirst = false;
    
    // Display top-level folder as linked header
    dv.el("h3", dv.fileLink(topLevelIndex.file.path, firstLevel));
    
    // Display ONLY direct subfolders
    const subfolders = subPages.filter(p => {
        const parts = p.file.folder.split('/');
        return parts.length === 3 && parts[2] && p.file.folder !== `Knowledge/${firstLevel}`;
    });
    
    if (subfolders.length > 0) {
        dv.list(
            subfolders.map(p => {
                const parts = p.file.folder.split('/');
                const folderName = parts[2];
                return dv.fileLink(p.file.path, folderName);
            })
        );
    }
}
```