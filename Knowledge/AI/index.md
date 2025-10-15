
# AI Knowledge Index

```dataview 
TABLE WITHOUT ID link(key + "/index", replace(key, "Knowledge/AI/", "")) AS "Table of Contents" FROM "Knowledge/AI" GROUP BY file.folder SORT key ASC
```