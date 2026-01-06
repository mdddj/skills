# Hurl - Other

**Pages:** 15

---

## Check that our server responds with HTTP/2

**URL:** llms-txt#check-that-our-server-responds-with-http/2

GET https://example.org/api/pets
HTTP/2 200
hurl

**Examples:**

Example 1 (unknown):
```unknown
#### Headers {#file-format-asserting-response-headers}

Optional list of the expected HTTP response headers that must be in the received response.

A header consists of a name, followed by a `:` and a value.

For each expected header, the received response headers are checked. If the received header is not equal to the
expected, or not present, an error is raised. The comparison is case-insensitive for the name: expecting a
`Content-Type` header is equivalent to a `content-type` one. Note that the expected headers list is not fully
descriptive: headers present in the response and not in the expected list doesn't raise error.
```

---

## /usr/local/opt/curl/lib/libcurl.4.dylib is installed by `brew install curl`

**URL:** llms-txt#/usr/local/opt/curl/lib/libcurl.4.dylib-is-installed-by-`brew-install-curl`

$ sudo install_name_tool -change /usr/lib/libcurl.4.dylib /usr/local/opt/curl/lib/libcurl.4.dylib /usr/local/bin/hurl
```

---

## Without content encoding compression:

**URL:** llms-txt#without-content-encoding-compression:

GET https://example.org/a.txt
HTTP 200
[Asserts]
sha256 == hex,abcdef;

---

## ... next entries

**URL:** llms-txt#...-next-entries

**Examples:**

Example 1 (unknown):
```unknown
#### Cookie storage {#file-format-entry-cookie-storage}

Requests in the same Hurl file share the cookie storage, enabling, for example, session based scenario.

#### Redirects {#file-format-entry-redirects}

By default, Hurl doesn't follow redirection. To effectively run a redirection, entries should describe each step
of the redirection, allowing insertion of asserts in each response.
```

---

## No delay!

**URL:** llms-txt#no-delay!

GET https://example.org/turtle
HTTP 200
hurl

**Examples:**

Example 1 (unknown):
```unknown
[Doc](#getting-started-manual-delay)

#### Skipping Requests {#getting-started-samples-skipping-requests}
```

---

## One can specify the file content type:

**URL:** llms-txt#one-can-specify-the-file-content-type:

field3: file,example.zip; application/zip

--boundary
Content-Disposition: form-data; name="key1"

value1
--boundary
Content-Disposition: form-data; name="upload1"; filename="data.txt"
Content-Type: text/plain

Hello World!
--boundary
Content-Disposition: form-data; name="upload2"; filename="data.html"
Content-Type: text/html

<div>Hello <b>World</b>!</div>
--boundary--
hurl
GET https://example.org/index.html
[Cookies]
theme: light
sessionToken: abc123
hurl

**Examples:**

Example 1 (unknown):
```unknown
Files are relative to the input Hurl file, and cannot contain implicit parent directory (`..`). You can use
[`--file-root` option](#getting-started-manual-file-root) to specify the root directory of all file nodes.

Content type can be specified or inferred based on the filename extension:

- `.gif`: `image/gif`,
- `.jpg`: `image/jpeg`,
- `.jpeg`: `image/jpeg`,
- `.png`: `image/png`,
- `.svg`: `image/svg+xml`,
- `.txt`: `text/plain`,
- `.htm`: `text/html`,
- `.html`: `text/html`,
- `.pdf`: `application/pdf`,
- `.xml`: `application/xml`

By default, content type is `application/octet-stream`.

As an alternative to a `[Multipart]` section, multipart forms can also be sent with a [multiline string body](#file-format-request-multiline-string-body):

~~~hurl
POST https://example.org/upload
Content-Type: multipart/form-data; boundary="boundary"
```

Example 2 (unknown):
```unknown
~~~

> When using a multiline string body to send a multipart form data, files content must be inlined in the Hurl file.


#### Cookies {#file-format-request-cookies}

Optional list of session cookies for this request.

A cookie consists of a name, followed by a `:` and a value. Cookies are sent per request, and are not added to
the cookie storage session, contrary to a cookie set in a header response. (for instance `Set-Cookie: theme=light`). The
cookies section starts with `[Cookies]`.
```

Example 3 (unknown):
```unknown
Cookies section can be seen as syntactic sugar over corresponding request header.
```

---

## Resources {#resources}

**URL:** llms-txt#resources-{#resources}

**Contents:**
- License {#resources-license-license}

## License {#resources-license-license}

**Examples:**

Example 1 (unknown):
```unknown
Apache License
                           Version 2.0, January 2004
                        http://www.apache.org/licenses/

TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

1. Definitions.

   "License" shall mean the terms and conditions for use, reproduction,
   and distribution as defined by Sections 1 through 9 of this document.

   "Licensor" shall mean the copyright owner or entity authorized by
   the copyright owner that is granting the License.

   "Legal Entity" shall mean the union of the acting entity and all
   other entities that control, are controlled by, or are under common
   control with that entity. For the purposes of this definition,
   "control" means (i) the power, direct or indirect, to cause the
   direction or management of such entity, whether by contract or
   otherwise, or (ii) ownership of fifty percent (50%) or more of the
   outstanding shares, or (iii) beneficial ownership of such entity.

   "You" (or "Your") shall mean an individual or Legal Entity
   exercising permissions granted by this License.

   "Source" form shall mean the preferred form for making modifications,
   including but not limited to software source code, documentation
   source, and configuration files.

   "Object" form shall mean any form resulting from mechanical
   transformation or translation of a Source form, including but
   not limited to compiled object code, generated documentation,
   and conversions to other media types.

   "Work" shall mean the work of authorship, whether in Source or
   Object form, made available under the License, as indicated by a
   copyright notice that is included in or attached to the work
   (an example is provided in the Appendix below).

   "Derivative Works" shall mean any work, whether in Source or Object
   form, that is based on (or derived from) the Work and for which the
   editorial revisions, annotations, elaborations, or other modifications
   represent, as a whole, an original work of authorship. For the purposes
   of this License, Derivative Works shall not include works that remain
   separable from, or merely link (or bind by name) to the interfaces of,
   the Work and Derivative Works thereof.

   "Contribution" shall mean any work of authorship, including
   the original version of the Work and any modifications or additions
   to that Work or Derivative Works thereof, that is intentionally
   submitted to Licensor for inclusion in the Work by the copyright owner
   or by an individual or Legal Entity authorized to submit on behalf of
   the copyright owner. For the purposes of this definition, "submitted"
   means any form of electronic, verbal, or written communication sent
   to the Licensor or its representatives, including but not limited to
   communication on electronic mailing lists, source code control systems,
   and issue tracking systems that are managed by, or on behalf of, the
   Licensor for the purpose of discussing and improving the Work, but
   excluding communication that is conspicuously marked or otherwise
   designated in writing by the copyright owner as "Not a Contribution."

   "Contributor" shall mean Licensor and any individual or Legal Entity
   on behalf of whom a Contribution has been received by Licensor and
   subsequently incorporated within the Work.

2. Grant of Copyright License. Subject to the terms and conditions of
   this License, each Contributor hereby grants to You a perpetual,
   worldwide, non-exclusive, no-charge, royalty-free, irrevocable
   copyright license to reproduce, prepare Derivative Works of,
   publicly display, publicly perform, sublicense, and distribute the
   Work and such Derivative Works in Source or Object form.

3. Grant of Patent License. Subject to the terms and conditions of
   this License, each Contributor hereby grants to You a perpetual,
   worldwide, non-exclusive, no-charge, royalty-free, irrevocable
   (except as stated in this section) patent license to make, have made,
   use, offer to sell, sell, import, and otherwise transfer the Work,
   where such license applies only to those patent claims licensable
   by such Contributor that are necessarily infringed by their
   Contribution(s) alone or by combination of their Contribution(s)
   with the Work to which such Contribution(s) was submitted. If You
   institute patent litigation against any entity (including a
   cross-claim or counterclaim in a lawsuit) alleging that the Work
   or a Contribution incorporated within the Work constitutes direct
   or contributory patent infringement, then any patent licenses
   granted to You under this License for that Work shall terminate
   as of the date such litigation is filed.

4. Redistribution. You may reproduce and distribute copies of the
   Work or Derivative Works thereof in any medium, with or without
   modifications, and in Source or Object form, provided that You
   meet the following conditions:

   (a) You must give any other recipients of the Work or
   Derivative Works a copy of this License; and

   (b) You must cause any modified files to carry prominent notices
   stating that You changed the files; and

   (c) You must retain, in the Source form of any Derivative Works
   that You distribute, all copyright, patent, trademark, and
   attribution notices from the Source form of the Work,
   excluding those notices that do not pertain to any part of
   the Derivative Works; and

   (d) If the Work includes a "NOTICE" text file as part of its
   distribution, then any Derivative Works that You distribute must
   include a readable copy of the attribution notices contained
   within such NOTICE file, excluding those notices that do not
   pertain to any part of the Derivative Works, in at least one
   of the following places: within a NOTICE text file distributed
   as part of the Derivative Works; within the Source form or
   documentation, if provided along with the Derivative Works; or,
   within a display generated by the Derivative Works, if and
   wherever such third-party notices normally appear. The contents
   of the NOTICE file are for informational purposes only and
   do not modify the License. You may add Your own attribution
   notices within Derivative Works that You distribute, alongside
   or as an addendum to the NOTICE text from the Work, provided
   that such additional attribution notices cannot be construed
   as modifying the License.

   You may add Your own copyright statement to Your modifications and
   may provide additional or different license terms and conditions
   for use, reproduction, or distribution of Your modifications, or
   for any such Derivative Works as a whole, provided Your use,
   reproduction, and distribution of the Work otherwise complies with
   the conditions stated in this License.

5. Submission of Contributions. Unless You explicitly state otherwise,
   any Contribution intentionally submitted for inclusion in the Work
   by You to the Licensor shall be under the terms and conditions of
   this License, without any additional terms or conditions.
   Notwithstanding the above, nothing herein shall supersede or modify
   the terms of any separate license agreement you may have executed
   with Licensor regarding such Contributions.

6. Trademarks. This License does not grant permission to use the trade
   names, trademarks, service marks, or product names of the Licensor,
   except as required for reasonable and customary use in describing the
   origin of the Work and reproducing the content of the NOTICE file.

7. Disclaimer of Warranty. Unless required by applicable law or
   agreed to in writing, Licensor provides the Work (and each
   Contributor provides its Contributions) on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
   implied, including, without limitation, any warranties or conditions
   of TITLE, NON-INFRINGEMENT, MERCHANTABILITY, or FITNESS FOR A
   PARTICULAR PURPOSE. You are solely responsible for determining the
   appropriateness of using or redistributing the Work and assume any
   risks associated with Your exercise of permissions under this License.

8. Limitation of Liability. In no event and under no legal theory,
   whether in tort (including negligence), contract, or otherwise,
   unless required by applicable law (such as deliberate and grossly
   negligent acts) or agreed to in writing, shall any Contributor be
   liable to You for damages, including any direct, indirect, special,
   incidental, or consequential damages of any character arising as a
   result of this License or out of the use or inability to use the
   Work (including but not limited to damages for loss of goodwill,
   work stoppage, computer failure or malfunction, or any and all
   other commercial damages or losses), even if such Contributor
   has been advised of the possibility of such damages.

9. Accepting Warranty or Additional Liability. While redistributing
   the Work or Derivative Works thereof, You may choose to offer,
   and charge a fee for, acceptance of support, warranty, indemnity,
   or other liability obligations and/or rights consistent with this
   License. However, in accepting such obligations, You may act only
   on Your own behalf and on Your sole responsibility, not on behalf
   of any other Contributor, and only if You agree to indemnify,
   defend, and hold each Contributor harmless for any liability
   incurred by, or claims asserted against, such Contributor by reason
   of your accepting any such warranty or additional liability.

END OF TERMS AND CONDITIONS

APPENDIX: How to apply the Apache License to your work.

      To apply the Apache License to your work, attach the following
      boilerplate notice, with the fields enclosed by brackets "[]"
      replaced with your own identifying information. (Don't include
      the brackets!)  The text should be enclosed in the appropriate
      comment syntax for the file format. We also recommend that a
      file or class name and description of purpose be included on the
      same "printed page" as the copyright notice for easier
      identification within third-party archives.

Copyright 2021 Hurl

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

---

## Hurl Documentation

**URL:** llms-txt#hurl-documentation

**Contents:**
- Version 6.1.0 - 12-03-2025

## Version 6.1.0 - 12-03-2025

---

## A very simple Hurl file

**URL:** llms-txt#a-very-simple-hurl-file

---

## Send a cafÃ©, encoded in UTF-8

**URL:** llms-txt#send-a-cafã©,-encoded-in-utf-8

hex,636166c3a90a;
hurl
POST https://example.org

**Examples:**

Example 1 (unknown):
```unknown
##### File body {#file-format-request-file-body}

To use the binary content of a local file as the body request, file body can be used. File body starts with
`file,` and ends with `;``
```

---

## Running hurl --location google.hurl

**URL:** llms-txt#running-hurl---location-google.hurl

GET https://google.fr
HTTP 200
hurl
GET https://google.fr
[Options]
location-trusted: true
HTTP 200
hurl

**Examples:**

Example 1 (unknown):
```unknown
Finally, you can force redirection on a particular request with an [`[Options]` section][options](#file-format-request-options) and the[`--location`](#getting-started-manual-location)
/ [`--location-trusted`](#getting-started-manual-location-trusted) options:
```

Example 2 (unknown):
```unknown
#### Retry {#file-format-entry-retry}

Every entry can be retried upon asserts, captures or runtime errors. Retries allow polling scenarios and effective runs
under flaky conditions. Asserts can be explicit (with an [`[Asserts]` section][asserts](#file-format-response-asserts)), or implicit (like [headers](#file-format-response-headers) or [status code](#file-format-response-version-status)).

Retries can be set globally for every request (see [`--retry`](#getting-started-manual-retry) and [`--retry-interval`](#getting-started-manual-retry-interval)),
or activated on a particular request with an [`[Options]` section][options](#file-format-request-options).

For example, in this Hurl file, first we create a new job then we poll the new job until it's completed:
```

---

## Predicate value with matches predicate:

**URL:** llms-txt#predicate-value-with-matches-predicate:

jsonpath "$.date" matches "^\\d{4}-\\d{2}-\\d{2}$"
jsonpath "$.name" matches "Hello [a-zA-Z]+!"

---

## ... previous entries

**URL:** llms-txt#...-previous-entries

GET https://api.example.org
[Options]
very-verbose: true
HTTP 200

---

## Get home:

**URL:** llms-txt#get-home:

GET https://example.org
HTTP 200
[Captures]
csrf_token: xpath "string(//meta[@name='_csrf_token']/@content)"

---

## Check that user toto is redirected to home after login.

**URL:** llms-txt#check-that-user-toto-is-redirected-to-home-after-login.

**Contents:**
  - Explicit asserts {#file-format-asserting-response-explicit-asserts}

POST https://example.org/login
[Form]
user: toto
password: 12345678
HTTP 302
Location: https://example.org/home

> ETag: W/"<etag_value>"
> ETag: "<etag_value>"
> 
Set-Cookie: theme=light
Set-Cookie: sessionToken=abc123; Expires=Wed, 09 Jun 2021 10:18:14 GMT
hurl
GET https://example.org/index.html
Host: example.net
HTTP 200
Set-Cookie: theme=light
Set-Cookie: sessionToken=abc123; Expires=Wed, 09 Jun 2021 10:18:14 GMT
hurl
GET https://example.org/index.html
Host: example.net
HTTP 200
Set-Cookie: theme=light
hurl
GET https://example.org/home
HTTP 200
[Asserts]
xpath "boolean(count(//h1))" == true
xpath "//h1" exists # Equivalent but simpler
xml
<article
  id="electric-cars"
  data-visible="true"
...
</article>
hurl
GET https://example.org/home
HTTP 200
[Asserts]
xpath "string(//article/@data-visible)" == "true"
hurl

**Examples:**

Example 1 (unknown):
```unknown
> Quotes in the header value are part of the value itself.
>
> This is used by the [ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag) Header
>
```

Example 2 (unknown):
```unknown
Testing duplicated headers is also possible.

For example with the `Set-Cookie` header:
```

Example 3 (unknown):
```unknown
You can either test the two header values:
```

Example 4 (unknown):
```unknown
Or only one:
```

---
