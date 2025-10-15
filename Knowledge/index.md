# Knowledge Base Contents

```dataviewjs 
const pages = dv.pages('"Knowledge"')
    .where(p => p.file.name === "index")
    .sort(p => p.file.folder);

// Group by first-level folder
const grouped = {};
for (let page of pages) {
    const parts = page.file.folder.split('/');
    const firstLevel = parts[1]; // Gets folder after "Knowledge"
    
    if (!grouped[firstLevel]) {
        grouped[firstLevel] = [];
    }
    grouped[firstLevel].push(page);
}

// Display grouped results
let isFirst = true;
for (let [firstLevel, subPages] of Object.entries(grouped).sort()) {
    if (!isFirst) {
        dv.el("hr", "");
    }
    isFirst = false;
    
    // Find the index page for the top-level folder
    const topLevelIndex = subPages.find(p => p.file.folder === `Knowledge/${firstLevel}`);
    
    // Display top-level folder as linked header
    if (topLevelIndex) {
        dv.el("h3", dv.fileLink(topLevelIndex.file.path, firstLevel));
    } else {
        dv.header(3, firstLevel);
    }
    
    // Display subfolders
    const subfolders = subPages.filter(p => p.file.folder !== `Knowledge/${firstLevel}`);
    
    if (subfolders.length > 0) {
        dv.table(["Subfolders"], 
            subfolders.map(p => {
                const displayPath = p.file.folder.replace(`Knowledge/${firstLevel}/`, "");
                return [dv.fileLink(p.file.path, displayPath)];
            })
        );
    }
}
```