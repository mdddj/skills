# Hurl - Filters

**Pages:** 1

---

## Equivalent syntax:

**URL:** llms-txt#equivalent-syntax:

jsonpath "$.date" matches /^\d{4}-\d{2}-\d{2}$/
jsonpath "$.name" matches /Hello [a-zA-Z]+!/
hurl
GET https://example.org/hello
HTTP 200
[Asserts]
regex "^(\\d{4}-\\d{2}-\\d{2})$" == "2018-12-31"

**Examples:**

Example 1 (unknown):
```unknown
#### Regex assert {#file-format-asserting-response-regex-assert}

Check that the HTTP received body, decoded as text, matches a regex pattern.
```

---
