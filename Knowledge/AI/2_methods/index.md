
## Contents

```dataview
TABLE WITHOUT ID
  file.link as "Topic",
  summary as "Description"
FROM "Knowledge/AI/2_methods"
WHERE file.name != "index.md"
SORT file.name ASC
````