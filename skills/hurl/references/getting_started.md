# Hurl - Getting Started

**Pages:** 7

---

## Getting Started {#getting-started}

**URL:** llms-txt#getting-started-{#getting-started}

**Contents:**
- Installation {#getting-started-installation-installation}
  - Binaries Installation {#getting-started-installation-binaries-installation}
  - Building From Sources {#getting-started-installation-building-from-sources}
- Manual {#getting-started-manual-manual}
  - Name {#getting-started-manual-name}
  - Synopsis {#getting-started-manual-synopsis}
  - Description {#getting-started-manual-description}
  - Hurl File Format {#getting-started-manual-hurl-file-format}

## Installation {#getting-started-installation-installation}

### Binaries Installation {#getting-started-installation-binaries-installation}

#### Linux {#getting-started-installation-linux}

Precompiled binary is available at [Hurl latest GitHub release](https://github.com/Orange-OpenSource/hurl/releases/latest):

##### Debian / Ubuntu {#getting-started-installation-debian--ubuntu}

For Debian / Ubuntu, Hurl can be installed using a binary .deb file provided in each Hurl release.

For Ubuntu (bionic, focal, jammy, noble), Hurl can be installed from `ppa:lepapareil/hurl`

##### Alpine {#getting-started-installation-alpine}

Hurl is available on `testing` channel.

##### Arch Linux / Manjaro {#getting-started-installation-arch-linux--manjaro}

Hurl is available on [extra](https://archlinux.org/packages/extra/x86_64/hurl/) channel.

##### NixOS / Nix {#getting-started-installation-nixos--nix}

[NixOS / Nix package](https://search.nixos.org/packages?from=0&size=1&sort=relevance&type=packages&query=hurl) is available on stable channel.

#### macOS {#getting-started-installation-macos}

Precompiled binaries for Intel and ARM CPUs are available at [Hurl latest GitHub release](https://github.com/Orange-OpenSource/hurl/releases/latest).

##### Homebrew {#getting-started-installation-homebrew}

##### MacPorts {#getting-started-installation-macports}

#### FreeBSD {#getting-started-installation-freebsd}

#### Windows {#getting-started-installation-windows}

Windows requires the [Visual C++ Redistributable Package](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170#latest-microsoft-visual-c-redistributable-version) to be installed manually, as this is not included in the installer.

##### Zip File {#getting-started-installation-zip-file}

Hurl can be installed from a standalone zip file at [Hurl latest GitHub release](https://github.com/Orange-OpenSource/hurl/releases/latest). You will need to update your `PATH` variable.

##### Installer {#getting-started-installation-installer}

An executable installer is also available at [Hurl latest GitHub release](https://github.com/Orange-OpenSource/hurl/releases/latest).

##### Chocolatey {#getting-started-installation-chocolatey}

##### Scoop {#getting-started-installation-scoop}

##### Windows Package Manager {#getting-started-installation-windows-package-manager}

#### Cargo {#getting-started-installation-cargo}

If you're a Rust programmer, Hurl can be installed with cargo.

#### conda-forge {#getting-started-installation-conda-forge}

Hurl can also be installed with [`conda-forge`](https://conda-forge.org) powered package manager like [`pixi`](https://prefix.dev).

#### Docker {#getting-started-installation-docker}

#### npm {#getting-started-installation-npm}

### Building From Sources {#getting-started-installation-building-from-sources}

Hurl sources are available in [GitHub](https://github.com/Orange-OpenSource/hurl).

#### Build on Linux {#getting-started-installation-build-on-linux}

Hurl depends on libssl, libcurl and libxml2 native libraries. You will need their development files in your platform.

##### Debian based distributions {#getting-started-installation-debian-based-distributions}

##### Fedora based distributions {#getting-started-installation-fedora-based-distributions}

##### Red Hat based distributions {#getting-started-installation-red-hat-based-distributions}

##### Arch based distributions {#getting-started-installation-arch-based-distributions}

##### Alpine based distributions {#getting-started-installation-alpine-based-distributions}

#### Build on macOS {#getting-started-installation-build-on-macos}

Hurl is written in [Rust](https://www.rust-lang.org). You should [install](https://www.rust-lang.org/tools/install) the latest stable release.

#### Build on Windows {#getting-started-installation-build-on-windows}

Please follow the [contrib on Windows section](https://github.com/Orange-OpenSource/hurl/blob/master/contrib/windows/README.md).

## Manual {#getting-started-manual-manual}

### Name {#getting-started-manual-name}

hurl - run and test HTTP requests.

### Synopsis {#getting-started-manual-synopsis}

**hurl** [options] [FILE...]

### Description {#getting-started-manual-description}

**Hurl** is a command line tool that runs HTTP requests defined in a simple plain text format.

It can chain requests, capture values and evaluate queries on headers and body response. Hurl is very versatile, it can be used for fetching data and testing HTTP sessions: HTML content, REST / SOAP / GraphQL APIs, or any other XML / JSON based APIs.

If no input files are specified, input is read from stdin.

Hurl can take files as input, or directories. In the latter case, Hurl will search files with `.hurl` extension recursively.

Output goes to stdout by default. To have output go to a file, use the [`-o, --output`](#getting-started-manual-output) option:

By default, Hurl executes all HTTP requests and outputs the response body of the last HTTP call.

To have a test oriented output, you can use [`--test`](#getting-started-manual-test) option:

### Hurl File Format {#getting-started-manual-hurl-file-format}

The Hurl file format is fully documented in [https://hurl.dev/docs/hurl-file.html](https://hurl.dev/docs/hurl-file.html)

It consists of one or several HTTP requests

#### Capturing values {#getting-started-manual-capturing-values}

A value from an HTTP response can be-reused for successive HTTP requests.

A typical example occurs with CSRF tokens.

```hurl
GET https://example.org
HTTP 200

**Examples:**

Example 1 (shell):
```shell
$ INSTALL_DIR=/tmp
$ VERSION=6.1.0
$ curl --silent --location https://github.com/Orange-OpenSource/hurl/releases/download/$VERSION/hurl-$VERSION-x86_64-unknown-linux-gnu.tar.gz | tar xvz -C $INSTALL_DIR
$ export PATH=$INSTALL_DIR/hurl-$VERSION-x86_64-unknown-linux-gnu/bin:$PATH
```

Example 2 (shell):
```shell
$ VERSION=6.1.0
$ curl --location --remote-name https://github.com/Orange-OpenSource/hurl/releases/download/$VERSION/hurl_${VERSION}_amd64.deb
$ sudo apt update && sudo apt install ./hurl_${VERSION}_amd64.deb
```

Example 3 (shell):
```shell
$ VERSION=6.1.0
$ sudo apt-add-repository -y ppa:lepapareil/hurl
$ sudo apt install hurl="${VERSION}"*
```

Example 4 (shell):
```shell
$ apk add --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing hurl
```

---

## Introduction {#introduction}

**URL:** llms-txt#introduction-{#introduction}

**Contents:**
- What's Hurl? {#introduction-home-whats-hurl}

<div class="home-logo">
    <img class="u-theme-light" src="https://hurl.dev/assets/img/logo-light.svg" width="277px" height="72px" alt="Hurl logo"/>
    <img class="u-theme-dark" src="https://hurl.dev/assets/img/logo-dark.svg" width="277px" height="72px" alt="Hurl logo"/>
</div>

## What's Hurl? {#introduction-home-whats-hurl}

Hurl is a command line tool that runs <b>HTTP requests</b> defined in a simple <b>plain text format</b>.

It can chain requests, capture values and evaluate queries on headers and body response. Hurl is very
versatile: it can be used for both <b>fetching data</b> and <b>testing HTTP</b> sessions.

Hurl makes it easy to work with <b>HTML</b> content, <b>REST / SOAP / GraphQL</b> APIs, or any other <b>XML / JSON</b> based APIs.

---

## First, test home title.

**URL:** llms-txt#first,-test-home-title.

GET https://acmecorp.net
HTTP 200
[Asserts]
xpath "normalize-space(//head/title)" == "Hello world!"

---

## A request with query parameters section, equivalent to the first request.

**URL:** llms-txt#a-request-with-query-parameters-section,-equivalent-to-the-first-request.

GET https://example.org/forum/questions/
[Query]
search: Install Linux
order: newest
hurl
GET https://example.org/news
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:70.0) Gecko/20100101 Firefox/70.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
hurl
PATCH https://example.org/file.txt
If-Match: "e0023aa4e"
hurl
GET https://example.org

**Examples:**

Example 1 (unknown):
```unknown
> Query parameters in query parameter section are not URL encoded.

When query parameters are present in the URL and in a query parameters section, the resulting request will
have both parameters.

#### Headers {#file-format-request-headers}

Optional list of HTTP request headers.

A header consists of a name, followed by a `:` and a value.
```

Example 2 (unknown):
```unknown
> Headers directly follow URL, without any section name, contrary to query parameters, form parameters
> or cookies

Note that a header usually doesn't start with double quotes. If a header value starts with double quotes, double
quotes will be part of the header value:
```

Example 3 (unknown):
```unknown
`If-Match` request header will be sent will the following value `"e0023aa4e"` (started and ended with double quotes).

Headers must follow directly after the [method](#file-format-request-method) and [URL](#file-format-request-url).

#### Options {#file-format-request-options}

Options used to execute this request.

Options such as [`--location`](#getting-started-manual-location), [`--verbose`](#getting-started-manual-verbose), [`--insecure`](#getting-started-manual-insecure) can be used at the command line and applied to every
request of an Hurl file. An `[Options]` section can be used to apply option to only one request (without passing options
to the command line), while other requests are unaffected.
```

---

## First entry, test the redirection (status code and 'Location' header)

**URL:** llms-txt#first-entry,-test-the-redirection-(status-code-and-'location'-header)

GET https://google.fr
HTTP 301
Location: https://www.google.fr/

---

## First GET request to get CSRF token value:

**URL:** llms-txt#first-get-request-to-get-csrf-token-value:

GET https://example.org
HTTP 200

---

## Table of Contents

**URL:** llms-txt#table-of-contents

* [Introduction](#introduction)
    * [What's Hurl?](#introduction-home-whats-hurl)
    * [Also an HTTP Test Tool](#introduction-home-also-an-http-test-tool)
    * [Why Hurl?](#introduction-home-why-hurl)
    * [Powered by curl](#introduction-home-powered-by-curl)
    * [Feedbacks](#introduction-home-feedbacks)
    * [Resources](#introduction-home-resources)
* [Getting Started](#getting-started)
    * [Installation](#getting-started-installation-installation)
        * [Binaries Installation](#getting-started-installation-binaries-installation)
            * [Linux](#getting-started-installation-linux)
                * [Debian / Ubuntu](#getting-started-installation-debian--ubuntu)
                * [Alpine](#getting-started-installation-alpine)
                * [Arch Linux / Manjaro](#getting-started-installation-arch-linux--manjaro)
                * [NixOS / Nix](#getting-started-installation-nixos--nix)
            * [macOS](#getting-started-installation-macos)
                * [Homebrew](#getting-started-installation-homebrew)
                * [MacPorts](#getting-started-installation-macports)
            * [FreeBSD](#getting-started-installation-freebsd)
            * [Windows](#getting-started-installation-windows)
                * [Zip File](#getting-started-installation-zip-file)
                * [Installer](#getting-started-installation-installer)
                * [Chocolatey](#getting-started-installation-chocolatey)
                * [Scoop](#getting-started-installation-scoop)
                * [Windows Package Manager](#getting-started-installation-windows-package-manager)
            * [Cargo](#getting-started-installation-cargo)
            * [conda-forge](#getting-started-installation-conda-forge)
            * [Docker](#getting-started-installation-docker)
            * [npm](#getting-started-installation-npm)
        * [Building From Sources](#getting-started-installation-building-from-sources)
            * [Build on Linux](#getting-started-installation-build-on-linux)
                * [Debian based distributions](#getting-started-installation-debian-based-distributions)
                * [Fedora based distributions](#getting-started-installation-fedora-based-distributions)
                * [Red Hat based distributions](#getting-started-installation-red-hat-based-distributions)
                * [Arch based distributions](#getting-started-installation-arch-based-distributions)
                * [Alpine based distributions](#getting-started-installation-alpine-based-distributions)
            * [Build on macOS](#getting-started-installation-build-on-macos)
            * [Build on Windows](#getting-started-installation-build-on-windows)
    * [Manual](#getting-started-manual-manual)
        * [Name](#getting-started-manual-name)
        * [Synopsis](#getting-started-manual-synopsis)
        * [Description](#getting-started-manual-description)
        * [Hurl File Format](#getting-started-manual-hurl-file-format)
            * [Capturing values](#getting-started-manual-capturing-values)
            * [Asserts](#getting-started-manual-asserts)
        * [Options](#getting-started-manual-options)
        * [Environment](#getting-started-manual-environment)
        * [Exit Codes](#getting-started-manual-exit-codes)
        * [WWW](#getting-started-manual-www)
        * [See Also](#getting-started-manual-see-also)
    * [Samples](#getting-started-samples-samples)
        * [Getting Data](#getting-started-samples-getting-data)
            * [HTTP Headers](#getting-started-samples-http-headers)
            * [Query Params](#getting-started-samples-query-params)
            * [Basic Authentication](#getting-started-samples-basic-authentication)
            * [Passing Data between Requests ](#getting-started-samples-passing-data-between-requests)
        * [Sending Data](#getting-started-samples-sending-data)
            * [Sending HTML Form Data](#getting-started-samples-sending-html-form-data)
            * [Sending Multipart Form Data](#getting-started-samples-sending-multipart-form-data)
            * [Posting a JSON Body](#getting-started-samples-posting-a-json-body)
            * [Templating a JSON Body](#getting-started-samples-templating-a-json-body)
            * [Templating a XML Body](#getting-started-samples-templating-a-xml-body)
            * [Using GraphQL Query](#getting-started-samples-using-graphql-query)
            * [Using Dynamic Datas](#getting-started-samples-using-dynamic-datas)
        * [Testing Response](#getting-started-samples-testing-response)
            * [Testing Status Code](#getting-started-samples-testing-status-code)
            * [Testing Response Headers](#getting-started-samples-testing-response-headers)
            * [Testing REST APIs](#getting-started-samples-testing-rest-apis)
            * [Testing HTML Response](#getting-started-samples-testing-html-response)
            * [Testing Set-Cookie Attributes](#getting-started-samples-testing-set-cookie-attributes)
            * [Testing Bytes Content](#getting-started-samples-testing-bytes-content)
            * [SSL Certificate](#getting-started-samples-ssl-certificate)
            * [Checking Full Body](#getting-started-samples-checking-full-body)
        * [Reports](#getting-started-samples-reports)
            * [HTML Report](#getting-started-samples-html-report)
            * [JSON Report](#getting-started-samples-json-report)
            * [JUnit Report](#getting-started-samples-junit-report)
            * [TAP Report](#getting-started-samples-tap-report)
            * [JSON Output](#getting-started-samples-json-output)
        * [Others](#getting-started-samples-others)
            * [HTTP Version](#getting-started-samples-http-version)
            * [IP Address](#getting-started-samples-ip-address)
            * [Polling and Retry](#getting-started-samples-polling-and-retry)
            * [Delaying Requests](#getting-started-samples-delaying-requests)
            * [Skipping Requests](#getting-started-samples-skipping-requests)
            * [Testing Endpoint Performance](#getting-started-samples-testing-endpoint-performance)
            * [Using SOAP APIs](#getting-started-samples-using-soap-apis)
            * [Capturing and Using a CSRF Token](#getting-started-samples-capturing-and-using-a-csrf-token)
            * [Redacting Secrets](#getting-started-samples-redacting-secrets)
            * [Checking Byte Order Mark (BOM) in Response Body](#getting-started-samples-checking-byte-order-mark-bom-in-response-body)
            * [AWS Signature Version 4 Requests](#getting-started-samples-aws-signature-version-4-requests)
            * [Using curl Options](#getting-started-samples-using-curl-options)
    * [Running Tests](#getting-started-running-tests-running-tests)
        * [Use --test Option](#getting-started-running-tests-use-test-option)
            * [Selecting Tests](#getting-started-running-tests-selecting-tests)
        * [Debugging](#getting-started-running-tests-debugging)
            * [Debug Logs](#getting-started-running-tests-debug-logs)
            * [HTTP Responses](#getting-started-running-tests-http-responses)
        * [Generating Report](#getting-started-running-tests-generating-report)
            * [HTML Report](#getting-started-running-tests-html-report)
            * [JSON Report](#getting-started-running-tests-json-report)
            * [JUnit Report](#getting-started-running-tests-junit-report)
            * [TAP Report](#getting-started-running-tests-tap-report)
        * [Use Variables in Tests](#getting-started-running-tests-use-variables-in-tests)
    * [Frequently Asked Questions](#getting-started-frequently-asked-questions-frequently-asked-questions)
        * [General](#getting-started-frequently-asked-questions-general)
            * [Why "Hurl"?](#getting-started-frequently-asked-questions-why-hurl)
            * [Yet Another Tool, I already use X](#getting-started-frequently-asked-questions-yet-another-tool-i-already-use-x)
            * [Hurl is build on top of libcurl, but what is added?](#getting-started-frequently-asked-questions-hurl-is-build-on-top-of-libcurl-but-what-is-added)
            * [Why shouldn't I use Hurl?](#getting-started-frequently-asked-questions-why-shouldnt-i-use-hurl)
            * [I have a large numbers of tests, how to run just specific tests?](#getting-started-frequently-asked-questions-i-have-a-large-numbers-of-tests-how-to-run-just-specific-tests)
            * [How can I use my Hurl files outside Hurl?](#getting-started-frequently-asked-questions-how-can-i-use-my-hurl-files-outside-hurl)
            * [Can I do calculation within a Hurl file?](#getting-started-frequently-asked-questions-can-i-do-calculation-within-a-hurl-file)
        * [macOS](#getting-started-frequently-asked-questions-macos)
            * [How can I use a custom libcurl (from Homebrew by instance)?](#getting-started-frequently-asked-questions-how-can-i-use-a-custom-libcurl-from-homebrew-by-instance)
* [File Format](#file-format)
    * [Hurl File](#file-format-hurl-file-hurl-file)
        * [Character Encoding](#file-format-hurl-file-character-encoding)
        * [File Extension](#file-format-hurl-file-file-extension)
        * [Comments](#file-format-hurl-file-comments)
        * [Special Characters in Strings](#file-format-hurl-file-special-characters-in-strings)
    * [Entry](#file-format-entry-entry)
        * [Definition](#file-format-entry-definition)
        * [Example](#file-format-entry-example)
        * [Description](#file-format-entry-description)
            * [Options](#file-format-entry-options)
            * [Cookie storage](#file-format-entry-cookie-storage)
            * [Redirects](#file-format-entry-redirects)
            * [Retry](#file-format-entry-retry)
            * [Control flow](#file-format-entry-control-flow)
    * [Request](#file-format-request-request)
        * [Definition](#file-format-request-definition)
        * [Example](#file-format-request-example)
        * [Structure](#file-format-request-structure)
        * [Description](#file-format-request-description)
            * [Method](#file-format-request-method)
            * [URL](#file-format-request-url)
            * [Headers](#file-format-request-headers)
            * [Options](#file-format-request-options)
            * [Query parameters](#file-format-request-query-parameters)
            * [Form parameters](#file-format-request-form-parameters)
            * [Multipart Form Data](#file-format-request-multipart-form-data)
            * [Cookies](#file-format-request-cookies)
            * [Basic Authentication](#file-format-request-basic-authentication)
            * [Body](#file-format-request-body)
                * [JSON body](#file-format-request-json-body)
                * [XML body](#file-format-request-xml-body)
                * [GraphQL query](#file-format-request-graphql-query)
                * [Multiline string body](#file-format-request-multiline-string-body)
                * [Oneline string body](#file-format-request-oneline-string-body)
                * [Base64 body](#file-format-request-base64-body)
                * [Hex body](#file-format-request-hex-body)
                * [File body](#file-format-request-file-body)
    * [Response](#file-format-response-response)
        * [Definition](#file-format-response-definition)
        * [Example](#file-format-response-example)
        * [Structure](#file-format-response-structure)
        * [Capture and Assertion](#file-format-response-capture-and-assertion)
            * [Body compression](#file-format-response-body-compression)
        * [Timings](#file-format-response-timings)
    * [Capturing Response](#file-format-capturing-response-capturing-response)
        * [Captures](#file-format-capturing-response-captures)
            * [Query](#file-format-capturing-response-query)
            * [Status capture](#file-format-capturing-response-status-capture)
            * [Version capture](#file-format-capturing-response-version-capture)
            * [Header capture](#file-format-capturing-response-header-capture)
            * [Cookie capture](#file-format-capturing-response-cookie-capture)
            * [Body capture](#file-format-capturing-response-body-capture)
            * [Bytes capture](#file-format-capturing-response-bytes-capture)
            * [XPath capture](#file-format-capturing-response-xpath-capture)
            * [JSONPath capture](#file-format-capturing-response-jsonpath-capture)
            * [Regex capture](#file-format-capturing-response-regex-capture)
            * [SHA-256 capture](#file-format-capturing-response-sha-256-capture)
            * [MD5 capture](#file-format-capturing-response-md5-capture)
            * [URL capture](#file-format-capturing-response-url-capture)
            * [IP address capture](#file-format-capturing-response-ip-address-capture)
            * [Variable capture](#file-format-capturing-response-variable-capture)
            * [Duration capture](#file-format-capturing-response-duration-capture)
            * [SSL certificate capture](#file-format-capturing-response-ssl-certificate-capture)
        * [Redacting Secrets](#file-format-capturing-response-redacting-secrets)
    * [Asserting Response](#file-format-asserting-response-asserting-response)
        * [Asserts](#file-format-asserting-response-asserts)
        * [Implicit asserts](#file-format-asserting-response-implicit-asserts)
            * [Version - Status](#file-format-asserting-response-version-status)
            * [Headers](#file-format-asserting-response-headers)
        * [Explicit asserts](#file-format-asserting-response-explicit-asserts)
            * [Predicates](#file-format-asserting-response-predicates)
            * [Status assert](#file-format-asserting-response-status-assert)
            * [Version assert](#file-format-asserting-response-version-assert)
            * [Header assert](#file-format-asserting-response-header-assert)
            * [Cookie assert](#file-format-asserting-response-cookie-assert)
            * [Body assert](#file-format-asserting-response-body-assert)
            * [Bytes assert](#file-format-asserting-response-bytes-assert)
            * [XPath assert](#file-format-asserting-response-xpath-assert)
            * [JSONPath assert](#file-format-asserting-response-jsonpath-assert)
            * [Regex assert](#file-format-asserting-response-regex-assert)
            * [SHA-256 assert](#file-format-asserting-response-sha-256-assert)
            * [MD5 assert](#file-format-asserting-response-md5-assert)
            * [URL assert](#file-format-asserting-response-url-assert)
            * [IP address assert](#file-format-asserting-response-ip-address-assert)
            * [Variable assert](#file-format-asserting-response-variable-assert)
            * [Duration assert](#file-format-asserting-response-duration-assert)
            * [SSL certificate assert](#file-format-asserting-response-ssl-certificate-assert)
        * [Body](#file-format-asserting-response-body)
            * [JSON body](#file-format-asserting-response-json-body)
            * [XML body](#file-format-asserting-response-xml-body)
            * [Multiline string body](#file-format-asserting-response-multiline-string-body)
                * [Oneline string body](#file-format-asserting-response-oneline-string-body)
            * [Base64 body](#file-format-asserting-response-base64-body)
            * [File body](#file-format-asserting-response-file-body)
    * [Filters](#file-format-filters-filters)
        * [Definition](#file-format-filters-definition)
        * [Example](#file-format-filters-example)
        * [Description](#file-format-filters-description)
            * [base64Decode](#file-format-filters-base64decode)
            * [base64Encode](#file-format-filters-base64encode)
            * [count](#file-format-filters-count)
            * [daysAfterNow](#file-format-filters-daysafternow)
            * [daysBeforeNow](#file-format-filters-daysbeforenow)
            * [decode](#file-format-filters-decode)
            * [format](#file-format-filters-format)
            * [htmlEscape](#file-format-filters-htmlescape)
            * [htmlUnescape](#file-format-filters-htmlunescape)
            * [jsonpath ](#file-format-filters-jsonpath)
            * [nth](#file-format-filters-nth)
            * [regex](#file-format-filters-regex)
            * [replace](#file-format-filters-replace)
            * [split](#file-format-filters-split)
            * [toDate](#file-format-filters-todate)
            * [toFloat](#file-format-filters-tofloat)
            * [toInt](#file-format-filters-toint)
            * [toString](#file-format-filters-tostring)
            * [urlDecode](#file-format-filters-urldecode)
            * [urlEncode](#file-format-filters-urlencode)
            * [xpath](#file-format-filters-xpath)
    * [Templates](#file-format-templates-templates)
        * [Variables](#file-format-templates-variables)
        * [Functions](#file-format-templates-functions)
        * [Types](#file-format-templates-types)
        * [Injecting Variables](#file-format-templates-injecting-variables)
            * [`variable` option](#file-format-templates-variable-option)
            * [`variables-file` option](#file-format-templates-variables-file-option)
            * [Environment variable](#file-format-templates-environment-variable)
            * [Options sections](#file-format-templates-options-sections)
            * [Secrets](#file-format-templates-secrets)
        * [Templating Body](#file-format-templates-templating-body)
    * [Grammar](#file-format-grammar-grammar)
        * [Definitions](#file-format-grammar-definitions)
        * [Syntax Grammar](#file-format-grammar-syntax-grammar)
* [Resources](#resources)
    * [License](#resources-license-license)

---
