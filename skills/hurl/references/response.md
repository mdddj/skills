# Hurl - Response

**Pages:** 28

---

## Testing status code is in a 200-300 range

**URL:** llms-txt#testing-status-code-is-in-a-200-300-range

**Contents:**
  - Reports {#getting-started-samples-reports}
  - Others {#getting-started-samples-others}

HTTP *
[Asserts]
status >= 200
status < 300
hurl
GET https://example.org/index.html
HTTP 200
Set-Cookie: theme=light
Set-Cookie: sessionToken=abc123; Expires=Wed, 09 Jun 2021 10:18:14 GMT
hurl
GET https://example.org
HTTP 302
[Asserts]
header "Location" contains "www.example.net"
hurl
GET https://example.org/index.html
HTTP 200
Set-Cookie: theme=light
Set-Cookie: sessionToken=abc123; Expires=Wed, 09 Jun 2021 10:18:14 GMT
[Asserts]
header "Location" contains "www.example.net"
hurl
GET https://example.org/order
screencapability: low
HTTP 200
[Asserts]
jsonpath "$.validated" == true
jsonpath "$.userInfo.firstName" == "Franck"
jsonpath "$.userInfo.lastName" == "Herbert"
jsonpath "$.hasDevice" == false
jsonpath "$.links" count == 12
jsonpath "$.state" != null
jsonpath "$.order" matches "^order-\\d{8}$"
jsonpath "$.order" matches /^order-\d{8}$/     # Alternative syntax with regex literal
jsonpath "$.created" isIsoDate
hurl
GET https://example.org
HTTP 200
Content-Type: text/html; charset=UTF-8
[Asserts]
xpath "string(/html/head/title)" contains "Example" # Check title
xpath "count(//p)" == 2  # Check the number of p
xpath "//p" count == 2  # Similar assert for p
xpath "boolean(count(//h2))" == false  # Check there is no h2
xpath "//h2" not exists  # Similar assert for h2
xpath "string(//div[1])" matches /Hello.*/
hurl
GET https://example.org/home
HTTP 200
[Asserts]
cookie "JSESSIONID" == "8400BAFE2F66443613DC38AE3D9D6239"
cookie "JSESSIONID[Value]" == "8400BAFE2F66443613DC38AE3D9D6239"
cookie "JSESSIONID[Expires]" contains "Wed, 13 Jan 2021"
cookie "JSESSIONID[Secure]" exists
cookie "JSESSIONID[HttpOnly]" exists
cookie "JSESSIONID[SameSite]" == "Lax"
hurl
GET https://example.org/data.tar.gz
HTTP 200
[Asserts]
sha256 == hex,039058c6f2c0cb492c533b0a4d14ef77cc0f78abccced5287d84a1a2011cfb81;
hurl
GET https://example.org
HTTP 200
[Asserts]
certificate "Subject" == "CN=example.org"
certificate "Issuer" == "C=US, O=Let's Encrypt, CN=R3"
certificate "Expire-Date" daysAfterNow > 15
certificate "Serial-Number" matches /[\da-f]+/
hurl
GET https://example.org/api/cats/123
HTTP 200
{
  "name" : "Purrsloud",
  "species" : "Cat",
  "favFoods" : ["wet food", "dry food", "<strong>any</strong> food"],
  "birthYear" : 2016,
  "photo" : "https://learnwebcode.github.io/json-example/images/cat-2.jpg"
}
hurl
GET https://example.org/index.html
HTTP 200
[Asserts]
body == file,cat.json;
hurl
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

Year,Make,Model,Description,Price
1997,Ford,E350,"ac, abs, moon",3000.00
1999,Chevy,"Venture ""Extended Edition""","",4900.00
1999,Chevy,"Venture ""Extended Edition, Very Large""",,5000.00
1996,Jeep,Grand Cherokee,"MUST SELL! air, moon roof, loaded",4799.00
hurl
POST https://example.org/helloworld
HTTP 200
`Hello world!`
hurl
GET https://example.org
HTTP 200
file,data.bin;
shell
$ hurl --test --report-html build/report/ *.hurl
shell
$ hurl --test --report-json build/report/ *.hurl
shell
$ hurl --test --report-junit build/report.xml *.hurl
shell
$ hurl --test --report-tap build/report.txt *.hurl
shell
$ hurl --json *.hurl
hurl
GET https://foo.com
HTTP/3 200

GET https://bar.com
HTTP/2 200
hurl
GET https://foo.com
HTTP 200
[Asserts]
version == "3"

GET https://bar.com
HTTP 200
[Asserts]
version == "2"
version toFloat > 1.1
hurl
GET https://foo.com
HTTP 200
[Asserts]
ip == " 2001:0db8:85a3:0000:0000:8a2e:0370:733"
ip startsWith "2001"
ip isIpv6
hurl

**Examples:**

Example 1 (unknown):
```unknown
[Doc](#file-format-asserting-response-status-assert)


#### Testing Response Headers {#getting-started-samples-testing-response-headers}

Use implicit response asserts to test header values:
```

Example 2 (unknown):
```unknown
[Doc](#file-format-asserting-response-headers)


Or use explicit response asserts with [predicates](#file-format-asserting-response-predicates):
```

Example 3 (unknown):
```unknown
[Doc](#file-format-asserting-response-header-assert)

Implicit and explicit asserts can be combined:
```

Example 4 (unknown):
```unknown
#### Testing REST APIs {#getting-started-samples-testing-rest-apis}

Asserting JSON body response (node values, collection count etc...) with [JSONPath](https://goessner.net/articles/JsonPath/):
```

---

## A status code check:

**URL:** llms-txt#a-status-code-check:

GET https://foo.com
HTTP 200

---

## that status code is Forbidden 403

**URL:** llms-txt#that-status-code-is-forbidden-403

**Contents:**
  - Description {#file-format-entry-description}

POST https://acmecorp.net/contact
[Form]
default: false
email: john.doe@rookie.org
number: 33611223344
HTTP 403
shell
$ hurl --location foo.hurl
hurl
GET https://google.fr
HTTP 301

GET https://google.fr
[Options]
location: true
HTTP 200

GET https://google.fr
HTTP 301
hurl

**Examples:**

Example 1 (unknown):
```unknown
### Description {#file-format-entry-description}

#### Options {#file-format-entry-options}

[Options](#getting-started-manual-options) specified on the command line apply to every entry in an Hurl file. For instance, with [`--location` option](#getting-started-manual-location),
every entry of a given file will follow redirection:
```

Example 2 (unknown):
```unknown
You can use an [`[Options]` section][options](#file-format-request-options) to set option only for a specified request. For instance, in this Hurl file,
the second entry will follow location (so we can test the status code to be 200 instead of 301).
```

Example 3 (unknown):
```unknown
You can use the `[Options](#getting-started-manual-options)` section to log a specific entry:
```

---

## not in this exact order, this assert will fail.

**URL:** llms-txt#not-in-this-exact-order,-this-assert-will-fail.

Set-Cookie: LSID=DQAAAKEaem_vYg; Expires=Wed, 13 Jan 2021 22:23:01 GMT; Secure; HttpOnly; Path=/accounts; SameSite=Lax;
Set-Cookie: HSID=AYQEVnDKrdst; Domain=localhost; Expires=Wed, 13 Jan 2021 22:23:01 GMT; HttpOnly; Path=/
Set-Cookie: SSID=Ap4PGTEq; Domain=localhost; Expires=Wed, 13 Jan 2021 22:23:01 GMT; Secure; HttpOnly; Path=/

---

## Get some news, response description is optional

**URL:** llms-txt#get-some-news,-response-description-is-optional

GET https://acmecorp.net/news

---

## Using cookie assert, one can check cookie value and various attributes.

**URL:** llms-txt#using-cookie-assert,-one-can-check-cookie-value-and-various-attributes.

[Asserts]
cookie "LSID" == "DQAAAKEaem_vYg"
cookie "LSID[Value]" == "DQAAAKEaem_vYg"
cookie "LSID[Expires]" exists
cookie "LSID[Expires]" contains "Wed, 13 Jan 2021"
cookie "LSID[Max-Age]" not exists
cookie "LSID[Domain]" not exists
cookie "LSID[Path]" == "/accounts"
cookie "LSID[Secure]" exists
cookie "LSID[HttpOnly]" exists
cookie "LSID[SameSite]" == "Lax"
hurl
GET https://example.org
HTTP 200
[Asserts]
body contains "<h1>Welcome!</h1>"
hurl

**Examples:**

Example 1 (unknown):
```unknown
> `Secure` and `HttpOnly` attributes can only be tested with `exists` or `not exists` predicates
> to reflect the [Set-Cookie header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie) semantics (in other words, queries `<cookie-name>[HttpOnly]`
> and `<cookie-name>[Secure]` don't return boolean).

#### Body assert {#file-format-asserting-response-body-assert}

Check the value of the received HTTP response body when decoded as a string. Body assert consists of the keyword `body`
followed by a predicate function and value.
```

Example 2 (unknown):
```unknown
The encoding used to decode the response body bytes to a string is based on the `charset` value in the `Content-Type`
header response.
```

---

## Our HTML response is encoded using GB 2312.

**URL:** llms-txt#our-html-response-is-encoded-using-gb-2312.

---

## Create a new job

**URL:** llms-txt#create-a-new-job

POST http://api.example.org/jobs
HTTP 201
[Captures]
job_id: jsonpath "$.id"
[Asserts]
jsonpath "$.state" == "RUNNING"

---

## with tasty comments...

**URL:** llms-txt#with-tasty-comments...

**Contents:**
  - Special Characters in Strings {#file-format-hurl-file-special-characters-in-strings}

GET https://www.sample.net
x-app: MY_APP  # Add a dummy header
HTTP 302       # Check that we have a redirection
[Asserts]
header "Location" exists
header "Location" contains "login"  # Check that we are redirected to the login page
hurl
GET https://example.org/api
HTTP 200

**Examples:**

Example 1 (unknown):
```unknown
### Special Characters in Strings {#file-format-hurl-file-special-characters-in-strings}

String can include the following special characters:

- The escaped special characters \" (double quotation mark), \\ (backslash), \b (backspace), \f (form feed),
 \n (line feed), \r (carriage return), and \t (horizontal tab)
- An arbitrary Unicode scalar value, written as \u{n}, where n is a 1â8 digit hexadecimal number
```

---

## Open the captured page.

**URL:** llms-txt#open-the-captured-page.

GET https://example.org/home/pets/{{pet-id}}
HTTP 200
hurl

**Examples:**

Example 1 (unknown):
```unknown
XPath captures are not limited to node values (like string, or boolean); any
valid XPath can be captured and asserted with variable asserts.
```

---

## Do login!

**URL:** llms-txt#do-login!

**Contents:**
- Also an HTTP Test Tool {#introduction-home-also-an-http-test-tool}
- Why Hurl? {#introduction-home-why-hurl}
- Powered by curl {#introduction-home-powered-by-curl}
- Feedbacks {#introduction-home-feedbacks}
- Resources {#introduction-home-resources}

POST https://example.org/login?user=toto&password=1234
X-CSRF-TOKEN: {{csrf_token}}
HTTP 302
hurl
GET https://example.org/api/health
GET https://example.org/api/step1
GET https://example.org/api/step2
GET https://example.org/api/step3
hurl
POST https://example.org/api/tests
{
    "id": "4568",
    "evaluate": true
}
HTTP 200
[Asserts]
header "X-Frame-Options" == "SAMEORIGIN"
jsonpath "$.status" == "RUNNING"    # Check the status code
jsonpath "$.tests" count == 25      # Check the number of items
jsonpath "$.id" matches /\d{4}/     # Check the format of the id
hurl
GET https://example.org
HTTP 200
[Asserts]
xpath "normalize-space(//head/title)" == "Hello world!"
graphql
{
  human(id: "1000") {
    name
    height(unit: FOOT)
  }
}
hurl
POST https://example.org/InStock
Content-Type: application/soap+xml; charset=utf-8
SOAPAction: "http://www.w3.org/2003/05/soap-envelope"
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope" xmlns:m="https://example.org">
  <soap:Header></soap:Header>
  <soap:Body>
    <m:GetStockPrice>
      <m:StockName>GOOG</m:StockName>
    </m:GetStockPrice>
  </soap:Body>
</soap:Envelope>
HTTP 200
hurl
GET https://example.org/api/v1/pets
HTTP 200
[Asserts]
duration < 1000  # Duration in ms
hurl
GET https://example.org/data.tar.gz
HTTP 200
[Asserts]
sha256 == hex,039058c6f2c0cb492c533b0a4d14ef77cc0f78abccced5287d84a1a2011cfb81;
hurl
POST https://hurl.dev/api/feedback
{
  "name": "John Doe",
  "feedback": "Hurl is awesome!"
}
HTTP 200
```

## Resources {#introduction-home-resources}

[License](#resources-license)

[Blog](https://hurl.dev/blog)

[Tutorial](https://hurl.dev/docs/tutorial/your-first-hurl-file.html)

[Documentation](https://hurl.dev)

[GitHub](https://github.com/Orange-OpenSource/hurl)

**Examples:**

Example 1 (unknown):
```unknown
Chaining multiple requests is easy:
```

Example 2 (unknown):
```unknown
## Also an HTTP Test Tool {#introduction-home-also-an-http-test-tool}

Hurl can run HTTP requests but can also be used to <b>test HTTP responses</b>.
Different types of queries and predicates are supported, from [XPath](https://en.wikipedia.org/wiki/XPath) and [JSONPath](https://goessner.net/articles/JsonPath/) on body response,
to assert on status code and response headers.



It is well adapted for <b>REST / JSON APIs</b>
```

Example 3 (unknown):
```unknown
<b>HTML content</b>
```

Example 4 (unknown):
```unknown
<b>GraphQL</b>

~~~hurl
POST https://example.org/graphql
```

---

## a, c, d are run, b is skipped

**URL:** llms-txt#a,-c,-d-are-run,-b-is-skipped

GET https://example.org/a

GET https://example.org/b
[Options]
skip: true

GET https://example.org/c

GET https://example.org/d
hurl
GET https://sample.org/helloworld
HTTP *
[Asserts]
duration < 1000   # Check that response time is less than one second
hurl
POST https://example.org/InStock
Content-Type: application/soap+xml; charset=utf-8
SOAPAction: "http://www.w3.org/2003/05/soap-envelope"
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope" xmlns:m="https://example.org">
  <soap:Header></soap:Header>
  <soap:Body>
    <m:GetStockPrice>
      <m:StockName>GOOG</m:StockName>
    </m:GetStockPrice>
  </soap:Body>
</soap:Envelope>
HTTP 200
hurl
GET https://example.org
HTTP 200
[Captures]
csrf_token: xpath "string(//meta[@name='_csrf_token']/@content)"

POST https://example.org/login?user=toto&password=1234
X-CSRF-TOKEN: {{csrf_token}}
HTTP 302
shell
$ hurl --secret token=1234 file.hurl
hurl
POST https://example.org
X-Token: {{token}}
{
  "name": "Alice",
  "value": 100
}
HTTP 200
hurl

**Examples:**

Example 1 (unknown):
```unknown
[Doc](#getting-started-manual-skip)

#### Testing Endpoint Performance {#getting-started-samples-testing-endpoint-performance}
```

Example 2 (unknown):
```unknown
[Doc](#file-format-asserting-response-duration-assert)

#### Using SOAP APIs {#getting-started-samples-using-soap-apis}
```

Example 3 (unknown):
```unknown
[Doc](#file-format-request-xml-body)

#### Capturing and Using a CSRF Token {#getting-started-samples-capturing-and-using-a-csrf-token}
```

Example 4 (unknown):
```unknown
[Doc](#file-format-capturing-response-xpath-capture)

#### Redacting Secrets {#getting-started-samples-redacting-secrets}

Using command-line for known values:
```

---

## so we decode explicitly the bytes.

**URL:** llms-txt#so-we-decode-explicitly-the-bytes.

GET https://example.org/cn
HTTP 200
[Asserts]
header "Content-Type" == "text/html"
bytes contains hex,c4e3bac3cac0bde7; # ä½ å¥½ä¸ç encoded in GB2312
bytes decode "gb2312" contains "ä½ å¥½ä¸ç"
hurl

**Examples:**

Example 1 (unknown):
```unknown
Body asserts are automatically decompressed based on the value of `Content-Encoding` response header. So,
whatever is the response compression (`gzip`, `brotli`) etc... asserts values don't depend on the content encoding.
```

---

## Our HTML response is encoded with GB 2312 (see https://en.wikipedia.org/wiki/GB_2312)

**URL:** llms-txt#our-html-response-is-encoded-with-gb-2312-(see-https://en.wikipedia.org/wiki/gb_2312)

GET https://example.org/cn
HTTP 200
[Asserts]
header "Content-Type" == "text/html; charset=gb2312"

---

## A really well tested web page...

**URL:** llms-txt#a-really-well-tested-web-page...

GET https://example.org/home
HTTP 200
[Asserts]
header "Content-Type" contains "text/html"
header "Last-Modified" == "Wed, 21 Oct 2015 07:28:00 GMT"
xpath "//h1" exists  # Check we've at least one h1
xpath "normalize-space(//h1)" contains "Welcome"
xpath "//h2" count == 13
xpath "string(//article/@data-id)" startsWith "electric"
hurl
GET https://example.org
HTTP *
[Asserts]
status < 300
hurl
GET https://example.org
HTTP *
[Asserts]
version == "2"
hurl
GET https://example.org
HTTP 302
[Asserts]
header "Location" contains "www.example.net"
header "Last-Modified" matches /\d{2} [a-z-A-Z]{3} \d{4}/

> GET /hello HTTP/1.1
> Host: example.org
> Accept: */*
> User-Agent: hurl/2.0.0-SNAPSHOT
>
* Response: (received 12 bytes in 11 ms)
*
< HTTP/1.0 200 OK
< Vary: Content-Type
< Vary: User-Agent
< Content-Type: text/html; charset=utf-8
< Content-Length: 12
< Server: Flask Server
< Date: Fri, 07 Oct 2022 20:53:35 GMT
hurl
GET https://example.org/hello
HTTP 200
[Asserts]
header "Vary" count == 2
header "Vary" contains "User-Agent"
header "Vary" contains "Content-Type"
hurl
GET https://example.org/hello
HTTP 200
Vary: User-Agent
Vary: Content-Type
hurl
GET http://localhost:8000/cookies/set
HTTP 200

**Examples:**

Example 1 (unknown):
```unknown
#### Status assert {#file-format-asserting-response-status-assert}

Check the received HTTP response status code. Status assert consists of the keyword `status` followed by a predicate
function and value.
```

Example 2 (unknown):
```unknown
#### Version assert {#file-format-asserting-response-version-assert}

Check the received HTTP version. Version assert consists of the keyword `version` followed by a predicate function
and value. The value returns by `version` is a string:
```

Example 3 (unknown):
```unknown
#### Header assert {#file-format-asserting-response-header-assert}

Check the value of a received HTTP response header. Header assert consists of the keyword `header` followed by the value
of the header, a predicate function and a predicate value. Like [headers implicit asserts](#file-format-asserting-response-headers), the check is
case-insensitive for the name: comparing a `Content-Type` header is equivalent to a `content-type` one.
```

Example 4 (unknown):
```unknown
If there are multiple headers with the same name, the header assert returns a collection, so `count`, `contains` can be
used in this case to test the header list.

Let's say we have this request and response:
```

---

## Scenario: create and retrieve a cat

**URL:** llms-txt#scenario:-create-and-retrieve-a-cat

**Contents:**
  - macOS {#getting-started-frequently-asked-questions-macos}

POST http://myhost.com/v1/cats
{ "name": "Billie" }
HTTP 201
[Captures]
cat_id: jsonpath "$.id"
[Asserts]
jsonpath "$.name" == "Billie"

GET http://myshost.com/v1/cats/{{cat_id}}
HTTP 200
shell
$ hurl --version
hurl 2.0.0 libcurl/7.79.1 (SecureTransport) LibreSSL/3.3.6 zlib/1.2.11 nghttp2/1.45.1
Features (libcurl):  alt-svc AsynchDNS HSTS HTTP2 IPv6 Largefile libz NTLM NTLM_WB SPNEGO SSL UnixSockets
Features (built-in): brotli
shell
$ which hurl
/opt/homebrew/bin/hurl
$ otool -L /opt/homebrew/bin/hurl:
	/usr/lib/libxml2.2.dylib (compatibility version 10.0.0, current version 10.9.0)
	/System/Library/Frameworks/CoreFoundation.framework/Versions/A/CoreFoundation (compatibility version 150.0.0, current version 1858.112.0)
	/usr/lib/libcurl.4.dylib (compatibility version 7.0.0, current version 9.0.0)
	/usr/lib/libiconv.2.dylib (compatibility version 7.0.0, current version 7.0.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1311.100.3)
shell
$ which hurl
/root/.cargo/bin/hurl
$ ldd /root/.cargo/bin/hurl
ldd /root/.cargo/bin/hurl
	linux-vdso.so.1 (0x0000ffff8656a000)
	libxml2.so.2 => /usr/lib/aarch64-linux-gnu/libxml2.so.2 (0x0000ffff85fe8000)
	libcurl.so.4 => /usr/lib/aarch64-linux-gnu/libcurl.so.4 (0x0000ffff85f45000)
	libgcc_s.so.1 => /lib/aarch64-linux-gnu/libgcc_s.so.1 (0x0000ffff85f21000)
	...
	libkeyutils.so.1 => /lib/aarch64-linux-gnu/libkeyutils.so.1 (0x0000ffff82ed5000)
	libffi.so.7 => /usr/lib/aarch64-linux-gnu/libffi.so.7 (0x0000ffff82ebc000)

critical/test1.hurl
critical/test2.hurl
additional/test1.hurl
additional/test2.hurl
shell
$ hurl --test critical/*.hurl
hurl
GET https://example.org/api/users/1
User-Agent: Custom
HTTP 200
[Asserts]
jsonpath "$.name" == "Bob"
shell
$ hurlfmt test.hurl --out json | jq
{
  "entries": [
    {
      "request": {
        "method": "GET",
        "url": "https://example.org/api/users/1",
        "headers": [
          {
            "name": "User-Agent",
            "value": "Custom"
          }
        ]
      },
      "response": {
        "version": "HTTP",
        "status": 200,
        "asserts": [
          {
            "query": {
              "type": "jsonpath",
              "expr": "$.name"
            },
            "predicate": {
              "type": "==",
              "value": "Bob"
            }
          }
        ]
      }
    }
  ]
}
shell
$ TODAY=$(date '+%y%m%d')
$ TOMORROW=$(date '+%y%m%d' -d"+1days")
$ hurl --variable "today=$TODAY" --variable "tomorrow=$TOMORROW" test.hurl
shell
$ export HURL_today=$(date '+%y%m%d')
$ export HURL_tomorrow=$(date '+%y%m%d' -d"+1days")
$ hurl test.hurl
shell
$ sudo install_name_tool -change /usr/lib/libcurl.4.dylib PATH_TO_CUSTOM_LIBCURL PATH_TO_HURL_BIN
shell

**Examples:**

Example 1 (unknown):
```unknown
A key point of Hurl is to work on the HTTP domain. In particular, there is no JavaScript runtime, Hurl works on the
raw HTTP requests/responses, and not on a DOM managed by a HTML engine. For security, this can be seen as a feature:
let's say you want to test backend validation, you want to be able to bypass the browser or javascript validations and
directly test a backend endpoint.

Finally, with no headless browser and working on the raw HTTP data, Hurl is also
really reliable with a very small probability of false positives. Integration tests with tools like
[Selenium](https://www.selenium.dev) can, in this regard, be challenging to maintain.

Just use what is convenient for you. In our case, it's Hurl!

#### Hurl is build on top of libcurl, but what is added? {#getting-started-frequently-asked-questions-hurl-is-build-on-top-of-libcurl-but-what-is-added}

Hurl has two main functionalities on top of [curl](https://curl.haxx.se):

1. Chain several requests:

   With its [captures](#file-format-capturing-response), it enables to inject data received from a response into
   following requests. [CSRF tokens](https://en.wikipedia.org/wiki/Cross-site_request_forgery)
   are typical examples in a standard web session.

2. Test HTTP responses:

   With its [asserts](#file-format-asserting-response), responses can be easily tested.

Hurl benefits from the features of the `libcurl` against it is linked. You can check `libcurl` version with `hurl --version`.

For instance on macOS:
```

Example 2 (unknown):
```unknown
You can also check which `libcurl` is used.

On macOS:
```

Example 3 (unknown):
```unknown
On Linux:
```

Example 4 (unknown):
```unknown
Note that some Hurl features are dependent on `libcurl` capacities: for instance, if your `libcurl` doesn't support
HTTP/2 Hurl won't be able to send HTTP/2 request.


#### Why shouldn't I use Hurl? {#getting-started-frequently-asked-questions-why-shouldnt-i-use-hurl}

If you need a GUI. Currently, Hurl does not offer a GUI version (like [Postman](https://www.postman.com)). While we
think that it can be useful, we prefer to focus for the time-being on the core, keeping something simple and fast.
Contributions to build a GUI are welcome.


#### I have a large numbers of tests, how to run just specific tests? {#getting-started-frequently-asked-questions-i-have-a-large-numbers-of-tests-how-to-run-just-specific-tests}

By convention, you can organize Hurl files into different folders or prefix them.

For example, you can split your tests into two folders critical and additional.
```

---

## Check that response status code is > 400 and <= 500

**URL:** llms-txt#check-that-response-status-code-is->-400-and-<=-500

[Asserts]
status > 400
status <= 500
hurl

**Examples:**

Example 1 (unknown):
```unknown
While `HTTP/1.0`, `HTTP/1.1`, `HTTP/2` and `HTTP/3` explicitly check HTTP version:
```

---

## Pull job status until it is completed

**URL:** llms-txt#pull-job-status-until-it-is-completed

GET http://api.example.org/jobs/{{job_id}}
[Options]
retry: 10   # maximum number of retry, -1 for unlimited
retry-interval: 300ms
HTTP 200
[Asserts]
jsonpath "$.state" == "COMPLETED"
hurl

**Examples:**

Example 1 (unknown):
```unknown
#### Control flow {#file-format-entry-control-flow}

In `[Options](#getting-started-manual-options)` section, `skip` and `repeat` can be used to control flow of execution:

- `skip: true/false` skip this request and execute the next one unconditionally,
- `repeat: N` loop the request N times. If there are assert or runtime errors, the requests execution is stopped.
```

---

## bytes of the response, without any text decoding:

**URL:** llms-txt#bytes-of-the-response,-without-any-text-decoding:

bytes contains hex,c4e3bac3cac0bde7; # ä½ å¥½ä¸ç encoded in GB 2312

---

## Test that the XML endpoint return 200 pets

**URL:** llms-txt#test-that-the-xml-endpoint-return-200-pets

**Contents:**
  - Body {#file-format-asserting-response-body}

GET https://example.org/api/pets
HTTP 200
[Captures]
pets: xpath "//pets"
[Asserts]
variable "pets" count == 200
hurl
GET https://example.org/helloworld
HTTP 200
[Asserts]
duration < 1000   # Check that response time is less than one second
hurl
GET https://example.org
HTTP 200
[Asserts]
certificate "Subject" == "CN=example.org"
certificate "Issuer" == "C=US, O=Let's Encrypt, CN=R3"
certificate "Expire-Date" daysAfterNow > 15
certificate "Serial-Number" matches "[0-9af]+"
hurl

**Examples:**

Example 1 (unknown):
```unknown
#### Duration assert {#file-format-asserting-response-duration-assert}

Check the total duration (sending plus receiving time) of the HTTP transaction.
```

Example 2 (unknown):
```unknown
#### SSL certificate assert {#file-format-asserting-response-ssl-certificate-assert}

Check the SSL certificate properties. Certificate assert consists of the keyword `certificate`, followed by the
certificate attribute value.

The following attributes are supported: `Subject`, `Issuer`, `Start-Date`, `Expire-Date` and `Serial-Number`.
```

Example 3 (unknown):
```unknown
### Body {#file-format-asserting-response-body}

Optional assertion on the received HTTP response body. Body section can be seen as syntactic sugar over [body asserts](#file-format-asserting-response-body-assert)
(with `==` predicate). If the body of the response is a [JSON](https://www.json.org) string or a [XML](https://en.wikipedia.org/wiki/XML) string, the body assertion can be
directly inserted without any modification. For a text based body that is neither JSON nor XML, one can use multiline
string that starts with <code>&#96;&#96;&#96;</code> and ends with <code>&#96;&#96;&#96;</code>. For a precise byte
control of the response body, a [Base64](https://en.wikipedia.org/wiki/Base64) encoded string or an input file can be used to describe exactly the body byte
content to check.

Like explicit [`body` assert](#file-format-asserting-response-body-assert), the body section is automatically decompressed based on the value of `Content-Encoding`
response header. So, whatever is the response compression (`gzip`, `brotli`, etc...) body section doesn't depend on
the content encoding. For textual body sections (JSON, XML, multiline, etc...), content is also decoded to string, based
on the value of `Content-Type` response header.

#### JSON body {#file-format-asserting-response-json-body}
```

---

## Explicit asserts section

**URL:** llms-txt#explicit-asserts-section

**Contents:**
  - Implicit asserts {#file-format-asserting-response-implicit-asserts}

bytes count == 120
header "Content-Type" contains "utf-8"
jsonpath "$.cats" count == 49
jsonpath "$.cats[0].name" == "Felix"
jsonpath "$.cats[0].lives" == 9
hurl
GET https://example.org/404.html
HTTP 404
hurl
GET https://example.org/api/pets
HTTP *

**Examples:**

Example 1 (unknown):
```unknown
Body responses can be encoded by server (see [`Content-Encoding` HTTP header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding)) but asserts in Hurl files are not
affected by this content compression. All body asserts (`body`, `bytes`, `sha256` etc...) work _after_ content decoding.

Finally, body text asserts (`body`, `jsonpath`, `xpath` etc...) are also decoded to strings based on [`Content-Type` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)
so these asserts can be written with usual strings.

### Implicit asserts {#file-format-asserting-response-implicit-asserts}

#### Version - Status {#file-format-asserting-response-version-status}

Expected protocol version and status code of the HTTP response.

Protocol version is one of `HTTP/1.0`, `HTTP/1.1`, `HTTP/2`, `HTTP/3` or
`HTTP`; `HTTP` describes any version. Note that there are no status text following the status code.
```

Example 2 (unknown):
```unknown
Wildcard keywords `HTTP` and `*` can be used to disable tests on protocol version and status:
```

---

## Same assert as previous using regex literals

**URL:** llms-txt#same-assert-as-previous-using-regex-literals

regex /^(\d{4}-\d{2}-\d{2})$/ == "2018-12-31"
hurl
GET https://example.org/data.tar.gz
HTTP 200
[Asserts]
sha256 == hex,039058c6f2c0cb492c533b0a4d14ef77cc0f78abccced5287d84a1a2011cfb81;
hurl

**Examples:**

Example 1 (unknown):
```unknown
The regex pattern must have at least one capture group, otherwise the assert will fail. The assertion is done on the
captured group value. When the regex pattern is a double-quoted string, metacharacters beginning with a backslash in the
pattern (like `\d`, `\s`) must be escaped; literal pattern enclosed by `/` can also be used to avoid metacharacters
escaping.

#### SHA-256 assert {#file-format-asserting-response-sha-256-assert}

Check response body [SHA-256](https://en.wikipedia.org/wiki/SHA-2) hash.
```

Example 2 (unknown):
```unknown
Like `body` assert, `sha256` assert works _after_ content encoding decompression (so the predicates values are not
affected by `Content-Encoding` response header). For instance, if we have a resource `a.txt` on a server with a
given hash `abcdef`, `sha256` value is not affected by `Content-Encoding`:
```

---

## Capture the identifier from the dom node <div id="pet0">5646eaf23</div

**URL:** llms-txt#capture-the-identifier-from-the-dom-node-<div-id="pet0">5646eaf23</div

HTTP 200
[Captures]
pet-id: xpath "normalize-space(//div[@id='pet0'])"

---

## With content encoding compression:

**URL:** llms-txt#with-content-encoding-compression:

GET https://example.org/a.txt
Accept-Encoding: brotli
HTTP 200
[Asserts]
header "Content-Encoding" == "brotli"
sha256 == hex,abcdef;
hurl
GET https://example.org/data.tar.gz
HTTP 200
[Asserts]
md5 == hex,ed076287532e86365e841e92bfc50d8c;
hurl
GET https://example.org/redirecting
[Options]
location: true
HTTP 200
[Asserts]
url == "https://example.org/redirected"
hurl
GET https://example.org/hello
HTTP 200
[Asserts]
ip isIpv4
ip not isIpv6
ip == "172.16.45.87"
hurl

**Examples:**

Example 1 (unknown):
```unknown
#### MD5 assert {#file-format-asserting-response-md5-assert}

Check response body [MD5](https://en.wikipedia.org/wiki/MD5) hash.
```

Example 2 (unknown):
```unknown
Like `sha256` asserts, `md5` assert works _after_ content encoding decompression (so the predicates values are not
affected by `Content-Encoding` response header)

#### URL assert {#file-format-asserting-response-url-assert}

Check the last fetched URL. This is most meaningful if you have told Hurl to follow redirection (see [`[Options]`section][options](#file-format-request-options) or
[`--location` option](#getting-started-manual-location)). URL assert consists of the keyword `url` followed by a predicate function and value.
```

Example 3 (unknown):
```unknown
#### IP address assert {#file-format-asserting-response-ip-address-assert}

Check the IP address of the last connection. The value of the `ip` query is a string.

> Predicates `isIpv4` and `isIpv6` are available to check if a particular string matches an IPv4 or IPv6 address and
> can use with `ip` queries.
```

Example 4 (unknown):
```unknown
#### Variable assert {#file-format-asserting-response-variable-assert}
```

---

## Get an authorization token:

**URL:** llms-txt#get-an-authorization-token:

GET https://example.org/token
HTTP 200
[Captures]
token: header "X-Token" redact

---

## text of the response, decoded with GB 2312:

**URL:** llms-txt#text-of-the-response,-decoded-with-gb-2312:

body contains "ä½ å¥½ä¸ç"
hurl

**Examples:**

Example 1 (unknown):
```unknown
If the `Content-Type` response header doesn't include any encoding hint, a [`decode` filter](#file-format-filters-decode) can be used to explicitly
decode the response body bytes.
```

---

## The following assert are equivalent:

**URL:** llms-txt#the-following-assert-are-equivalent:

**Contents:**
- Entry {#file-format-entry-entry}
  - Definition {#file-format-entry-definition}
  - Example {#file-format-entry-example}

[Asserts]
jsonpath "$.slideshow.title" == "A beautiful â!"
jsonpath "$.slideshow.title" == "A beautiful \u{2708}!"
hurl
GET https://example.org/api
x-token: BEEF \#STEACK # Some comment
HTTP 200
hurl

**Examples:**

Example 1 (unknown):
```unknown
In some case, (in headers value, etc..), you will also need to escape # to distinguish it from a comment.
In the following example:
```

Example 2 (unknown):
```unknown
We're sending a header `x-token` with value `BEEF #STEACK`



<hr>

## Entry {#file-format-entry-entry}

### Definition {#file-format-entry-definition}

A Hurl file is a list of entries, each entry being a mandatory [request](#file-format-request), optionally followed by a [response](#file-format-response).

Responses are not mandatory, a Hurl file consisting only of requests is perfectly valid. To sum up, responses can be used
to [capture values](#file-format-capturing-response) to perform subsequent requests, or [add asserts to HTTP responses](#file-format-asserting-response).

### Example {#file-format-entry-example}
```

---

## Second entry, the 200 OK response

**URL:** llms-txt#second-entry,-the-200-ok-response

GET https://www.google.fr
HTTP 200
hurl

**Examples:**

Example 1 (unknown):
```unknown
Alternatively, one can use [`--location`](#getting-started-manual-location) / [`--location-trusted`](#getting-started-manual-location-trusted) options to force redirection
to be followed. In this case, asserts are executed on the last received response. Optionally, the number of
redirections can be limited with [`--max-redirs`](#getting-started-manual-max-redirs).
```

---
