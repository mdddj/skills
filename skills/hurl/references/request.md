# Hurl - Request

**Pages:** 33

---

## The 'Content-Type' HTTP response header does not precise the charset 'gb2312'

**URL:** llms-txt#the-'content-type'-http-response-header-does-not-precise-the-charset-'gb2312'

---

## Run the same POST request with a body section:

**URL:** llms-txt#run-the-same-post-request-with-a-body-section:

POST https://example.org/test
Content-Type: application/x-www-form-urlencoded
`name=John%20Doe&key1=value1`
~~~

When both [body section](#file-format-request-body) and form parameters section are present, only the body section is taken into account.

#### Multipart Form Data {#file-format-request-multipart-form-data}

A multipart form data section can be used to send data, with key / value and file content
(see [multipart/form-data on MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST)).

The form parameters section starts with `[Multipart]`.

```hurl
POST https://example.org/upload
[Multipart]
field1: value1
field2: file,example.txt;

---

## Do a POST request without CSRF token and check

**URL:** llms-txt#do-a-post-request-without-csrf-token-and-check

---

## File Format {#file-format}

**URL:** llms-txt#file-format-{#file-format}

**Contents:**
- Hurl File {#file-format-hurl-file-hurl-file}
  - Character Encoding {#file-format-hurl-file-character-encoding}
  - File Extension {#file-format-hurl-file-file-extension}
  - Comments {#file-format-hurl-file-comments}

## Hurl File {#file-format-hurl-file-hurl-file}

### Character Encoding {#file-format-hurl-file-character-encoding}

Hurl file should be encoded in UTF-8, without a byte order mark at the beginning
(while Hurl ignores the presence of a byte order mark rather than treating it as an error)

### File Extension {#file-format-hurl-file-file-extension}

Hurl file extension is `.hurl`

### Comments {#file-format-hurl-file-comments}

Comments begin with `#` and continue until the end of line. Hurl file can serve as
a documentation for HTTP based workflows so it can be useful to be very descriptive.

---

## so body must be decoded explicitly by Hurl before processing any text based assert

**URL:** llms-txt#so-body-must-be-decoded-explicitly-by-hurl-before-processing-any-text-based-assert

GET https://example.org/hello_china
HTTP 200
[Asserts]
header "Content-Type" == "text/html"

---

## Without content encoding, asserts remains identical

**URL:** llms-txt#without-content-encoding,-asserts-remains-identical

GET https://example.org
HTTP 200
[Asserts]
header "Content-Encoding" not exists
body contains "<h1>Welcome!</h1>"
hurl
GET https://example.org/data.bin
HTTP 200
[Asserts]
bytes startsWith hex,efbbbf;
bytes count == 12424
header "Content-Length" == "12424"
plain
$ curl -v https://example.org

< HTTP/1.1 200 OK
< Content-Type: text/html; charset=UTF-8
...
<!doctype html>
<html>
  <head>
    <title>Example Domain</title>
    ...
  </head>

<body>
    <div>
      <h1>Example</h1>
      <p>This domain is for use in illustrative examples in documents. You may use this domain in literature without prior coordination or asking for permission.</p>
      <p><a href="https://www.iana.org/domains/example">More information...</a></p>
    </div>
  </body>
</html>
hurl
GET https://example.org
HTTP 200
Content-Type: text/html; charset=UTF-8
[Asserts]
xpath "string(/html/head/title)" contains "Example" # Check title
xpath "count(//p)" == 2                             # Check the number of <p>
xpath "//p" count == 2                              # Similar assert for <p>
xpath "boolean(count(//h2))" == false               # Check there is no <h2>
xpath "//h2" not exists                             # Similar assert for <h2>
xml
<?xml version="1.0"?>
<!-- both namespace prefixes are available throughout -->
<bk:book xmlns:bk='urn:loc.gov:books'
         xmlns:isbn='urn:ISBN:0-395-36341-6'>
    <bk:title>Cheaper by the Dozen</bk:title>
    <isbn:number>1568491379</isbn:number>
</bk:book>
hurl
GET http://localhost:8000/assert-xpath
HTTP 200
[Asserts]

xpath "string(//bk:book/bk:title)" == "Cheaper by the Dozen"
xpath "string(//*[name()='bk:book']/*[name()='bk:title'])" == "Cheaper by the Dozen"
xpath "string(//*[local-name()='book']/*[local-name()='title'])" == "Cheaper by the Dozen"

xpath "string(//bk:book/isbn:number)" == "1568491379"
xpath "string(//*[name()='bk:book']/*[name()='isbn:number'])" == "1568491379"
xpath "string(//*[local-name()='book']/*[local-name()='number'])" == "1568491379"
plain
curl -v http://httpbin.org/json

< HTTP/1.1 200 OK
< Content-Type: application/json
...

{
  "slideshow": {
    "author": "Yours Truly",
    "date": "date of publication",
    "slides": [
      {
        "title": "Wake up to WonderWidgets!",
        "type": "all"
      },
       ...
    ],
    "title": "Sample Slide Show"
  }
}
hurl
GET http://httpbin.org/json
HTTP 200
[Asserts]
jsonpath "$.slideshow.author" == "Yours Truly"
jsonpath "$.slideshow.slides[0].title" contains "Wonder"
jsonpath "$.slideshow.slides" count == 2
jsonpath "$.slideshow.date" != null
jsonpath "$.slideshow.slides[*].title" contains "Mind Blowing!"
hurl
GET https://example.org/hello
HTTP 200
[Asserts]

**Examples:**

Example 1 (unknown):
```unknown
#### Bytes assert {#file-format-asserting-response-bytes-assert}

Check the value of the received HTTP response body as a bytestream. Body assert consists of the keyword `bytes`
followed by a predicate function and value.
```

Example 2 (unknown):
```unknown
Like `body` assert, `bytes` assert works _after_ content encoding decompression (so the predicates values are not
affected by `Content-Encoding` response header value).

#### XPath assert {#file-format-asserting-response-xpath-assert}

Check the value of a [XPath](https://en.wikipedia.org/wiki/XPath) query on the received HTTP body decoded as a string (using the `charset` value in the
`Content-Type` header response). Currently, only XPath 1.0 expression can be used. Body assert consists of the
keyword `xpath` followed by a predicate function and value. Values can be string,
boolean or number depending on the XPath query.

Let's say we want to check this HTML response:
```

Example 3 (unknown):
```unknown
With Hurl, we can write multiple XPath asserts describing the DOM content:
```

Example 4 (unknown):
```unknown
XML Namespaces are also supported. Let's say you want to check this XML response:
```

---

## A request with (almost) no check:

**URL:** llms-txt#a-request-with-(almost)-no-check:

---

## Explicit check of Set-Cookie header value. If the attributes are

**URL:** llms-txt#explicit-check-of-set-cookie-header-value.-if-the-attributes-are

---

## This request will be played exactly 3 times

**URL:** llms-txt#this-request-will-be-played-exactly-3-times

GET https://example.org/foo
[Options]
repeat: 3
HTTP 200

---

## But, the 'Content-Type' HTTP response header doesn't precise any charset,

**URL:** llms-txt#but,-the-'content-type'-http-response-header-doesn't-precise-any-charset,

---

## This request is skipped

**URL:** llms-txt#this-request-is-skipped

GET https://example.org/foo
[Options]
skip: true
HTTP 200
hurl

**Examples:**

Example 1 (unknown):
```unknown
Additionally, a `delay` can be inserted between requests, to add a delay before execution of a request (aka sleep).
```

---

## Authorization header value can be computed with `echo -n 'bob:secret' | base64`

**URL:** llms-txt#authorization-header-value-can-be-computed-with-`echo--n-'bob:secret'-|-base64`

GET https://example.org/protected
Authorization: Basic Ym9iOnNlY3JldA==
hurl

**Examples:**

Example 1 (unknown):
```unknown
Basic authentication allows per request authentication.
If you want to add basic authentication to all the requests of a Hurl file
you can use [`-u/--user` option](#getting-started-manual-user).

#### Body {#file-format-request-body}

Optional HTTP body request.

If the body of the request is a [JSON](https://www.json.org) string or a [XML](https://en.wikipedia.org/wiki/XML) string, the value can be
directly inserted without any modification. For a text based body that is neither JSON nor XML,
one can use [multiline string body](#file-format-request-multiline-string-body) that starts with <code>&#96;&#96;&#96;</code> and ends
with <code>&#96;&#96;&#96;</code>. Multiline string body support "language hint" and can be used
to create [GraphQL queries](#file-format-request-graphql-query).

For a precise byte control of the request body, [Base64](https://en.wikipedia.org/wiki/Base64) encoded string, [hexadecimal string](#file-format-request-hex-body)
or [included file](#file-format-request-file-body) can be used to describe exactly the body byte content.

> You can set a body request even with a `GET` body, even if this is not a common practice.

The body section must be the last section of the request configuration.

##### JSON body {#file-format-request-json-body}

JSON request body is used to set a literal JSON as the request body.
```

---

## Request a gzipped reponse, the `body` asserts works with ungzipped response

**URL:** llms-txt#request-a-gzipped-reponse,-the-`body`-asserts-works-with-ungzipped-response

GET https://example.org
Accept-Encoding: gzip
HTTP 200
[Asserts]
header "Content-Encoding" == "gzip"
body contains "<h1>Welcome!</h1>"

---

## A 5 seconds delayed request

**URL:** llms-txt#a-5-seconds-delayed-request

**Contents:**
- Request {#file-format-request-request}
  - Definition {#file-format-request-definition}
  - Example {#file-format-request-example}
  - Structure {#file-format-request-structure}
  - Description {#file-format-request-description}

GET https://example.org/foo
[Options]
delay: 5s
HTTP 200
shell
$ hurl --delay 500ms --repeat 3 foo.hurl
hurl
GET https://example.org/api/dogs?id=4567
User-Agent: My User Agent
Content-Type: application/json
[BasicAuth]
alice: secret
hurl
GET https://example.org/api/dogs
User-Agent: My User Agent
[Query]
id: 4567
order: newest
[BasicAuth]
alice: secret
hurl
GET https://example.org/api/dogs
User-Agent: My User Agent
[BasicAuth]
alice: secret
[Query]
id: 4567
order: newest
hurl
POST https://example.org/api/dogs?id=4567
User-Agent: My User Agent
{
 "name": "Ralphy"
}
hurl

**Examples:**

Example 1 (unknown):
```unknown
[`delay`](#getting-started-manual-retry) and [`repeat`](#getting-started-manual-repeat) can also be used globally as command line options:
```

Example 2 (unknown):
```unknown
For complete reference, below is a diagram for the executed entries.

<div class="picture">
    <img class="u-theme-light u-drop-shadow u-border u-max-width-100" src="https://hurl.dev/assets/img/run-cycle-light.svg" alt="Run cycle explanation"/>
    <img class="u-theme-dark u-drop-shadow u-border u-max-width-100" src="https://hurl.dev/assets/img/run-cycle-dark.svg" alt="Run cycle explanation"/>
</div>





<hr>

## Request {#file-format-request-request}

### Definition {#file-format-request-definition}

Request describes an HTTP request: a mandatory [method](#file-format-request-method) and [URL](#file-format-request-url), followed by optional [headers](#file-format-request-headers).

Then, [options](#file-format-request-options), [query parameters](#file-format-request-query-parameters), [form parameters](#file-format-request-form-parameters), [multipart form data](#file-format-request-multipart-form-data), [cookies](#file-format-request-cookies), and [basic authentication](#file-format-request-basic-authentication)
can be used to configure the HTTP request.

Finally, an optional [body](#file-format-request-body) can be used to configure the HTTP request body.

### Example {#file-format-request-example}
```

Example 3 (unknown):
```unknown
### Structure {#file-format-request-structure}

<div class="hurl-structure-schema">
  <div class="hurl-structure">
    <div class="hurl-structure-col-0">
        <div class="hurl-part-0">
            PUT https://sample.net
        </div>
        <div class="hurl-part-1">
            accept: */*<br>x-powered-by: Express<br>user-agent: Test
        </div>
        <div class="hurl-part-2">
            [Options]<br>...
        </div>
        <div class="hurl-part-2">
            [Query]<br>...
        </div>
        <div class="hurl-part-2">
            [Form]<br>...
        </div>
        <div class="hurl-part-2">
            [BasicAuth]<br>...
        </div>
        <div class="hurl-part-2">
            [Cookies]<br>...
        </div>
        <div class="hurl-part-2">
            ...
        </div>
        <div class="hurl-part-2">
            ...
        </div>
        <div class="hurl-part-3">
            {<br>
            &nbsp;&nbsp;"type": "FOO",<br>
            &nbsp;&nbsp;"value": 356789,<br>
            &nbsp;&nbsp;"ordered": true,<br>
            &nbsp;&nbsp;"index": 10<br>
            }
        </div>
    </div>
    <div class="hurl-structure-col-1">
        <div class="hurl-request-explanation-part-0">
            <a href="#file-format-request-method">Method</a> and <a href="#file-format-request-url">URL</a> (mandatory)
        </div>
        <div class="hurl-request-explanation-part-1">
            <br><a href="#file-format-request-headers">HTTP request headers</a> (optional)
        </div>
        <div class="hurl-request-explanation-part-2">
            <br>
            <br>
            <br>
            <br>
            <br>
        </div>
        <div class="hurl-request-explanation-part-2">
            <a href="#file-format-options">Options</a>, <a href="#file-format-request-query-parameters">query strings</a>, <a href="#file-format-request-form-parameters">form params</a>, <a href="#file-format-request-cookies">cookies</a>, <a href="#file-format-request-basic-authentication">authentication</a> ...<br>(optional sections, unordered)
        </div>
        <div class="hurl-request-explanation-part-2">
            <br>
            <br>
            <br>
            <br>
        </div>
        <div class="hurl-request-explanation-part-3">
            <br>
        </div>
        <div class="hurl-request-explanation-part-3">
            <a href="#file-format-request-body">HTTP request body</a> (optional)
        </div>
    </div>
</div>
</div>


[Headers](#file-format-request-headers), if present, follow directly after the [method](#file-format-request-method) and [URL](#file-format-request-url). This allows Hurl format to 'look like' the real HTTP format.
Contrary to HTTP headers, other parameters are defined in sections (`[Cookies]`, `[Query]`, `[Form]` etc...)
These sections are not ordered and can be mixed in any way:
```

Example 4 (unknown):
```unknown

```

---

## Implicit assert on `Content-Type` Header

**URL:** llms-txt#implicit-assert-on-`content-type`-header

Content-Type: application/json; charset=utf-8
[Asserts]

---

## Run a GET request with cookies section:

**URL:** llms-txt#run-a-get-request-with-cookies-section:

GET https://example.org/index.html
[Cookies]
theme: light
sessionToken: abc123

---

## Capture the CSRF token value from html body.

**URL:** llms-txt#capture-the-csrf-token-value-from-html-body.

[Captures]
csrf_token: xpath "normalize-space(//meta[@name='_csrf_token']/@content)"

---

## Perform basic authentication with login `bob` and password `secret`.

**URL:** llms-txt#perform-basic-authentication-with-login-`bob`-and-password-`secret`.

GET https://example.org/protected
[BasicAuth]
bob: secret
hurl

**Examples:**

Example 1 (unknown):
```unknown
> Spaces surrounded username and password are trimmed. If you
> really want a space in your password (!!), you could use [Hurl unicode literals \u{20}](#file-format-hurl-file-special-characters-in-strings).

This is equivalent (but simpler) to construct the request with a [Authorization](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization) header:
```

---

## An options section, each option is optional and applied only to this request...

**URL:** llms-txt#an-options-section,-each-option-is-optional-and-applied-only-to-this-request...

[Options]
aws-sigv4: aws:amz:sts     # generate AWS SigV4 Authorization header
cacert: /etc/cert.pem      # custom certificate file
cert: /etc/client-cert.pem # client authentication certificate
key: /etc/client-cert.key  # client authentication certificate key
compressed: true           # request a compressed response
connect-timeout: 20s       # connect timeout
delay: 3s                  # delay for this request (aka sleep)
http3: true                # use HTTP/3 protocol version
insecure: true             # allow insecure SSL connections and transfers
ipv6: true                 # use IPv6 addresses
limit-rate: 32000          # limit this request to the specidied speed (bytes/s)
location: true             # follow redirection for this request
max-redirs: 10             # maximum number of redirections
output: out.html           # dump the response to this file
path-as-is: true           # do not handle sequences of /../ or /./ in URL path
retry: 10                  # number of retry if HTTP/asserts errors
retry-interval: 500ms      # interval between retry
skip: false                # skip this request
unix-socket: sock          # use Unix socket for transfer
user: bob:secret           # use basic authentication
proxy: my.proxy:8012       # define proxy (host:port where host can be an IP address)
variable: country=Italy    # define variable country
variable: planet=Earth     # define variable planet
verbose: true              # allow verbose output
very-verbose: true         # allow more verbose output
hurl
GET https://example.org/news
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:70.0) Gecko/20100101 Firefox/70.0
[Query]
order: newest
search: {{custom-search}}
count: 100
hurl
POST https://example.org/contact
[Form]
default: false
token: {{token}}
email: john.doe@rookie.org
number: 33611223344
```

Form parameters section can be seen as syntactic sugar over body section (values in form parameters section
are not URL encoded.). A [oneline string body](#file-format-request-oneline-string-body) could be used instead of a forms parameters section.

**Examples:**

Example 1 (unknown):
```unknown
> Variable defined in an `[Options]` section are defined also for the next entries. This is
> the exception, all other options are defined only for the current request.


#### Query parameters {#file-format-request-query-parameters}

Optional list of query parameters.

A query parameter consists of a field, followed by a `:` and a value. The query parameters section starts with
`[Query]`. Contrary to query parameters in the URL, each value in the query parameters section is not
URL encoded.
```

Example 2 (unknown):
```unknown
If there are any parameters in the URL, the resulted request will have both parameters.

#### Form parameters {#file-format-request-form-parameters}

A form parameters section can be used to send data, like [HTML form](https://developer.mozilla.org/en-US/docs/Learn/Forms).

This section contains an optional list of key values, each key followed by a `:` and a value. Key values will be
encoded in key-value tuple separated by '&', with a '=' between the key and the value, and sent in the body request.
The content type of the request is `application/x-www-form-urlencoded`. The form parameters section starts
with `[Form]`.
```

---

## Create a new doggy thing with JSON body:

**URL:** llms-txt#create-a-new-doggy-thing-with-json-body:

POST https://example.org/api/dogs

##### XML body {#file-format-request-xml-body}

XML request body is used to set a literal XML as the request body.

**Examples:**

Example 1 (json):
```json
{
    "id": 0,
    "name": "Frieda",
    "picture": "images/scottish-terrier.jpeg",
    "age": 3,
    "breed": "Scottish Terrier",
    "location": "Lisco, Alabama"
}
```

---

## A request with URL containing query parameters.

**URL:** llms-txt#a-request-with-url-containing-query-parameters.

GET https://example.org/forum/questions/?search=Install%20Linux&order=newest

---

## Do the login !

**URL:** llms-txt#do-the-login-!

**Contents:**
  - Functions {#file-format-templates-functions}
  - Types {#file-format-templates-types}
  - Injecting Variables {#file-format-templates-injecting-variables}
  - Templating Body {#file-format-templates-templating-body}
- Grammar {#file-format-grammar-grammar}
  - Definitions {#file-format-grammar-definitions}
  - Syntax Grammar {#file-format-grammar-syntax-grammar}

POST https://acmecorp.net/login?user=toto&password=1234
X-CSRF-TOKEN: {{csrf_token}}
HTTP 302
hurl
GET https://example.org/api/index
HTTP 200
[Captures]
index: body

GET https://example.org/api/status
HTTP 200
[Asserts]
jsonpath "$.errors[{{index}}].id" == "error"
hurl
GET https://example.org/api/foo
[Query]
date: {{newDate}}
HTTP 200
hurl
POST https://example.org/api/foo
{
  "name": "foo",
  "email": "{{newUuid}}@test.com"
}

{
  "name": "foo",
  "email": "0531f78f-7f87-44be-a7f2-969a1c4e6d97@test.com"
}
hurl
GET https://sample/counter

HTTP 200
[Captures]
count: jsonpath "$.results[0]"
hurl
GET https://sample/counter/{{count}}

HTTP 200
[Asserts]
jsonpath "$.id" == "{{count}}"
hurl
GET https://sample/counter/458

HTTP 200
[Asserts]
jsonpath "$.id" == "458"
hurl
GET https://sample/counter/{{count}}

HTTP 200
[Asserts]
jsonpath "$.index" == {{count}}
hurl
GET https://sample/counter/458

HTTP 200
[Asserts]
jsonpath "$.index" == 458
hurl
GET https://{{host}}/{{id}}/status
HTTP 304

GET https://{{host}}/health
HTTP 200
shell
$ hurl --variable host=example.net --variable id=1234 test.hurl
shell
$ hurl --variables-file vars.env test.hurl

host=example.net
id=1234
shell
$ export HURL_host=example.net
$ export HURL_id=1234
$ hurl test.hurl
hurl
GET https://{{host}}/{{id}}/status
[Options]
variable: host=example.net
variable: id=1234
HTTP 304

GET https://{{host}}/health
HTTP 200
shell
$ hurl --secret token=FooBar test.hurl
shell
$ hurl --secret token=FooBar \
       --secret token_alt_0=FOOBAR \
       --secret token_alt_1=foobar \
       test.hurl
 ``:

~~~hurl
PUT https://example.org/api/hits
Content-Type: application/json

Variables can be initialized via command line:

Resulting in a PUT request with the following JSON body:

## Grammar {#file-format-grammar-grammar}

### Definitions {#file-format-grammar-definitions}

- operator &#124; denotes alternative,
- operator * denotes iteration (zero or more),
- operator + denotes iteration (one or more),

### Syntax Grammar {#file-format-grammar-syntax-grammar}

<div class="grammar-ruleset"><h3 id="general">General</h3><div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="hurl-file">hurl-file</span></div><div class="grammar-rule-expression"><a href="#entry">entry</a><span class="grammar-symbol">*</span><br>
<a href="#lt">lt</a><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="entry">entry</span><span class="grammar-usedby">(used by <a href="#hurl-file">hurl-file</a>)</span></div><div class="grammar-rule-expression"><a href="#request">request</a><br>
<a href="#response">response</a><span class="grammar-symbol">?</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="request">request</span><span class="grammar-usedby">(used by <a href="#entry">entry</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<a href="#method">method</a>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#value-string">value-string</a>&nbsp;<a href="#lt">lt</a><br>
<a href="#header">header</a><span class="grammar-symbol">*</span><br>
<a href="#request-section">request-section</a><span class="grammar-symbol">*</span><br>
<a href="#body">body</a><span class="grammar-symbol">?</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="response">response</span><span class="grammar-usedby">(used by <a href="#entry">entry</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<a href="#version">version</a>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#status">status</a>&nbsp;<a href="#lt">lt</a><br>
<a href="#header">header</a><span class="grammar-symbol">*</span><br>
<a href="#response-section">response-section</a><span class="grammar-symbol">*</span><br>
<a href="#body">body</a><span class="grammar-symbol">?</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="method">method</span><span class="grammar-usedby">(used by <a href="#request">request</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">[A-Z]+</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="version">version</span><span class="grammar-usedby">(used by <a href="#response">response</a>)</span></div><div class="grammar-rule-expression">&nbsp;<span class="grammar-literal">HTTP/1.0</span><br>
<span class="grammar-symbol">|</span><span class="grammar-literal">HTTP/1.1</span><br>
<span class="grammar-symbol">|</span><span class="grammar-literal">HTTP/2</span><br>
<span class="grammar-symbol">|</span><span class="grammar-literal">HTTP</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="status">status</span><span class="grammar-usedby">(used by <a href="#response">response</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">[0-9]+</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="header">header</span><span class="grammar-usedby">(used by <a href="#request">request</a>,&nbsp;<a href="#response">response</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<a href="#key-value">key-value</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="body">body</span><span class="grammar-usedby">(used by <a href="#request">request</a>,&nbsp;<a href="#response">response</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<a href="#bytes">bytes</a>&nbsp;<a href="#lt">lt</a></div></div>
</div><div class="grammar-ruleset"><h3 id="sections">Sections</h3><div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="request-section">request-section</span><span class="grammar-usedby">(used by <a href="#request">request</a>)</span></div><div class="grammar-rule-expression">&nbsp;<a href="#basic-auth-section">basic-auth-section</a><br>
<span class="grammar-symbol">|</span><a href="#query-string-params-section">query-string-params-section</a><br>
<span class="grammar-symbol">|</span><a href="#form-params-section">form-params-section</a><br>
<span class="grammar-symbol">|</span><a href="#multipart-form-data-section">multipart-form-data-section</a><br>
<span class="grammar-symbol">|</span><a href="#cookies-section">cookies-section</a><br>
<span class="grammar-symbol">|</span><a href="#options-section">options-section</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="response-section">response-section</span><span class="grammar-usedby">(used by <a href="#response">response</a>)</span></div><div class="grammar-rule-expression">&nbsp;<a href="#captures-section">captures-section</a><br>
<span class="grammar-symbol">|</span><a href="#asserts-section">asserts-section</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="query-string-params-section">query-string-params-section</span><span class="grammar-usedby">(used by <a href="#request-section">request-section</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<span class="grammar-symbol">(</span><span class="grammar-literal">[QueryStringParams]</span><span class="grammar-symbol">|</span><span class="grammar-literal">[Query]</span><span class="grammar-symbol">)</span>&nbsp;<a href="#lt">lt</a><br>
<a href="#key-value">key-value</a><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="form-params-section">form-params-section</span><span class="grammar-usedby">(used by <a href="#request-section">request-section</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<span class="grammar-symbol">(</span><span class="grammar-literal">[FormParams]</span><span class="grammar-symbol">|</span><span class="grammar-literal">[Form]</span><span class="grammar-symbol">)</span>&nbsp;<a href="#lt">lt</a><br>
<a href="#key-value">key-value</a><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="multipart-form-data-section">multipart-form-data-section</span><span class="grammar-usedby">(used by <a href="#request-section">request-section</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<span class="grammar-symbol">(</span><span class="grammar-literal">[MultipartFormData]</span><span class="grammar-symbol">|</span><span class="grammar-literal">[Multipart]</span><span class="grammar-symbol">)</span>&nbsp;<a href="#lt">lt</a><br>
<a href="#multipart-form-data-param">multipart-form-data-param</a><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="cookies-section">cookies-section</span><span class="grammar-usedby">(used by <a href="#request-section">request-section</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<span class="grammar-literal">[Cookies]</span>&nbsp;<a href="#lt">lt</a><br>
<a href="#key-value">key-value</a><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="captures-section">captures-section</span><span class="grammar-usedby">(used by <a href="#response-section">response-section</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<span class="grammar-literal">[Captures]</span>&nbsp;<a href="#lt">lt</a><br>
<a href="#capture">capture</a><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="asserts-section">asserts-section</span><span class="grammar-usedby">(used by <a href="#response-section">response-section</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<span class="grammar-literal">[Asserts]</span>&nbsp;<a href="#lt">lt</a><br>
<a href="#assert">assert</a><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="basic-auth-section">basic-auth-section</span><span class="grammar-usedby">(used by <a href="#request-section">request-section</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<span class="grammar-literal">[BasicAuth]</span>&nbsp;<a href="#lt">lt</a><br>
<a href="#key-value">key-value</a><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="options-section">options-section</span><span class="grammar-usedby">(used by <a href="#request-section">request-section</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<span class="grammar-literal">[Options]</span>&nbsp;<a href="#lt">lt</a><br>
<a href="#option">option</a><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="key-value">key-value</span><span class="grammar-usedby">(used by <a href="#header">header</a>,&nbsp;<a href="#query-string-params-section">query-string-params-section</a>,&nbsp;<a href="#form-params-section">form-params-section</a>,&nbsp;<a href="#cookies-section">cookies-section</a>,&nbsp;<a href="#basic-auth-section">basic-auth-section</a>,&nbsp;<a href="#multipart-form-data-param">multipart-form-data-param</a>)</span></div><div class="grammar-rule-expression"><a href="#key-string">key-string</a>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#value-string">value-string</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="multipart-form-data-param">multipart-form-data-param</span><span class="grammar-usedby">(used by <a href="#multipart-form-data-section">multipart-form-data-section</a>)</span></div><div class="grammar-rule-expression"><a href="#file-param">file-param</a><span class="grammar-symbol">|</span><a href="#key-value">key-value</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="file-param">file-param</span><span class="grammar-usedby">(used by <a href="#multipart-form-data-param">multipart-form-data-param</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<a href="#key-string">key-string</a>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#file-value">file-value</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="file-value">file-value</span><span class="grammar-usedby">(used by <a href="#file-param">file-param</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">file,</span>&nbsp;<a href="#filename">filename</a>&nbsp;<span class="grammar-literal">;</span>&nbsp;<span class="grammar-symbol">(</span><a href="#file-contenttype">file-contenttype</a><span class="grammar-symbol">)</span><span class="grammar-symbol">?</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="file-contenttype">file-contenttype</span><span class="grammar-usedby">(used by <a href="#file-value">file-value</a>)</span></div><div class="grammar-rule-expression"><a href="#value-string">value-string</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="capture">capture</span><span class="grammar-usedby">(used by <a href="#captures-section">captures-section</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<a href="#key-string">key-string</a>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#query">query</a>&nbsp;<span class="grammar-symbol">(</span><a href="#sp">sp</a>&nbsp;<a href="#filter">filter</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span>&nbsp;<span class="grammar-symbol">(</span><a href="#sp">sp</a>&nbsp;<span class="grammar-literal">redact</span><span class="grammar-symbol">)</span><span class="grammar-symbol">?</span>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="assert">assert</span><span class="grammar-usedby">(used by <a href="#asserts-section">asserts-section</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<a href="#query">query</a>&nbsp;<span class="grammar-symbol">(</span><a href="#sp">sp</a>&nbsp;<a href="#filter">filter</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#predicate">predicate</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="option">option</span><span class="grammar-usedby">(used by <a href="#options-section">options-section</a>)</span></div><div class="grammar-rule-expression"><a href="#lt">lt</a><span class="grammar-symbol">*</span><br>
<span class="grammar-symbol">(</span><a href="#aws-sigv4-option">aws-sigv4-option</a><span class="grammar-symbol">|</span><a href="#ca-certificate-option">ca-certificate-option</a><span class="grammar-symbol">|</span><a href="#client-certificate-option">client-certificate-option</a><span class="grammar-symbol">|</span><a href="#client-key-option">client-key-option</a><span class="grammar-symbol">|</span><a href="#compressed-option">compressed-option</a><span class="grammar-symbol">|</span><a href="#connect-to-option">connect-to-option</a><span class="grammar-symbol">|</span><a href="#connect-timeout-option">connect-timeout-option</a><span class="grammar-symbol">|</span><a href="#delay-option">delay-option</a><span class="grammar-symbol">|</span><a href="#follow-redirect-option">follow-redirect-option</a><span class="grammar-symbol">|</span><a href="#follow-redirect-trusted-option">follow-redirect-trusted-option</a><span class="grammar-symbol">|</span><a href="#header-option">header-option</a><span class="grammar-symbol">|</span><a href="#http10-option">http10-option</a><span class="grammar-symbol">|</span><a href="#http11-option">http11-option</a><span class="grammar-symbol">|</span><a href="#http2-option">http2-option</a><span class="grammar-symbol">|</span><a href="#http3-option">http3-option</a><span class="grammar-symbol">|</span><a href="#insecure-option">insecure-option</a><span class="grammar-symbol">|</span><a href="#ipv4-option">ipv4-option</a><span class="grammar-symbol">|</span><a href="#ipv6-option">ipv6-option</a><span class="grammar-symbol">|</span><a href="#limit-rate-option">limit-rate-option</a><span class="grammar-symbol">|</span><a href="#max-redirs-option">max-redirs-option</a><span class="grammar-symbol">|</span><a href="#netrc-option">netrc-option</a><span class="grammar-symbol">|</span><a href="#netrc-file-option">netrc-file-option</a><span class="grammar-symbol">|</span><a href="#netrc-optional-option">netrc-optional-option</a><span class="grammar-symbol">|</span><a href="#output-option">output-option</a><span class="grammar-symbol">|</span><a href="#path-as-is-option">path-as-is-option</a><span class="grammar-symbol">|</span><a href="#proxy-option">proxy-option</a><span class="grammar-symbol">|</span><a href="#repeat-option">repeat-option</a><span class="grammar-symbol">|</span><a href="#resolve-option">resolve-option</a><span class="grammar-symbol">|</span><a href="#retry-option">retry-option</a><span class="grammar-symbol">|</span><a href="#retry-interval-option">retry-interval-option</a><span class="grammar-symbol">|</span><a href="#skip-option">skip-option</a><span class="grammar-symbol">|</span><a href="#unix-socket-option">unix-socket-option</a><span class="grammar-symbol">|</span><a href="#user-option">user-option</a><span class="grammar-symbol">|</span><a href="#variable-option">variable-option</a><span class="grammar-symbol">|</span><a href="#verbose-option">verbose-option</a><span class="grammar-symbol">|</span><a href="#very-verbose-option">very-verbose-option</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="aws-sigv4-option">aws-sigv4-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">aws-sigv4</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#value-string">value-string</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="ca-certificate-option">ca-certificate-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">cacert</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#filename">filename</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="client-certificate-option">client-certificate-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">cert</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#filename-password">filename-password</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="client-key-option">client-key-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">key</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#value-string">value-string</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="compressed-option">compressed-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">compressed</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="connect-to-option">connect-to-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">connect-to</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#value-string">value-string</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="connect-timeout-option">connect-timeout-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">connect-timeout</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#duration-option">duration-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="delay-option">delay-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">delay</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#duration-option">duration-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="follow-redirect-option">follow-redirect-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">location</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="follow-redirect-trusted-option">follow-redirect-trusted-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">location-trusted</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="header-option">header-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">header</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#value-string">value-string</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="http10-option">http10-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">http1.0</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="http11-option">http11-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">http1.1</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="http2-option">http2-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">http2</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="http3-option">http3-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">http3</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="insecure-option">insecure-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">insecure</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="ipv4-option">ipv4-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">ipv4</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="ipv6-option">ipv6-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">ipv6</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="limit-rate-option">limit-rate-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">limit-rate</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#integer-option">integer-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="max-redirs-option">max-redirs-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">max-redirs</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#integer-option">integer-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="netrc-option">netrc-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">netrc</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="netrc-file-option">netrc-file-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">netrc-file</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#value-string">value-string</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="netrc-optional-option">netrc-optional-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">netrc-optional</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="output-option">output-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">output</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#value-string">value-string</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="path-as-is-option">path-as-is-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">path-as-is</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="proxy-option">proxy-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">proxy</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#value-string">value-string</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="resolve-option">resolve-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">resolve</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#value-string">value-string</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="repeat-option">repeat-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">repeat</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#integer-option">integer-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="retry-option">retry-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">retry</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#integer-option">integer-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="retry-interval-option">retry-interval-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">retry-interval</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#duration-option">duration-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="skip-option">skip-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">skip</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="unix-socket-option">unix-socket-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">unix-socket</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#value-string">value-string</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="user-option">user-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">user</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#value-string">value-string</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="variable-option">variable-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">variable</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#variable-definition">variable-definition</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="verbose-option">verbose-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">verbose</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="very-verbose-option">very-verbose-option</span><span class="grammar-usedby">(used by <a href="#option">option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">very-verbose</span>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#boolean-option">boolean-option</a>&nbsp;<a href="#lt">lt</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="variable-definition">variable-definition</span><span class="grammar-usedby">(used by <a href="#variable-option">variable-option</a>)</span></div><div class="grammar-rule-expression"><a href="#variable-name">variable-name</a>&nbsp;<span class="grammar-literal">=</span>&nbsp;<a href="#variable-value">variable-value</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="boolean-option">boolean-option</span><span class="grammar-usedby">(used by <a href="#compressed-option">compressed-option</a>,&nbsp;<a href="#follow-redirect-option">follow-redirect-option</a>,&nbsp;<a href="#follow-redirect-trusted-option">follow-redirect-trusted-option</a>,&nbsp;<a href="#http10-option">http10-option</a>,&nbsp;<a href="#http11-option">http11-option</a>,&nbsp;<a href="#http2-option">http2-option</a>,&nbsp;<a href="#http3-option">http3-option</a>,&nbsp;<a href="#insecure-option">insecure-option</a>,&nbsp;<a href="#ipv4-option">ipv4-option</a>,&nbsp;<a href="#ipv6-option">ipv6-option</a>,&nbsp;<a href="#netrc-option">netrc-option</a>,&nbsp;<a href="#netrc-optional-option">netrc-optional-option</a>,&nbsp;<a href="#path-as-is-option">path-as-is-option</a>,&nbsp;<a href="#skip-option">skip-option</a>,&nbsp;<a href="#verbose-option">verbose-option</a>,&nbsp;<a href="#very-verbose-option">very-verbose-option</a>)</span></div><div class="grammar-rule-expression"><a href="#boolean">boolean</a><span class="grammar-symbol">|</span><a href="#placeholder">placeholder</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="integer-option">integer-option</span><span class="grammar-usedby">(used by <a href="#limit-rate-option">limit-rate-option</a>,&nbsp;<a href="#max-redirs-option">max-redirs-option</a>,&nbsp;<a href="#repeat-option">repeat-option</a>,&nbsp;<a href="#retry-option">retry-option</a>)</span></div><div class="grammar-rule-expression"><a href="#integer">integer</a><span class="grammar-symbol">|</span><a href="#placeholder">placeholder</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="duration-option">duration-option</span><span class="grammar-usedby">(used by <a href="#connect-timeout-option">connect-timeout-option</a>,&nbsp;<a href="#delay-option">delay-option</a>,&nbsp;<a href="#retry-interval-option">retry-interval-option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#integer">integer</a>&nbsp;<a href="#duration-unit">duration-unit</a><span class="grammar-symbol">?</span><span class="grammar-symbol">)</span><span class="grammar-symbol">|</span><a href="#placeholder">placeholder</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="duration-unit">duration-unit</span><span class="grammar-usedby">(used by <a href="#duration-option">duration-option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">ms</span><span class="grammar-symbol">|</span><span class="grammar-literal">s</span><span class="grammar-symbol">|</span><span class="grammar-literal">m</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="variable-value">variable-value</span><span class="grammar-usedby">(used by <a href="#variable-definition">variable-definition</a>)</span></div><div class="grammar-rule-expression">&nbsp;<a href="#null">null</a><br>
<span class="grammar-symbol">|</span><a href="#boolean">boolean</a><br>
<span class="grammar-symbol">|</span><a href="#integer">integer</a><br>
<span class="grammar-symbol">|</span><a href="#float">float</a><br>
<span class="grammar-symbol">|</span><a href="#key-string">key-string</a><br>
<span class="grammar-symbol">|</span><a href="#quoted-string">quoted-string</a></div></div>
</div><div class="grammar-ruleset"><h3 id="query">Query</h3><div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="query">query</span><span class="grammar-usedby">(used by <a href="#capture">capture</a>,&nbsp;<a href="#assert">assert</a>)</span></div><div class="grammar-rule-expression">&nbsp;<a href="#status-query">status-query</a><br>
<span class="grammar-symbol">|</span><a href="#version-query">version-query</a><br>
<span class="grammar-symbol">|</span><a href="#url-query">url-query</a><br>
<span class="grammar-symbol">|</span><a href="#ip-query">ip-query</a><br>
<span class="grammar-symbol">|</span><a href="#header-query">header-query</a><br>
<span class="grammar-symbol">|</span><a href="#certificate-query">certificate-query</a><br>
<span class="grammar-symbol">|</span><a href="#cookie-query">cookie-query</a><br>
<span class="grammar-symbol">|</span><a href="#body-query">body-query</a><br>
<span class="grammar-symbol">|</span><a href="#xpath-query">xpath-query</a><br>
<span class="grammar-symbol">|</span><a href="#jsonpath-query">jsonpath-query</a><br>
<span class="grammar-symbol">|</span><a href="#regex-query">regex-query</a><br>
<span class="grammar-symbol">|</span><a href="#variable-query">variable-query</a><br>
<span class="grammar-symbol">|</span><a href="#duration-query">duration-query</a><br>
<span class="grammar-symbol">|</span><a href="#bytes-query">bytes-query</a><br>
<span class="grammar-symbol">|</span><a href="#sha256-query">sha256-query</a><br>
<span class="grammar-symbol">|</span><a href="#md5-query">md5-query</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="status-query">status-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">status</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="version-query">version-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">version</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="url-query">url-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">url</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="ip-query">ip-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">ip</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="header-query">header-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">header</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#quoted-string">quoted-string</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="certificate-query">certificate-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">certificate</span>&nbsp;<a href="#sp">sp</a>&nbsp;<span class="grammar-symbol">(</span><span class="grammar-literal">Subject</span><span class="grammar-symbol">|</span><span class="grammar-literal">Issuer</span><span class="grammar-symbol">|</span><span class="grammar-literal">Start-Date</span><span class="grammar-symbol">|</span><span class="grammar-literal">Expire-Date</span><span class="grammar-symbol">|</span><span class="grammar-literal">Serial-Number</span><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="cookie-query">cookie-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">cookie</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#quoted-string">quoted-string</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="body-query">body-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">body</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="xpath-query">xpath-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">xpath</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#quoted-string">quoted-string</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="jsonpath-query">jsonpath-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">jsonpath</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#quoted-string">quoted-string</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="regex-query">regex-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">regex</span>&nbsp;<a href="#sp">sp</a>&nbsp;<span class="grammar-symbol">(</span><a href="#quoted-string">quoted-string</a><span class="grammar-symbol">|</span><a href="#regex">regex</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="variable-query">variable-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">variable</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#quoted-string">quoted-string</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="duration-query">duration-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">duration</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="sha256-query">sha256-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">sha256</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="md5-query">md5-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">md5</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="bytes-query">bytes-query</span><span class="grammar-usedby">(used by <a href="#query">query</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">bytes</span></div></div>
</div><div class="grammar-ruleset"><h3 id="predicates">Predicates</h3><div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="predicate">predicate</span><span class="grammar-usedby">(used by <a href="#assert">assert</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><span class="grammar-literal">not</span>&nbsp;<a href="#sp">sp</a><span class="grammar-symbol">)</span><span class="grammar-symbol">?</span>&nbsp;<a href="#predicate-func">predicate-func</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="predicate-func">predicate-func</span><span class="grammar-usedby">(used by <a href="#predicate">predicate</a>)</span></div><div class="grammar-rule-expression">&nbsp;<a href="#equal-predicate">equal-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#not-equal-predicate">not-equal-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#greater-predicate">greater-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#greater-or-equal-predicate">greater-or-equal-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#less-predicate">less-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#less-or-equal-predicate">less-or-equal-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#start-with-predicate">start-with-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#end-with-predicate">end-with-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#contain-predicate">contain-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#match-predicate">match-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#exist-predicate">exist-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#is-empty-predicate">is-empty-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#include-predicate">include-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#integer-predicate">integer-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#float-predicate">float-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#boolean-predicate">boolean-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#string-predicate">string-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#collection-predicate">collection-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#date-predicate">date-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#iso-date-predicate">iso-date-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#is-ipv4-predicate">is-ipv4-predicate</a><br>
<span class="grammar-symbol">|</span><a href="#is-ipv6-predicate">is-ipv6-predicate</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="equal-predicate">equal-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">==</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#predicate-value">predicate-value</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="not-equal-predicate">not-equal-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">!=</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#predicate-value">predicate-value</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="greater-predicate">greater-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">&gt;</span>&nbsp;<a href="#sp">sp</a>&nbsp;<span class="grammar-symbol">(</span><a href="#number">number</a><span class="grammar-symbol">|</span><a href="#quoted-string">quoted-string</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="greater-or-equal-predicate">greater-or-equal-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">&gt;=</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#sp">sp</a><span class="grammar-symbol">*</span>&nbsp;<span class="grammar-symbol">(</span><a href="#number">number</a><span class="grammar-symbol">|</span><a href="#quoted-string">quoted-string</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="less-predicate">less-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">&lt;</span>&nbsp;<a href="#sp">sp</a>&nbsp;<span class="grammar-symbol">(</span><a href="#number">number</a><span class="grammar-symbol">|</span><a href="#quoted-string">quoted-string</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="less-or-equal-predicate">less-or-equal-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">&lt;=</span>&nbsp;<a href="#sp">sp</a>&nbsp;<span class="grammar-symbol">(</span><a href="#number">number</a><span class="grammar-symbol">|</span><a href="#quoted-string">quoted-string</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="start-with-predicate">start-with-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">startsWith</span>&nbsp;<a href="#sp">sp</a>&nbsp;<span class="grammar-symbol">(</span><a href="#quoted-string">quoted-string</a><span class="grammar-symbol">|</span><a href="#oneline-hex">oneline-hex</a><span class="grammar-symbol">|</span><a href="#oneline-base64">oneline-base64</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="end-with-predicate">end-with-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">endsWith</span>&nbsp;<a href="#sp">sp</a>&nbsp;<span class="grammar-symbol">(</span><a href="#quoted-string">quoted-string</a><span class="grammar-symbol">|</span><a href="#oneline-hex">oneline-hex</a><span class="grammar-symbol">|</span><a href="#oneline-base64">oneline-base64</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="contain-predicate">contain-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">contains</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#quoted-string">quoted-string</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="match-predicate">match-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">matches</span>&nbsp;<a href="#sp">sp</a>&nbsp;<span class="grammar-symbol">(</span><a href="#quoted-string">quoted-string</a><span class="grammar-symbol">|</span><a href="#regex">regex</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="exist-predicate">exist-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">exists</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="is-empty-predicate">is-empty-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">isEmpty</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="include-predicate">include-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">includes</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#predicate-value">predicate-value</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="integer-predicate">integer-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">isInteger</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="float-predicate">float-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">isFloat</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="boolean-predicate">boolean-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">isBoolean</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="string-predicate">string-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">isString</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="collection-predicate">collection-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">isCollection</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="date-predicate">date-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">isDate</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="iso-date-predicate">iso-date-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">isIsoDate</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="is-ipv4-predicate">is-ipv4-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">isIpv4</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="is-ipv6-predicate">is-ipv6-predicate</span><span class="grammar-usedby">(used by <a href="#predicate-func">predicate-func</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">isIpv6</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="predicate-value">predicate-value</span><span class="grammar-usedby">(used by <a href="#equal-predicate">equal-predicate</a>,&nbsp;<a href="#not-equal-predicate">not-equal-predicate</a>,&nbsp;<a href="#include-predicate">include-predicate</a>)</span></div><div class="grammar-rule-expression">&nbsp;<a href="#boolean">boolean</a><br>
<span class="grammar-symbol">|</span><a href="#multiline-string">multiline-string</a><br>
<span class="grammar-symbol">|</span><a href="#null">null</a><br>
<span class="grammar-symbol">|</span><a href="#number">number</a><br>
<span class="grammar-symbol">|</span><a href="#oneline-string">oneline-string</a><br>
<span class="grammar-symbol">|</span><a href="#oneline-base64">oneline-base64</a><br>
<span class="grammar-symbol">|</span><a href="#oneline-file">oneline-file</a><br>
<span class="grammar-symbol">|</span><a href="#oneline-hex">oneline-hex</a><br>
<span class="grammar-symbol">|</span><a href="#quoted-string">quoted-string</a><br>
<span class="grammar-symbol">|</span><a href="#placeholder">placeholder</a></div></div>
</div><div class="grammar-ruleset"><h3 id="bytes">Bytes</h3><div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="bytes">bytes</span><span class="grammar-usedby">(used by <a href="#body">body</a>)</span></div><div class="grammar-rule-expression">&nbsp;<a href="#json-value">json-value</a><br>
<span class="grammar-symbol">|</span><a href="#xml">xml</a><br>
<span class="grammar-symbol">|</span><a href="#multiline-string">multiline-string</a><br>
<span class="grammar-symbol">|</span><a href="#oneline-string">oneline-string</a><br>
<span class="grammar-symbol">|</span><a href="#oneline-base64">oneline-base64</a><br>
<span class="grammar-symbol">|</span><a href="#oneline-file">oneline-file</a><br>
<span class="grammar-symbol">|</span><a href="#oneline-hex">oneline-hex</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="xml">xml</span><span class="grammar-usedby">(used by <a href="#bytes">bytes</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">&lt;</span>&nbsp;<span class="grammar-literal">To Be Defined</span>&nbsp;<span class="grammar-literal">&gt;</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="oneline-base64">oneline-base64</span><span class="grammar-usedby">(used by <a href="#start-with-predicate">start-with-predicate</a>,&nbsp;<a href="#end-with-predicate">end-with-predicate</a>,&nbsp;<a href="#predicate-value">predicate-value</a>,&nbsp;<a href="#bytes">bytes</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">base64,</span>&nbsp;<span class="grammar-regex">[A-Z0-9+-= \n]+</span>&nbsp;<span class="grammar-literal">;</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="oneline-file">oneline-file</span><span class="grammar-usedby">(used by <a href="#predicate-value">predicate-value</a>,&nbsp;<a href="#bytes">bytes</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">file,</span>&nbsp;<a href="#filename">filename</a>&nbsp;<span class="grammar-literal">;</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="oneline-hex">oneline-hex</span><span class="grammar-usedby">(used by <a href="#start-with-predicate">start-with-predicate</a>,&nbsp;<a href="#end-with-predicate">end-with-predicate</a>,&nbsp;<a href="#predicate-value">predicate-value</a>,&nbsp;<a href="#bytes">bytes</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">hex,</span>&nbsp;<a href="#hexdigit">hexdigit</a><span class="grammar-symbol">*</span>&nbsp;<span class="grammar-literal">;</span></div></div>
</div><div class="grammar-ruleset"><h3 id="strings">Strings</h3><div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="quoted-string">quoted-string</span><span class="grammar-usedby">(used by <a href="#variable-value">variable-value</a>,&nbsp;<a href="#header-query">header-query</a>,&nbsp;<a href="#cookie-query">cookie-query</a>,&nbsp;<a href="#xpath-query">xpath-query</a>,&nbsp;<a href="#jsonpath-query">jsonpath-query</a>,&nbsp;<a href="#regex-query">regex-query</a>,&nbsp;<a href="#variable-query">variable-query</a>,&nbsp;<a href="#greater-predicate">greater-predicate</a>,&nbsp;<a href="#greater-or-equal-predicate">greater-or-equal-predicate</a>,&nbsp;<a href="#less-predicate">less-predicate</a>,&nbsp;<a href="#less-or-equal-predicate">less-or-equal-predicate</a>,&nbsp;<a href="#start-with-predicate">start-with-predicate</a>,&nbsp;<a href="#end-with-predicate">end-with-predicate</a>,&nbsp;<a href="#contain-predicate">contain-predicate</a>,&nbsp;<a href="#match-predicate">match-predicate</a>,&nbsp;<a href="#predicate-value">predicate-value</a>,&nbsp;<a href="#jsonpath-filter">jsonpath-filter</a>,&nbsp;<a href="#regex-filter">regex-filter</a>,&nbsp;<a href="#replace-filter">replace-filter</a>,&nbsp;<a href="#split-filter">split-filter</a>,&nbsp;<a href="#xpath-filter">xpath-filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">"</span>&nbsp;<span class="grammar-symbol">(</span><a href="#quoted-string-content">quoted-string-content</a><span class="grammar-symbol">|</span><a href="#placeholder">placeholder</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span>&nbsp;<span class="grammar-literal">"</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="quoted-string-content">quoted-string-content</span><span class="grammar-usedby">(used by <a href="#quoted-string">quoted-string</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#quoted-string-text">quoted-string-text</a><span class="grammar-symbol">|</span><a href="#quoted-string-escaped-char">quoted-string-escaped-char</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="quoted-string-text">quoted-string-text</span><span class="grammar-usedby">(used by <a href="#quoted-string-content">quoted-string-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">~["\\]+</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="quoted-string-escaped-char">quoted-string-escaped-char</span><span class="grammar-usedby">(used by <a href="#quoted-string-content">quoted-string-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">\</span>&nbsp;<span class="grammar-symbol">(</span><span class="grammar-literal">"</span><span class="grammar-symbol">|</span><span class="grammar-literal">\</span><span class="grammar-symbol">|</span><span class="grammar-literal">\b</span><span class="grammar-symbol">|</span><span class="grammar-literal">\f</span><span class="grammar-symbol">|</span><span class="grammar-literal">\n</span><span class="grammar-symbol">|</span><span class="grammar-literal">\r</span><span class="grammar-symbol">|</span><span class="grammar-literal">\t</span><span class="grammar-symbol">|</span><span class="grammar-literal">\u</span>&nbsp;<a href="#unicode-char">unicode-char</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="key-string">key-string</span><span class="grammar-usedby">(used by <a href="#key-value">key-value</a>,&nbsp;<a href="#file-param">file-param</a>,&nbsp;<a href="#capture">capture</a>,&nbsp;<a href="#variable-value">variable-value</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#key-string-content">key-string-content</a><span class="grammar-symbol">|</span><a href="#placeholder">placeholder</a><span class="grammar-symbol">)</span><span class="grammar-symbol">+</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="key-string-content">key-string-content</span><span class="grammar-usedby">(used by <a href="#key-string">key-string</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#key-string-text">key-string-text</a><span class="grammar-symbol">|</span><a href="#key-string-escaped-char">key-string-escaped-char</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="key-string-text">key-string-text</span><span class="grammar-usedby">(used by <a href="#key-string-content">key-string-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#alphanum">alphanum</a><span class="grammar-symbol">|</span><span class="grammar-literal">_</span><span class="grammar-symbol">|</span><span class="grammar-literal">-</span><span class="grammar-symbol">|</span><span class="grammar-literal">.</span><span class="grammar-symbol">|</span><span class="grammar-literal">[</span><span class="grammar-symbol">|</span><span class="grammar-literal">]</span><span class="grammar-symbol">|</span><span class="grammar-literal">@</span><span class="grammar-symbol">|</span><span class="grammar-literal">$</span><span class="grammar-symbol">)</span><span class="grammar-symbol">+</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="key-string-escaped-char">key-string-escaped-char</span><span class="grammar-usedby">(used by <a href="#key-string-content">key-string-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">\</span>&nbsp;<span class="grammar-symbol">(</span><span class="grammar-literal">#</span><span class="grammar-symbol">|</span><span class="grammar-literal">:</span><span class="grammar-symbol">|</span><span class="grammar-literal">\</span><span class="grammar-symbol">|</span><span class="grammar-literal">\b</span><span class="grammar-symbol">|</span><span class="grammar-literal">\f</span><span class="grammar-symbol">|</span><span class="grammar-literal">\n</span><span class="grammar-symbol">|</span><span class="grammar-literal">\r</span><span class="grammar-symbol">|</span><span class="grammar-literal">\t</span><span class="grammar-symbol">|</span><span class="grammar-literal">\u</span>&nbsp;<a href="#unicode-char">unicode-char</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="value-string">value-string</span><span class="grammar-usedby">(used by <a href="#request">request</a>,&nbsp;<a href="#key-value">key-value</a>,&nbsp;<a href="#file-contenttype">file-contenttype</a>,&nbsp;<a href="#aws-sigv4-option">aws-sigv4-option</a>,&nbsp;<a href="#client-key-option">client-key-option</a>,&nbsp;<a href="#connect-to-option">connect-to-option</a>,&nbsp;<a href="#header-option">header-option</a>,&nbsp;<a href="#netrc-file-option">netrc-file-option</a>,&nbsp;<a href="#output-option">output-option</a>,&nbsp;<a href="#proxy-option">proxy-option</a>,&nbsp;<a href="#resolve-option">resolve-option</a>,&nbsp;<a href="#unix-socket-option">unix-socket-option</a>,&nbsp;<a href="#user-option">user-option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#value-string-content">value-string-content</a><span class="grammar-symbol">|</span><a href="#placeholder">placeholder</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="value-string-content">value-string-content</span><span class="grammar-usedby">(used by <a href="#value-string">value-string</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#value-string-text">value-string-text</a><span class="grammar-symbol">|</span><a href="#value-string-escaped-char">value-string-escaped-char</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="value-string-text">value-string-text</span><span class="grammar-usedby">(used by <a href="#value-string-content">value-string-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">~[#\n\\]+</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="value-string-escaped-char">value-string-escaped-char</span><span class="grammar-usedby">(used by <a href="#value-string-content">value-string-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">\</span>&nbsp;<span class="grammar-symbol">(</span><span class="grammar-literal">#</span><span class="grammar-symbol">|</span><span class="grammar-literal">\</span><span class="grammar-symbol">|</span><span class="grammar-literal">\b</span><span class="grammar-symbol">|</span><span class="grammar-literal">\f</span><span class="grammar-symbol">|</span><span class="grammar-literal">\n</span><span class="grammar-symbol">|</span><span class="grammar-literal">\r</span><span class="grammar-symbol">|</span><span class="grammar-literal">\t</span><span class="grammar-symbol">|</span><span class="grammar-literal">\u</span>&nbsp;<a href="#unicode-char">unicode-char</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="oneline-string">oneline-string</span><span class="grammar-usedby">(used by <a href="#predicate-value">predicate-value</a>,&nbsp;<a href="#bytes">bytes</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">`</span>&nbsp;<span class="grammar-symbol">(</span><a href="#oneline-string-content">oneline-string-content</a><span class="grammar-symbol">|</span><a href="#placeholder">placeholder</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span>&nbsp;<span class="grammar-literal">`</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="oneline-string-content">oneline-string-content</span><span class="grammar-usedby">(used by <a href="#oneline-string">oneline-string</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#oneline-string-text">oneline-string-text</a><span class="grammar-symbol">|</span><a href="#oneline-string-escaped-char">oneline-string-escaped-char</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="oneline-string-text">oneline-string-text</span><span class="grammar-usedby">(used by <a href="#oneline-string-content">oneline-string-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">~[#\n\\]</span>&nbsp;<span class="grammar-symbol">~</span><span class="grammar-literal">`</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="oneline-string-escaped-char">oneline-string-escaped-char</span><span class="grammar-usedby">(used by <a href="#oneline-string-content">oneline-string-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">\</span>&nbsp;<span class="grammar-symbol">(</span><span class="grammar-literal">`</span><span class="grammar-symbol">|</span><span class="grammar-literal">#</span><span class="grammar-symbol">|</span><span class="grammar-literal">\</span><span class="grammar-symbol">|</span><span class="grammar-literal">b</span><span class="grammar-symbol">|</span><span class="grammar-literal">f</span><span class="grammar-symbol">|</span><span class="grammar-literal">u</span>&nbsp;<a href="#unicode-char">unicode-char</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="multiline-string">multiline-string</span><span class="grammar-usedby">(used by <a href="#predicate-value">predicate-value</a>,&nbsp;<a href="#bytes">bytes</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal"></span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="multiline-string-type">multiline-string-type</span><span class="grammar-usedby">(used by <a href="#multiline-string">multiline-string</a>)</span></div><div class="grammar-rule-expression">&nbsp;<span class="grammar-literal">base64</span><br>
<span class="grammar-symbol">|</span><span class="grammar-literal">hex</span><br>
<span class="grammar-symbol">|</span><span class="grammar-literal">json</span><br>
<span class="grammar-symbol">|</span><span class="grammar-literal">xml</span><br>
<span class="grammar-symbol">|</span><span class="grammar-literal">graphql</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="multiline-string-attribute">multiline-string-attribute</span><span class="grammar-usedby">(used by <a href="#multiline-string">multiline-string</a>)</span></div><div class="grammar-rule-expression">&nbsp;<span class="grammar-literal">escape</span><br>
<span class="grammar-symbol">|</span><span class="grammar-literal">novariable</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="multiline-string-content">multiline-string-content</span><span class="grammar-usedby">(used by <a href="#multiline-string">multiline-string</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#multiline-string-text">multiline-string-text</a><span class="grammar-symbol">|</span><a href="#multiline-string-escaped-char">multiline-string-escaped-char</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="multiline-string-text">multiline-string-text</span><span class="grammar-usedby">(used by <a href="#multiline-string-content">multiline-string-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">~[\\]+</span>&nbsp;<span class="grammar-symbol">~</span><span class="grammar-literal">```</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="multiline-string-escaped-char">multiline-string-escaped-char</span><span class="grammar-usedby">(used by <a href="#multiline-string-content">multiline-string-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">\</span>&nbsp;<span class="grammar-symbol">(</span><span class="grammar-literal">\</span><span class="grammar-symbol">|</span><span class="grammar-literal">b</span><span class="grammar-symbol">|</span><span class="grammar-literal">f</span><span class="grammar-symbol">|</span><span class="grammar-literal">n</span><span class="grammar-symbol">|</span><span class="grammar-literal">r</span><span class="grammar-symbol">|</span><span class="grammar-literal">t</span><span class="grammar-symbol">|</span><span class="grammar-literal">`</span><span class="grammar-symbol">|</span><span class="grammar-literal">u</span>&nbsp;<a href="#unicode-char">unicode-char</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="filename">filename</span><span class="grammar-usedby">(used by <a href="#file-value">file-value</a>,&nbsp;<a href="#ca-certificate-option">ca-certificate-option</a>,&nbsp;<a href="#oneline-file">oneline-file</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#filename-content">filename-content</a><span class="grammar-symbol">|</span><a href="#placeholder">placeholder</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="filename-content">filename-content</span><span class="grammar-usedby">(used by <a href="#filename">filename</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#filename-text">filename-text</a><span class="grammar-symbol">|</span><a href="#filename-escaped-char">filename-escaped-char</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="filename-text">filename-text</span><span class="grammar-usedby">(used by <a href="#filename-content">filename-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">~[#;{} \n\\]+</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="filename-escaped-char">filename-escaped-char</span><span class="grammar-usedby">(used by <a href="#filename-content">filename-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">\</span>&nbsp;<span class="grammar-symbol">(</span><span class="grammar-literal">\</span><span class="grammar-symbol">|</span><span class="grammar-literal">b</span><span class="grammar-symbol">|</span><span class="grammar-literal">f</span><span class="grammar-symbol">|</span><span class="grammar-literal">n</span><span class="grammar-symbol">|</span><span class="grammar-literal">r</span><span class="grammar-symbol">|</span><span class="grammar-literal">t</span><span class="grammar-symbol">|</span><span class="grammar-literal">#</span><span class="grammar-symbol">|</span><span class="grammar-literal">;</span><span class="grammar-symbol">|</span><span class="grammar-literal"> </span><span class="grammar-symbol">|</span><span class="grammar-literal">{</span><span class="grammar-symbol">|</span><span class="grammar-literal">}</span><span class="grammar-symbol">|</span><span class="grammar-literal">u</span>&nbsp;<a href="#unicode-char">unicode-char</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="filename-password">filename-password</span><span class="grammar-usedby">(used by <a href="#client-certificate-option">client-certificate-option</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#filename-password-content">filename-password-content</a><span class="grammar-symbol">|</span><a href="#placeholder">placeholder</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="filename-password-content">filename-password-content</span><span class="grammar-usedby">(used by <a href="#filename-password">filename-password</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#filename-password-text">filename-password-text</a><span class="grammar-symbol">|</span><a href="#filename-password-escaped-char">filename-password-escaped-char</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="filename-password-text">filename-password-text</span><span class="grammar-usedby">(used by <a href="#filename-password-content">filename-password-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">~[#;{} \n\\]+</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="filename-password-escaped-char">filename-password-escaped-char</span><span class="grammar-usedby">(used by <a href="#filename-password-content">filename-password-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">\</span>&nbsp;<span class="grammar-symbol">(</span><span class="grammar-literal">\</span><span class="grammar-symbol">|</span><span class="grammar-literal">b</span><span class="grammar-symbol">|</span><span class="grammar-literal">f</span><span class="grammar-symbol">|</span><span class="grammar-literal">n</span><span class="grammar-symbol">|</span><span class="grammar-literal">r</span><span class="grammar-symbol">|</span><span class="grammar-literal">t</span><span class="grammar-symbol">|</span><span class="grammar-literal">#</span><span class="grammar-symbol">|</span><span class="grammar-literal">;</span><span class="grammar-symbol">|</span><span class="grammar-literal"> </span><span class="grammar-symbol">|</span><span class="grammar-literal">{</span><span class="grammar-symbol">|</span><span class="grammar-literal">}</span><span class="grammar-symbol">|</span><span class="grammar-literal">:</span><span class="grammar-symbol">|</span><span class="grammar-literal">u</span>&nbsp;<a href="#unicode-char">unicode-char</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="unicode-char">unicode-char</span><span class="grammar-usedby">(used by <a href="#quoted-string-escaped-char">quoted-string-escaped-char</a>,&nbsp;<a href="#key-string-escaped-char">key-string-escaped-char</a>,&nbsp;<a href="#value-string-escaped-char">value-string-escaped-char</a>,&nbsp;<a href="#oneline-string-escaped-char">oneline-string-escaped-char</a>,&nbsp;<a href="#multiline-string-escaped-char">multiline-string-escaped-char</a>,&nbsp;<a href="#filename-escaped-char">filename-escaped-char</a>,&nbsp;<a href="#filename-password-escaped-char">filename-password-escaped-char</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">{</span>&nbsp;<a href="#hexdigit">hexdigit</a><span class="grammar-symbol">+</span>&nbsp;<span class="grammar-literal">}</span></div></div>
</div><div class="grammar-ruleset"><h3 id="json">JSON</h3><div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="json-value">json-value</span><span class="grammar-usedby">(used by <a href="#bytes">bytes</a>,&nbsp;<a href="#json-key-value">json-key-value</a>,&nbsp;<a href="#json-array">json-array</a>)</span></div><div class="grammar-rule-expression">&nbsp;<a href="#placeholder">placeholder</a><br>
<span class="grammar-symbol">|</span><a href="#json-object">json-object</a><br>
<span class="grammar-symbol">|</span><a href="#json-array">json-array</a><br>
<span class="grammar-symbol">|</span><a href="#json-string">json-string</a><br>
<span class="grammar-symbol">|</span><a href="#json-number">json-number</a><br>
<span class="grammar-symbol">|</span><a href="#boolean">boolean</a><br>
<span class="grammar-symbol">|</span><a href="#null">null</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="json-object">json-object</span><span class="grammar-usedby">(used by <a href="#json-value">json-value</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">{</span>&nbsp;<a href="#json-key-value">json-key-value</a>&nbsp;<span class="grammar-symbol">(</span><span class="grammar-literal">,</span>&nbsp;<a href="#json-key-value">json-key-value</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span>&nbsp;<span class="grammar-literal">}</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="json-key-value">json-key-value</span><span class="grammar-usedby">(used by <a href="#json-object">json-object</a>)</span></div><div class="grammar-rule-expression"><a href="#json-string">json-string</a>&nbsp;<span class="grammar-literal">:</span>&nbsp;<a href="#json-value">json-value</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="json-array">json-array</span><span class="grammar-usedby">(used by <a href="#json-value">json-value</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">[</span>&nbsp;<a href="#json-value">json-value</a>&nbsp;<span class="grammar-symbol">(</span><span class="grammar-literal">,</span>&nbsp;<a href="#json-value">json-value</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span>&nbsp;<span class="grammar-literal">]</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="json-string">json-string</span><span class="grammar-usedby">(used by <a href="#json-value">json-value</a>,&nbsp;<a href="#json-key-value">json-key-value</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">"</span>&nbsp;<span class="grammar-symbol">(</span><a href="#json-string-content">json-string-content</a><span class="grammar-symbol">|</span><a href="#placeholder">placeholder</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span>&nbsp;<span class="grammar-literal">"</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="json-string-content">json-string-content</span><span class="grammar-usedby">(used by <a href="#json-string">json-string</a>)</span></div><div class="grammar-rule-expression"><a href="#json-string-text">json-string-text</a><span class="grammar-symbol">|</span><a href="#json-string-escaped-char">json-string-escaped-char</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="json-string-text">json-string-text</span><span class="grammar-usedby">(used by <a href="#json-string-content">json-string-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">~["\\]</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="json-string-escaped-char">json-string-escaped-char</span><span class="grammar-usedby">(used by <a href="#json-string-content">json-string-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">\</span>&nbsp;<span class="grammar-symbol">(</span><span class="grammar-literal">"</span><span class="grammar-symbol">|</span><span class="grammar-literal">\</span><span class="grammar-symbol">|</span><span class="grammar-literal">b</span><span class="grammar-symbol">|</span><span class="grammar-literal">f</span><span class="grammar-symbol">|</span><span class="grammar-literal">n</span><span class="grammar-symbol">|</span><span class="grammar-literal">r</span><span class="grammar-symbol">|</span><span class="grammar-literal">t</span><span class="grammar-symbol">|</span><span class="grammar-literal">u</span>&nbsp;<a href="#hexdigit">hexdigit</a>&nbsp;<a href="#hexdigit">hexdigit</a>&nbsp;<a href="#hexdigit">hexdigit</a>&nbsp;<a href="#hexdigit">hexdigit</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="json-number">json-number</span><span class="grammar-usedby">(used by <a href="#json-value">json-value</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">[-]?</span>&nbsp;<a href="#json-integer">json-integer</a>&nbsp;<a href="#fraction">fraction</a><span class="grammar-symbol">?</span>&nbsp;<a href="#exponent">exponent</a><span class="grammar-symbol">?</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="json-integer">json-integer</span><span class="grammar-usedby">(used by <a href="#json-number">json-number</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">0</span><span class="grammar-symbol">|</span><span class="grammar-regex">[1-9]</span>&nbsp;<a href="#digit">digit</a><span class="grammar-symbol">*</span></div></div>
</div><div class="grammar-ruleset"><h3 id="expression">Expression</h3><div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="placeholder">placeholder</span><span class="grammar-usedby">(used by <a href="#boolean-option">boolean-option</a>,&nbsp;<a href="#integer-option">integer-option</a>,&nbsp;<a href="#duration-option">duration-option</a>,&nbsp;<a href="#predicate-value">predicate-value</a>,&nbsp;<a href="#quoted-string">quoted-string</a>,&nbsp;<a href="#key-string">key-string</a>,&nbsp;<a href="#value-string">value-string</a>,&nbsp;<a href="#oneline-string">oneline-string</a>,&nbsp;<a href="#multiline-string">multiline-string</a>,&nbsp;<a href="#filename">filename</a>,&nbsp;<a href="#filename-password">filename-password</a>,&nbsp;<a href="#json-value">json-value</a>,&nbsp;<a href="#json-string">json-string</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">{{</span>&nbsp;<a href="#expr">expr</a>&nbsp;<span class="grammar-literal">}}</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="expr">expr</span><span class="grammar-usedby">(used by <a href="#placeholder">placeholder</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#variable-name">variable-name</a><span class="grammar-symbol">|</span><a href="#function">function</a><span class="grammar-symbol">)</span>&nbsp;<span class="grammar-symbol">(</span><a href="#sp">sp</a>&nbsp;<a href="#filter">filter</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="variable-name">variable-name</span><span class="grammar-usedby">(used by <a href="#variable-definition">variable-definition</a>,&nbsp;<a href="#expr">expr</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">[A-Za-z]</span>&nbsp;<span class="grammar-regex">[A-Za-z_-0-9]*</span></div></div>
</div><div class="grammar-ruleset"><h3 id="function">Function</h3><div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="function">function</span><span class="grammar-usedby">(used by <a href="#expr">expr</a>)</span></div><div class="grammar-rule-expression">&nbsp;<a href="#env-function">env-function</a><br>
<span class="grammar-symbol">|</span><a href="#now-function">now-function</a><br>
<span class="grammar-symbol">|</span><a href="#uuid-function">uuid-function</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="env-function">env-function</span><span class="grammar-usedby">(used by <a href="#function">function</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">getEnv</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="now-function">now-function</span><span class="grammar-usedby">(used by <a href="#function">function</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">newDate</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="uuid-function">uuid-function</span><span class="grammar-usedby">(used by <a href="#function">function</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">newUuid</span></div></div>
</div><div class="grammar-ruleset"><h3 id="filter">Filter</h3><div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="filter">filter</span><span class="grammar-usedby">(used by <a href="#capture">capture</a>,&nbsp;<a href="#assert">assert</a>,&nbsp;<a href="#expr">expr</a>)</span></div><div class="grammar-rule-expression">&nbsp;<a href="#base64-decode-filter">base64-decode-filter</a><br>
<span class="grammar-symbol">|</span><a href="#base64-encode-filter">base64-encode-filter</a><br>
<span class="grammar-symbol">|</span><a href="#count-filter">count-filter</a><br>
<span class="grammar-symbol">|</span><a href="#days-after-now-filter">days-after-now-filter</a><br>
<span class="grammar-symbol">|</span><a href="#days-before-now-filter">days-before-now-filter</a><br>
<span class="grammar-symbol">|</span><a href="#decode-filter">decode-filter</a><br>
<span class="grammar-symbol">|</span><a href="#format-filter">format-filter</a><br>
<span class="grammar-symbol">|</span><a href="#html-escape-filter">html-escape-filter</a><br>
<span class="grammar-symbol">|</span><a href="#html-unescape-filter">html-unescape-filter</a><br>
<span class="grammar-symbol">|</span><a href="#jsonpath-filter">jsonpath-filter</a><br>
<span class="grammar-symbol">|</span><a href="#nth-filter">nth-filter</a><br>
<span class="grammar-symbol">|</span><a href="#regex-filter">regex-filter</a><br>
<span class="grammar-symbol">|</span><a href="#replace-filter">replace-filter</a><br>
<span class="grammar-symbol">|</span><a href="#split-filter">split-filter</a><br>
<span class="grammar-symbol">|</span><a href="#to-date-filter">to-date-filter</a><br>
<span class="grammar-symbol">|</span><a href="#to-float-filter">to-float-filter</a><br>
<span class="grammar-symbol">|</span><a href="#to-int-filter">to-int-filter</a><br>
<span class="grammar-symbol">|</span><a href="#to-string-filter">to-string-filter</a><br>
<span class="grammar-symbol">|</span><a href="#url-decode-filter">url-decode-filter</a><br>
<span class="grammar-symbol">|</span><a href="#url-encode-filter">url-encode-filter</a><br>
<span class="grammar-symbol">|</span><a href="#xpath-filter">xpath-filter</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="base64-decode-filter">base64-decode-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">base64Decode</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="base64-encode-filter">base64-encode-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">base64Encode</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="count-filter">count-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">count</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="days-after-now-filter">days-after-now-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">daysAfterNow</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="days-before-now-filter">days-before-now-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">daysBeforeNow</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="decode-filter">decode-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">decode</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="format-filter">format-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">format</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="html-escape-filter">html-escape-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">htmlEscape</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="html-unescape-filter">html-unescape-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">htmlUnescape</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="jsonpath-filter">jsonpath-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">jsonpath</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#quoted-string">quoted-string</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="nth-filter">nth-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">nth</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#integer">integer</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="regex-filter">regex-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">regex</span>&nbsp;<a href="#sp">sp</a>&nbsp;<span class="grammar-symbol">(</span><a href="#quoted-string">quoted-string</a><span class="grammar-symbol">|</span><a href="#regex">regex</a><span class="grammar-symbol">)</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="replace-filter">replace-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">replace</span>&nbsp;<a href="#sp">sp</a>&nbsp;<span class="grammar-symbol">(</span><a href="#quoted-string">quoted-string</a><span class="grammar-symbol">|</span><a href="#regex">regex</a><span class="grammar-symbol">)</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#quoted-string">quoted-string</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="split-filter">split-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">split</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#quoted-string">quoted-string</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="to-date-filter">to-date-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">toDate</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="to-float-filter">to-float-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">toFloat</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="to-int-filter">to-int-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">toInt</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="to-string-filter">to-string-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">toString</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="url-decode-filter">url-decode-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">urlDecode</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="url-encode-filter">url-encode-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">urlEncode</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="xpath-filter">xpath-filter</span><span class="grammar-usedby">(used by <a href="#filter">filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">xpath</span>&nbsp;<a href="#sp">sp</a>&nbsp;<a href="#quoted-string">quoted-string</a></div></div>
</div><div class="grammar-ruleset"><h3 id="lexical-grammar">Lexical Grammar</h3><div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="boolean">boolean</span><span class="grammar-usedby">(used by <a href="#boolean-option">boolean-option</a>,&nbsp;<a href="#variable-value">variable-value</a>,&nbsp;<a href="#predicate-value">predicate-value</a>,&nbsp;<a href="#json-value">json-value</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">true</span><span class="grammar-symbol">|</span><span class="grammar-literal">false</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="null">null</span><span class="grammar-usedby">(used by <a href="#variable-value">variable-value</a>,&nbsp;<a href="#predicate-value">predicate-value</a>,&nbsp;<a href="#json-value">json-value</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">null</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="alphanum">alphanum</span><span class="grammar-usedby">(used by <a href="#key-string-text">key-string-text</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">[A-Za-z0-9]</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="integer">integer</span><span class="grammar-usedby">(used by <a href="#integer-option">integer-option</a>,&nbsp;<a href="#duration-option">duration-option</a>,&nbsp;<a href="#variable-value">variable-value</a>,&nbsp;<a href="#nth-filter">nth-filter</a>,&nbsp;<a href="#float">float</a>,&nbsp;<a href="#number">number</a>)</span></div><div class="grammar-rule-expression"><a href="#digit">digit</a><span class="grammar-symbol">+</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="float">float</span><span class="grammar-usedby">(used by <a href="#variable-value">variable-value</a>,&nbsp;<a href="#number">number</a>)</span></div><div class="grammar-rule-expression"><a href="#integer">integer</a>&nbsp;<a href="#fraction">fraction</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="number">number</span><span class="grammar-usedby">(used by <a href="#greater-predicate">greater-predicate</a>,&nbsp;<a href="#greater-or-equal-predicate">greater-or-equal-predicate</a>,&nbsp;<a href="#less-predicate">less-predicate</a>,&nbsp;<a href="#less-or-equal-predicate">less-or-equal-predicate</a>,&nbsp;<a href="#predicate-value">predicate-value</a>)</span></div><div class="grammar-rule-expression"><a href="#integer">integer</a><span class="grammar-symbol">|</span><a href="#float">float</a></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="digit">digit</span><span class="grammar-usedby">(used by <a href="#json-integer">json-integer</a>,&nbsp;<a href="#integer">integer</a>,&nbsp;<a href="#fraction">fraction</a>,&nbsp;<a href="#exponent">exponent</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">[0-9]</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="hexdigit">hexdigit</span><span class="grammar-usedby">(used by <a href="#oneline-hex">oneline-hex</a>,&nbsp;<a href="#unicode-char">unicode-char</a>,&nbsp;<a href="#json-string-escaped-char">json-string-escaped-char</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">[0-9A-Fa-f]</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="fraction">fraction</span><span class="grammar-usedby">(used by <a href="#json-number">json-number</a>,&nbsp;<a href="#float">float</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">.</span>&nbsp;<a href="#digit">digit</a><span class="grammar-symbol">+</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="exponent">exponent</span><span class="grammar-usedby">(used by <a href="#json-number">json-number</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><span class="grammar-literal">e</span><span class="grammar-symbol">|</span><span class="grammar-literal">E</span><span class="grammar-symbol">)</span>&nbsp;<span class="grammar-symbol">(</span><span class="grammar-literal">+</span><span class="grammar-symbol">|</span><span class="grammar-literal">-</span><span class="grammar-symbol">)</span><span class="grammar-symbol">?</span>&nbsp;<a href="#digit">digit</a><span class="grammar-symbol">+</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="sp">sp</span><span class="grammar-usedby">(used by <a href="#request">request</a>,&nbsp;<a href="#response">response</a>,&nbsp;<a href="#capture">capture</a>,&nbsp;<a href="#assert">assert</a>,&nbsp;<a href="#header-query">header-query</a>,&nbsp;<a href="#certificate-query">certificate-query</a>,&nbsp;<a href="#cookie-query">cookie-query</a>,&nbsp;<a href="#xpath-query">xpath-query</a>,&nbsp;<a href="#jsonpath-query">jsonpath-query</a>,&nbsp;<a href="#regex-query">regex-query</a>,&nbsp;<a href="#variable-query">variable-query</a>,&nbsp;<a href="#predicate">predicate</a>,&nbsp;<a href="#equal-predicate">equal-predicate</a>,&nbsp;<a href="#not-equal-predicate">not-equal-predicate</a>,&nbsp;<a href="#greater-predicate">greater-predicate</a>,&nbsp;<a href="#greater-or-equal-predicate">greater-or-equal-predicate</a>,&nbsp;<a href="#less-predicate">less-predicate</a>,&nbsp;<a href="#less-or-equal-predicate">less-or-equal-predicate</a>,&nbsp;<a href="#start-with-predicate">start-with-predicate</a>,&nbsp;<a href="#end-with-predicate">end-with-predicate</a>,&nbsp;<a href="#contain-predicate">contain-predicate</a>,&nbsp;<a href="#match-predicate">match-predicate</a>,&nbsp;<a href="#include-predicate">include-predicate</a>,&nbsp;<a href="#expr">expr</a>,&nbsp;<a href="#jsonpath-filter">jsonpath-filter</a>,&nbsp;<a href="#nth-filter">nth-filter</a>,&nbsp;<a href="#regex-filter">regex-filter</a>,&nbsp;<a href="#replace-filter">replace-filter</a>,&nbsp;<a href="#split-filter">split-filter</a>,&nbsp;<a href="#xpath-filter">xpath-filter</a>,&nbsp;<a href="#lt">lt</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">[ \t]</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="lt">lt</span><span class="grammar-usedby">(used by <a href="#hurl-file">hurl-file</a>,&nbsp;<a href="#request">request</a>,&nbsp;<a href="#response">response</a>,&nbsp;<a href="#header">header</a>,&nbsp;<a href="#body">body</a>,&nbsp;<a href="#query-string-params-section">query-string-params-section</a>,&nbsp;<a href="#form-params-section">form-params-section</a>,&nbsp;<a href="#multipart-form-data-section">multipart-form-data-section</a>,&nbsp;<a href="#cookies-section">cookies-section</a>,&nbsp;<a href="#captures-section">captures-section</a>,&nbsp;<a href="#asserts-section">asserts-section</a>,&nbsp;<a href="#basic-auth-section">basic-auth-section</a>,&nbsp;<a href="#options-section">options-section</a>,&nbsp;<a href="#file-param">file-param</a>,&nbsp;<a href="#capture">capture</a>,&nbsp;<a href="#assert">assert</a>,&nbsp;<a href="#option">option</a>,&nbsp;<a href="#aws-sigv4-option">aws-sigv4-option</a>,&nbsp;<a href="#ca-certificate-option">ca-certificate-option</a>,&nbsp;<a href="#client-certificate-option">client-certificate-option</a>,&nbsp;<a href="#client-key-option">client-key-option</a>,&nbsp;<a href="#compressed-option">compressed-option</a>,&nbsp;<a href="#connect-to-option">connect-to-option</a>,&nbsp;<a href="#connect-timeout-option">connect-timeout-option</a>,&nbsp;<a href="#delay-option">delay-option</a>,&nbsp;<a href="#follow-redirect-option">follow-redirect-option</a>,&nbsp;<a href="#follow-redirect-trusted-option">follow-redirect-trusted-option</a>,&nbsp;<a href="#header-option">header-option</a>,&nbsp;<a href="#http10-option">http10-option</a>,&nbsp;<a href="#http11-option">http11-option</a>,&nbsp;<a href="#http2-option">http2-option</a>,&nbsp;<a href="#http3-option">http3-option</a>,&nbsp;<a href="#insecure-option">insecure-option</a>,&nbsp;<a href="#ipv4-option">ipv4-option</a>,&nbsp;<a href="#ipv6-option">ipv6-option</a>,&nbsp;<a href="#limit-rate-option">limit-rate-option</a>,&nbsp;<a href="#max-redirs-option">max-redirs-option</a>,&nbsp;<a href="#netrc-option">netrc-option</a>,&nbsp;<a href="#netrc-file-option">netrc-file-option</a>,&nbsp;<a href="#netrc-optional-option">netrc-optional-option</a>,&nbsp;<a href="#output-option">output-option</a>,&nbsp;<a href="#path-as-is-option">path-as-is-option</a>,&nbsp;<a href="#proxy-option">proxy-option</a>,&nbsp;<a href="#resolve-option">resolve-option</a>,&nbsp;<a href="#repeat-option">repeat-option</a>,&nbsp;<a href="#retry-option">retry-option</a>,&nbsp;<a href="#retry-interval-option">retry-interval-option</a>,&nbsp;<a href="#skip-option">skip-option</a>,&nbsp;<a href="#unix-socket-option">unix-socket-option</a>,&nbsp;<a href="#user-option">user-option</a>,&nbsp;<a href="#variable-option">variable-option</a>,&nbsp;<a href="#verbose-option">verbose-option</a>,&nbsp;<a href="#very-verbose-option">very-verbose-option</a>,&nbsp;<a href="#multiline-string">multiline-string</a>)</span></div><div class="grammar-rule-expression"><a href="#sp">sp</a><span class="grammar-symbol">*</span>&nbsp;<a href="#comment">comment</a><span class="grammar-symbol">?</span>&nbsp;<span class="grammar-regex">[\n]?</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="comment">comment</span><span class="grammar-usedby">(used by <a href="#lt">lt</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">#</span>&nbsp;<span class="grammar-regex">~[\n]*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="regex">regex</span><span class="grammar-usedby">(used by <a href="#regex-query">regex-query</a>,&nbsp;<a href="#match-predicate">match-predicate</a>,&nbsp;<a href="#regex-filter">regex-filter</a>,&nbsp;<a href="#replace-filter">replace-filter</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">/</span>&nbsp;<a href="#regex-content">regex-content</a>&nbsp;<span class="grammar-literal">/</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="regex-content">regex-content</span><span class="grammar-usedby">(used by <a href="#regex">regex</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-symbol">(</span><a href="#regex-text">regex-text</a><span class="grammar-symbol">|</span><a href="#regex-escaped-char">regex-escaped-char</a><span class="grammar-symbol">)</span><span class="grammar-symbol">*</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="regex-text">regex-text</span><span class="grammar-usedby">(used by <a href="#regex-content">regex-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-regex">~[\n\/]+</span></div></div>
<div class="grammar-rule"><div class="grammar-rule-declaration"><span class="grammar-rule-id" id="regex-escaped-char">regex-escaped-char</span><span class="grammar-usedby">(used by <a href="#regex-content">regex-content</a>)</span></div><div class="grammar-rule-expression"><span class="grammar-literal">\</span>&nbsp;<span class="grammar-regex">~[\n]</span></div></div>
</div>

**Examples:**

Example 1 (unknown):
```unknown
In this second example, we capture the body in a variable `index`, and reuse this value in the query
`jsonpath "$.errors[{{index}}].id"`:
```

Example 2 (unknown):
```unknown
### Functions {#file-format-templates-functions}

Besides variables, functions can be used to generate dynamic values. Current functions are:

| Function    | Description                                                    |
|-------------|----------------------------------------------------------------|
| `newUuid`   | Generates an [UUID v4 random string](https://en.wikipedia.org/wiki/Universally_unique_identifier)                           |
| `newDate`   | Generates an [RFC 3339](https://www.rfc-editor.org/rfc/rfc3339) UTC date string, at the current time   |

In the following example, we use `newDate` to generate a dynamic query parameter:
```

Example 3 (unknown):
```unknown
We run a `GET` request to `https://example.org/api/foo?date=2024%2D12%2D02T10%3A35%3A44%2E461731Z` where the `date`
query parameter value is `2024-12-02T10:35:44.461731Z` URL encoded.

In this second example, we use `newUuid` function to generate an email dynamically:
```

Example 4 (unknown):
```unknown
When run, the request body will be:
```

---

## Send an authorized request:

**URL:** llms-txt#send-an-authorized-request:

**Contents:**
- Running Tests {#getting-started-running-tests-running-tests}
  - Use --test Option {#getting-started-running-tests-use-test-option}
  - Debugging {#getting-started-running-tests-debugging}
  - Generating Report {#getting-started-running-tests-generating-report}
  - Use Variables in Tests {#getting-started-running-tests-use-variables-in-tests}
- Frequently Asked Questions {#getting-started-frequently-asked-questions-frequently-asked-questions}
  - General {#getting-started-frequently-asked-questions-general}

POST https://example.org
X-Token: {{token}}
{
  "name": "Alice",
  "value": 100
}
HTTP 200
hurl
GET https://example.org/data.bin
HTTP 200
[Asserts]
bytes startsWith hex,efbbbf;
hurl
POST https://sts.eu-central-1.amazonaws.com/
[Options]
aws-sigv4: aws:amz:eu-central-1:sts
[Form]
Action: GetCallerIdentity
Version: 2011-06-15
hurl
POST https://sts.eu-central-1.amazonaws.com/
[Options]
aws-sigv4: aws:amz:eu-central-1:sts
user: bob=secret
[Form]
Action: GetCallerIdentity
Version: 2011-06-15
shell
$ hurl --resolve foo.com:8000:127.0.0.1 foo.hurl
hurl
GET http://bar.com
HTTP 200

GET http://foo.com:8000/resolve
[Options]
resolve: foo.com:8000:127.0.0.1
HTTP 200
`Hello World!`
shell
$ hurl hello.hurl
Hello World!
shell
$ hurl hello.hurl assert_json.hurl
Hello World![
  { "id": 1, "name": "Bob"},
  { "id": 2, "name": "Bill"}
]
shell
$ hurl --test hello.hurl assert_json.hurl
[1mhello.hurl[0m: [1;32mSuccess[0m (6 request(s) in 245 ms)
[1massert_json.hurl[0m: [1;32mSuccess[0m (8 request(s) in 308 ms)
--------------------------------------------------------------------------------
Executed files:    2
Executed requests: 10 (17.82/s)
Succeeded files:   2 (100.0%)
Failed files:      0 (0.0%)
Duration:          561 ms
shell
$ hurl --test hello.hurl error_assert_status.hurl
[1mhello.hurl[0m: [1;32mSuccess[0m (4 request(s) in 5 ms)
[1;31merror[0m: [1mAssert status code[0m
  [1;34m-->[0m error_assert_status.hurl:9:6
[1;34m   |[0m
[1;34m   |[0m [90mGET http://localhost:8000/not_found[0m
[1;34m   |[0m[90m ...[0m
[1;34m 9 |[0m HTTP 200
[1;34m   |[0m[1;31m      ^^^ actual value is <404>[0m
[1;34m   |[0m

[1merror_assert_status.hurl[0m: [1;31mFailure[0m (1 request(s) in 2 ms)
--------------------------------------------------------------------------------
Executed files:    2
Executed requests: 5 (500.0/s)
Succeeded files:   1 (50.0%)
Failed files:      1 (50.0%)
Duration:          10 ms
shell
$ hurl --test --jobs 1 *.hurl
shell
$ hurl --test --repeat 1000 stress.hurl
shell
$ hurl --test test/integration/a.hurl test/integration/b.hurl test/integration/c.hurl
shell
$ hurl --test test/integration/*.hurl
shell
$ hurl --test test/integration/
shell
$ hurl --test --glob "test/integration/**/*.hurl"
shell
$ hurl --test --error-format long hello.hurl error_assert_status.hurl
[1mhello.hurl[0m: [1;32mSuccess[0m (4 request(s) in 6 ms)
[1;32mHTTP/1.1 404
[0m[1;36mServer[0m: Werkzeug/3.0.3 Python/3.12.4
[1;36mDate[0m: Wed, 10 Jul 2024 15:42:41 GMT
[1;36mContent-Type[0m: text/html; charset=utf-8
[1;36mContent-Length[0m: 207
[1;36mServer[0m: Flask Server
[1;36mConnection[0m: close

<!doctype html>
<html lang=en>
<title>404 Not Found</title>
<h1>Not Found</h1>
<p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p>

[1;31merror[0m: [1mAssert status code[0m
  [1;34m-->[0m error_assert_status.hurl:9:6
[1;34m   |[0m
[1;34m   |[0m [90mGET http://localhost:8000/not_found[0m
[1;34m   |[0m[90m ...[0m
[1;34m 9 |[0m HTTP 200
[1;34m   |[0m[1;31m      ^^^ actual value is <404>[0m
[1;34m   |[0m

[1merror_assert_status.hurl[0m: [1;31mFailure[0m (1 request(s) in 2 ms)
--------------------------------------------------------------------------------
Executed files:    2
Executed requests: 5 (454.5/s)
Succeeded files:   1 (50.0%)
Failed files:      1 (50.0%)
Duration:          11 ms
hurl
GET https://foo.com
HTTP 200

GET https://bar.com
[Options]
very-verbose: true
HTTP 200

GET https://baz.com
HTTP 200
shell
$ hurl --test --very-verbose .
hurl
GET http://foo.com
HTTP 200

GET https://bar.com
[Options]
output: -
HTTP 200
shell
$ hurl --test .
<html><head><meta http-equiv="content-type" content="text/html;charset=utf-8">
<title>301 Moved</TITLE></head><body>
<h1>301 Moved</h1>
The document has moved
<a HREF="https://www.bar.com/">here</a>.
</body></html>
[1m/tmp/test.hurl[0m: [1;32mSuccess[0m (2 request(s) in 184 ms)
--------------------------------------------------------------------------------
Executed files:    1
Executed requests: 2 (10.7/s)
Succeeded files:   1 (100.0%)
Failed files:      0 (0.0%)
Duration:          187 ms

PUT http://localhost:3000/api/login
{
  "username": "xyz",
  "password": "xyz"
}

curl --header "Content-Type: application/json" \
     --request PUT \
     --data '{"username": "xyz","password": "xyz"}' \
     http://localhost:3000/api/login

Scenario: create and retrieve a cat

Given url 'http://myhost.com/v1/cats'
And request { name: 'Billie' }
When method post
Then status 201
And match response == { id: '#notnull', name: 'Billie }

Given path response.id
When method get
Then status 200

**Examples:**

Example 1 (unknown):
```unknown
[Doc](#file-format-capturing-response-redacting-secrets)

#### Checking Byte Order Mark (BOM) in Response Body {#getting-started-samples-checking-byte-order-mark-bom-in-response-body}
```

Example 2 (unknown):
```unknown
[Doc](#file-format-asserting-response-bytes-assert)

#### AWS Signature Version 4 Requests {#getting-started-samples-aws-signature-version-4-requests}

Generate signed API requests with [AWS Signature Version 4](https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html), as used by several cloud providers.
```

Example 3 (unknown):
```unknown
The Access Key is given per [`--user`](#getting-started-manual-user), either with command line option or within the [`[Options]`](#file-format-request-options) section:
```

Example 4 (unknown):
```unknown
[Doc](#getting-started-manual-aws-sigv4)

#### Using curl Options {#getting-started-samples-using-curl-options}

curl options (for instance [`--resolve`](#getting-started-manual-resolve) or [`--connect-to`](#getting-started-manual-connect-to)) can be used as CLI argument. In this case, they're applicable
to each request of an Hurl file.
```

---

## from one request to another:

**URL:** llms-txt#from-one-request-to-another:

---

## Some random comments before body

**URL:** llms-txt#some-random-comments-before-body

**Contents:**
- Response {#file-format-response-response}
  - Definition {#file-format-response-definition}
  - Example {#file-format-response-example}
  - Structure {#file-format-response-structure}
  - Capture and Assertion {#file-format-response-capture-and-assertion}
  - Timings {#file-format-response-timings}
- Capturing Response {#file-format-capturing-response-capturing-response}
  - Captures {#file-format-capturing-response-captures}

file,data.bin;
hurl
GET https://example.org
HTTP 200
Last-Modified: Wed, 21 Oct 2015 07:28:00 GMT
[Asserts]
xpath "normalize-space(//head/title)" startsWith "Welcome"
xpath "//li" count == 18
hurl

**Examples:**

Example 1 (unknown):
```unknown
File are relative to the input Hurl file, and cannot contain implicit parent directory (`..`). You can use
[`--file-root` option](#getting-started-manual-file-root) to specify the root directory of all file nodes.




<hr>

## Response {#file-format-response-response}

### Definition {#file-format-response-definition}

Responses can be used to capture values to perform subsequent requests, or add asserts to HTTP responses. Response on
requests are optional, a Hurl file can just consist of a sequence of [requests](#file-format-request).

A response describes the expected HTTP response, with mandatory [version and status](#file-format-asserting-response-version-status), followed by optional [headers](#file-format-asserting-response-headers),
[captures](#file-format-capturing-response-captures), [asserts](#file-format-asserting-response-asserts) and [body](#file-format-asserting-response-body). Assertions in the expected HTTP response describe values of the received HTTP response.
Captures capture values from the received HTTP response and populate a set of named variables that can be used
in the following entries.

### Example {#file-format-response-example}
```

Example 2 (unknown):
```unknown
### Structure {#file-format-response-structure}

<div class="hurl-structure-schema">
  <div class="hurl-structure">
    <div class="hurl-structure-col-0">
        <div class="hurl-part-0">
            HTTP 200
        </div>
        <div class=" hurl-part-1">
            content-length: 206<br>accept-ranges: bytes<br>user-agent: Test
        </div>
        <div class="hurl-part-2">
            [Captures]<br>...
        </div>
        <div class="hurl-part-2">
            [Asserts]<br>...
        </div>
        <div class="hurl-part-3">
            {<br>
            &nbsp;&nbsp;"type": "FOO",<br>
            &nbsp;&nbsp;"value": 356789,<br>
            &nbsp;&nbsp;"ordered": true,<br>
            &nbsp;&nbsp;"index": 10<br>
            }
        </div>
    </div>
    <div class="hurl-structure-col-1">
        <div class="hurl-request-explanation-part-0">
            <a href="#file-format-asserting-response-version-status">Version and status (mandatory if response present)</a>
        </div>
        <div class="hurl-request-explanation-part-1">
            <br><a href="#file-format-asserting-response-headers">HTTP response headers</a> (optional)
        </div>
        <div class="hurl-request-explanation-part-2">
            <br>
            <br>
        </div>
        <div class="hurl-request-explanation-part-2">
            <a href="#file-format-capturing-response-capturing-response">Captures</a> and <a href="#file-format-asserting-response-asserts">asserts</a> (optional sections, unordered)
        </div>
        <div class="hurl-request-explanation-part-2">
          <br>
          <br>
          <br>
          <br>
        </div>
        <div class="hurl-request-explanation-part-3">
            <a href="#file-format-asserting-response-body">HTTP response body</a> (optional)
        </div>
    </div>
</div>
</div>


### Capture and Assertion {#file-format-response-capture-and-assertion}

With the response section, one can optionally [capture value from headers, body](#file-format-capturing-response), or [add assert on status code, body or headers](#file-format-asserting-response).

#### Body compression {#file-format-response-body-compression}

Hurl outputs the raw HTTP body to stdout by default. If response body is compressed (using [br, gzip, deflate](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Encoding)),
the binary stream is output, without any modification. One can use [`--compressed` option](#getting-started-manual-compressed)
to request a compressed response and automatically get the decompressed body.

Captures and asserts work automatically on the decompressed body, so you can request compressed data (using [`Accept-Encoding`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Encoding)
header by example) and add assert and captures on the decoded body as if there weren't any compression.

### Timings {#file-format-response-timings}

HTTP response timings are exposed through Hurl structured output (see [`--json`](#getting-started-manual-json)), HTML report (see [`--report-html`](#getting-started-manual-report-html))
and JSON report (see [`--report-json`](#getting-started-manual-report-json)).

On each response, libcurl response timings are available:

- __time_namelookup__: the time it took from the start until the name resolving was completed. You can use
  [`--resolve`](#getting-started-manual-resolve) to exclude DNS performance from the measure.
- __time_connect__:  The time it took from the start until the TCP connect to the remote host (or proxy) was completed.
- __time_appconnect__: The time it took from the start until the SSL/SSH/etc connect/handshake to the remote host was
  completed. The client is then ready to send its HTTP GET request.
- __time_starttransfer__: The time it took from the start until the first byte was just about to be transferred
  (just before Hurl reads the first byte from the network). This includes time_pretransfer and also the time the server
  needed to calculate the result.
- __time_total__: The total time that the full operation lasted.

All timings are in microsecond.

<div class="picture">
    <img class="u-theme-light u-drop-shadow u-border u-max-width-100" src="https://hurl.dev/assets/img/timings-light.svg" alt="Response timings explanation"/>
    <img class="u-theme-dark u-drop-shadow u-border u-max-width-100" src="https://hurl.dev/assets/img/timings-dark.svg" alt="Response timings explanation"/>
    <a href="https://blog.cloudflare.com/a-question-of-timing/"><small>Courtesy of CloudFlare</small></a>
</div>





<hr>

## Capturing Response {#file-format-capturing-response-capturing-response}

### Captures {#file-format-capturing-response-captures}

Captures are optional values that are __extracted from the HTTP response__ and stored in a named variable.
These captures may be the response status code, part of or the entire the body, and response headers.

Captured variables can be accessed through a run session; each new value of a given variable overrides the last value.

Captures can be useful for using data from one request in another request, such as when working with [CSRF tokens](https://en.wikipedia.org/wiki/Cross-site_request_forgery).
Variables in a Hurl file can be created from captures or [injected into the session](#file-format-templates-injecting-variables).
```

---

## Create a new soapy thing XML body:

**URL:** llms-txt#create-a-new-soapy-thing-xml-body:

POST https://example.org/InStock
Content-Type: application/soap+xml; charset=utf-8
Content-Length: 299
SOAPAction: "http://www.w3.org/2003/05/soap-envelope"

> Contrary to JSON body, the succinct syntax of XML body can not use variables. If you need to use variables in your
> XML body, use a simple [multiline string body](#file-format-request-multiline-string-body) with variables.

##### GraphQL query {#file-format-request-graphql-query}

GraphQL query uses [multiline string body](#file-format-request-multiline-string-body) with `graphql` identifier:

~~~hurl
POST https://example.org/starwars/graphql

GraphQL query body can use [GraphQL variables](https://graphql.org/learn/queries/#variables):

~~~hurl
POST https://example.org/starwars/graphql

GraphQL query, as every multiline string body, can use Hurl variables.

~~~hurl
POST https://example.org/starwars/graphql

> Hurl variables and GraphQL variables can be mixed in the same body.

##### Multiline string body {#file-format-request-multiline-string-body}

For text based body that are neither JSON nor XML, one can use multiline string, started and ending with
<code>&#96;&#96;&#96;</code>.

~~~hurl
POST https://example.org/models

The standard usage of a multiline string is:

is evaluated as "line1\nline2\nline3\n".

Multiline string body can use language identifier, like `json`, `xml` or `graphql`. Depending on the language identifier,
an additional 'Content-Type' request header is sent, and the real body (bytes sent over the wire) can be different from the
raw multiline text.

~~~hurl
POST https://example.org/api/dogs

##### Oneline string body {#file-format-request-oneline-string-body}

For text based body that do not contain newlines, one can use oneline string, started and ending with <code>&#96;</code>.

~~~hurl
POST https://example.org/helloworld
`Hello world!`
~~~

##### Base64 body {#file-format-request-base64-body}

Base64 body is used to set binary data as the request body.

Base64 body starts with `base64,` and end with `;`. MIME's Base64 encoding is supported (newlines and white spaces may be
present anywhere but are to be ignored on decoding), and `=` padding characters might be added.

```hurl
POST https://example.org

**Examples:**

Example 1 (xml):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope" xmlns:m="http://example.net">
  <soap:Header></soap:Header>
  <soap:Body>
    <m:GetStockPrice>
      <m:StockName>GOOG</m:StockName>
    </m:GetStockPrice>
  </soap:Body>
</soap:Envelope>
```

Example 2 (graphql):
```graphql
{
  human(id: "1000") {
    name
    height(unit: FOOT)
  }
}
```

Example 3 (graphql):
```graphql
query Hero($episode: Episode, $withFriends: Boolean!) {
  hero(episode: $episode) {
    name
    friends @include(if: $withFriends) {
      name
    }
  }
}

variables {
  "episode": "JEDI",
  "withFriends": false
}
```

Example 4 (graphql):
```graphql
{
  human(id: "{{human_id}}") {
    name
    height(unit: FOOT)
  }
}
```

---

## Delaying this request by 5 seconds (aka sleep)

**URL:** llms-txt#delaying-this-request-by-5-seconds-(aka-sleep)

GET https://example.org/turtle
[Options]
delay: 5s
HTTP 200

---

## Content-Type has no encoding clue, we must decode ourselves the body response.

**URL:** llms-txt#content-type-has-no-encoding-clue,-we-must-decode-ourselves-the-body-response.

**Contents:**
- Templates {#file-format-templates-templates}
  - Variables {#file-format-templates-variables}

bytes decode "gb2312" xpath "string(//body)" == "ä½ å¥½ä¸ç"
hurl
GET https://example.org
HTTP 200
[Asserts]
cookie "LSID[Expires]" format "%a, %d %b %Y %H:%M:%S" == "Wed, 13 Jan 2021 22:23:01"
hurl
GET https://example.org/api
HTTP 200
[Asserts]
jsonpath "$.text" htmlEscape == "a &gt; b"
hurl
GET https://example.org/api
HTTP 200
[Asserts]
jsonpath "$.escaped_html[1]" htmlUnescape == "Foo Â© bar ð"
hurl
GET https://example.org/api
HTTP 200
[Captures]
books: xpath "string(//body/@data-books)"
[Asserts]
variable "books" jsonpath "$[0].name" == "Dune"
variable "books" jsonpath "$[0].author" == "Franck Herbert"
hurl
GET https://example.org/api
HTTP 200
[Asserts]
jsonpath "$.books" nth 2 == "Children of Dune"
hurl
GET https://example.org/foo
HTTP 200
[Captures]
param1: header "header1"
param2: header "header2" regex "Hello (.*)!"
param3: header "header2" regex /Hello (.*)!/
hurl
GET https://example.org/foo
HTTP 200
[Captures]
url: jsonpath "$.url" replace "http://" "https://"
[Asserts]
jsonpath "$.ips" replace ", " "|" == "192.168.2.1|10.0.0.20|10.0.0.10"
hurl
GET https://example.org/foo
HTTP 200
[Asserts]
jsonpath "$.ips" split ", " count == 3
hurl
GET https:///example.org
HTTP 200
[Asserts]
header "Expires" toDate "%a, %d %b %Y %H:%M:%S GMT" daysBeforeNow > 1000
hurl
GET https://example.org/api/books
HTTP 200
[Asserts]
jsonpath "$.published" == "2023-01-23T18:25:43.511Z"
jsonpath "$.published" toDate "%Y-%m-%dT%H:%M:%S%.fZ" format "%A" == "Monday"
jsonpath "$.published" toDate "%+" format "%A" == "Monday" # %+ can be used to parse ISO 8601 / RFC 3339
hurl
GET https://example.org/foo
HTTP 200
[Asserts]
jsonpath "$.pi" toFloat == 3.14
hurl
GET https://example.org/foo
HTTP 200
[Asserts]
jsonpath "$.id" toInt == 123
hurl
GET https://example.org/foo
HTTP 200
[Asserts]
jsonpath "$.count" toString == "42"
hurl
GET https://example.org/foo
HTTP 200
[Asserts]
jsonpath "$.encoded_url" urlDecode == "https://mozilla.org/?x=ÑÐµÐ»Ð»Ñ"
hurl
GET https://example.org/foo
HTTP 200
[Asserts]
jsonpath "$.url" urlEncode == "https%3A//mozilla.org/%3Fx%3D%D1%88%D0%B5%D0%BB%D0%BB%D1%8B"
hurl
GET https://example.org/hello_gb2312
HTTP 200
[Asserts]
bytes decode "gb2312" xpath "string(//body)" == "ä½ å¥½ä¸ç"
hurl
GET https://example.org
HTTP 200
[Captures]
csrf_token: xpath "string(//meta[@name='_csrf_token']/@content)"

**Examples:**

Example 1 (unknown):
```unknown
#### format {#file-format-filters-format}

Formats a date to a string given [a specification format](https://docs.rs/chrono/latest/chrono/format/strftime/index.html).
```

Example 2 (unknown):
```unknown
#### htmlEscape {#file-format-filters-htmlescape}

Converts the characters `&`, `<` and `>` to HTML-safe sequence.
```

Example 3 (unknown):
```unknown
#### htmlUnescape {#file-format-filters-htmlunescape}

Converts all named and numeric character references (e.g. `&gt;`, `&#62;`, `&#x3e;`) to the corresponding Unicode characters.
```

Example 4 (unknown):
```unknown
#### jsonpath  {#file-format-filters-jsonpath}

Evaluates a [JSONPath](https://goessner.net/articles/JsonPath/) expression.
```

---

## A test on response body

**URL:** llms-txt#a-test-on-response-body

GET https://foo.com
HTTP 200
[Asserts]
jsonpath "$.state" == "running"
hurl
GET https://example.org/order/435
HTTP 200
hurl
GET https://example.org/order/435

**Examples:**

Example 1 (unknown):
```unknown
#### Testing Status Code {#getting-started-samples-testing-status-code}
```

Example 2 (unknown):
```unknown
[Doc](#file-format-asserting-response-version-status)
```

---

## Create a new catty thing with JSON body:

**URL:** llms-txt#create-a-new-catty-thing-with-json-body:

POST https://example.org/api/cats
{
    "id": 42,
    "lives": {{ lives_count }},
    "name": "{{ name }}"
}
```

When using JSON request body, the content type `application/json` is automatically set.

JSON request body can be seen as syntactic sugar of [multiline string body](#file-format-request-multiline-string-body) with `json` identifier:

---

## Get a doggy thing:

**URL:** llms-txt#get-a-doggy-thing:

**Contents:**
- Filters {#file-format-filters-filters}
  - Definition {#file-format-filters-definition}
  - Example {#file-format-filters-example}
  - Description {#file-format-filters-description}

GET https://example.org/api/dogs/{{dog-id}}
HTTP 200

#### XML body {#file-format-asserting-response-xml-body}

~~~hurl
GET https://example.org/api/catalog
HTTP 200
<?xml version="1.0" encoding="UTF-8"?>
<catalog>
   <book id="bk101">
      <author>Gambardella, Matthew</author>
      <title>XML Developer's Guide</title>
      <genre>Computer</genre>
      <price>44.95</price>
      <publish_date>2000-10-01</publish_date>
      <description>An in-depth look at creating applications with XML.</description>
   </book>
</catalog>
~~~

XML response body can be seen as syntactic sugar of [multiline string body](#file-format-asserting-response-multiline-string-body) with `xml` identifier:

~~~hurl
GET https://example.org/api/catalog
HTTP 200

#### Multiline string body {#file-format-asserting-response-multiline-string-body}

~~~hurl
GET https://example.org/models
HTTP 200

The standard usage of a multiline string is :

##### Oneline string body {#file-format-asserting-response-oneline-string-body}

For text based response body that do not contain newlines, one can use oneline string, started and ending with <code>&#96;</code>.

~~~hurl
POST https://example.org/helloworld
HTTP 200
`Hello world!`
~~~

#### Base64 body {#file-format-asserting-response-base64-body}

Base64 response body assert starts with `base64,` and end with `;`. MIME's Base64 encoding is supported (newlines and
white spaces may be present anywhere but are to be ignored on decoding), and `=` padding characters might be added.

#### File body {#file-format-asserting-response-file-body}

To use the binary content of a local file as the body response assert, file body
can be used. File body starts with `file,` and ends with `;``

File are relative to the input Hurl file, and cannot contain implicit parent directory (`..`). You can use [`--file-root` option](#getting-started-manual-file-root)
to specify the root directory of all file nodes.

## Filters {#file-format-filters-filters}

### Definition {#file-format-filters-definition}

[Captures](#file-format-capturing-response) and [asserts](#file-format-asserting-response) share a common structure: query. A query is used to extract data from an HTTP response; this data
can come from the HTTP response body, the HTTP response headers or from the HTTP meta-information (like `duration` for instance)...

In this example, the query __`jsonpath "$.books[0].name"`__ is used in a capture to save data and in an assert to test
the HTTP response body.

<div class="schema-container schema-container u-font-size-2 u-font-size-3-md">
 <div class="schema">
   <span class="schema-token schema-color-1">name<span class="schema-label">variable</span></span>
   <span> : </span>
   <span class="schema-token schema-color-2">jsonpath "$.books[0].name"<span class="schema-label">query</span></span>
 </div>
</div>

<div class="schema-container schema-container u-font-size-2 u-font-size-3-md">
 <div class="schema">
   <span class="schema-token schema-color-2">jsonpath "$.books[0].name"<span class="schema-label">query</span></span>
   <span class="schema-token schema-color-3">== "Dune"<span class="schema-label">predicate</span></span>
 </div>
</div>

In both case, the query is exactly the same: queries are the core structure of asserts and captures. Sometimes, you want
to process data extracted by queries: that's the purpose of __filters__.

Filters are used to transform value extracted by a query and can be used in asserts and captures to refine data. Filters
__can be chained__, allowing for fine-grained data extraction.

<div class="schema-container schema-container u-font-size-2 u-font-size-3-md">
 <div class="schema">
    <span class="schema-token schema-color-2">jsonpath "$.name"<span class="schema-label">query</span></span>
    <span class="schema-token schema-color-1">split "," nth 0<span class="schema-label">2 filters</span></span>
    <span class="schema-token schema-color-3">== "Herbert"<span class="schema-label">predicate</span></span>
 </div>
</div>

### Example {#file-format-filters-example}

### Description {#file-format-filters-description}

#### base64Decode {#file-format-filters-base64decode}

Decode a base 64 encoded string into bytes.

#### base64Encode {#file-format-filters-base64encode}

Encode bytes into base 64 encoded string.

#### count {#file-format-filters-count}

Counts the number of items in a collection.

#### daysAfterNow {#file-format-filters-daysafternow}

Returns the number of days between now and a date in the future.

#### daysBeforeNow {#file-format-filters-daysbeforenow}

Returns the number of days between now and a date in the past.

#### decode {#file-format-filters-decode}

Decode bytes to string using encoding.

**Examples:**

Example 1 (json):
```json
{
    "id": 0,
    "name": "Frieda",
    "picture": "images/scottish-terrier.jpeg",
    "age": 3,
    "breed": "Scottish Terrier",
    "location": "Lisco, Alabama"
}
```

Example 2 (xml):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<catalog>
   <book id="bk101">
      <author>Gambardella, Matthew</author>
      <title>XML Developer's Guide</title>
      <genre>Computer</genre>
      <price>44.95</price>
      <publish_date>2000-10-01</publish_date>
      <description>An in-depth look at creating applications with XML.</description>
   </book>
</catalog>
```

Example 3 (unknown):
```unknown
Year,Make,Model,Description,Price
1997,Ford,E350,"ac, abs, moon",3000.00
1999,Chevy,"Venture ""Extended Edition""","",4900.00
1999,Chevy,"Venture ""Extended Edition, Very Large""",,5000.00
1996,Jeep,Grand Cherokee,"MUST SELL! air, moon roof, loaded",4799.00
```

Example 4 (unknown):
```unknown
line1
line2
line3
```

---

## Run the same GET request with a header:

**URL:** llms-txt#run-the-same-get-request-with-a-header:

GET https://example.org/index.html
Cookie: theme=light; sessionToken=abc123
hurl

**Examples:**

Example 1 (unknown):
```unknown
#### Basic Authentication {#file-format-request-basic-authentication}

A basic authentication section can be used to perform [basic authentication](#file-format-request-basic-authentication).

Username is followed by a `:` and a password. The basic authentication section starts with
`[BasicAuth]`. Username and password are _not_ base64 encoded.
```

---

## Run a POST request with form parameters section:

**URL:** llms-txt#run-a-post-request-with-form-parameters-section:

POST https://example.org/test
[Form]
name: John Doe
key1: value1

---
