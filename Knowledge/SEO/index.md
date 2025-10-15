# SEO Index


```dataview 
TABLE WITHOUT ID link(key + "/index", replace(key, "Knowledge/SEO/", "")) AS "Table of Contents" FROM "Knowledge/SEO" GROUP BY file.folder SORT key ASC
```