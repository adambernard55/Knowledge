

## Contents

```dataview
TABLE WITHOUT ID
  file.link as "Topic",
  summary as "Description"
FROM "Knowledge/AI/1_methods-and-systems/mcp"
WHERE file.name != "index.md"
SORT file.name ASC
````