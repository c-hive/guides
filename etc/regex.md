# Etc / Good Enough™ regexes

- use non-capturing groups for [performance reasons](https://stackoverflow.com/questions/33243292/capturing-group-vs-non-capturing-group)
- [avoid greedy matching](https://mariusschulz.com/blog/why-using-the-greedy-in-regular-expressions-is-almost-never-what-you-actually-want): use lazy matching and avoid "any" matchers
- [lookbehinds are only supported in the ECMA2018 standard](https://stackoverflow.com/a/50011952/2771889), at the moment the  browser support is very limited

## URLs

- Optional port: `(?::\d{1,5})?`
- Optional http scheme: `(?:http(?:s)?:\/\/)?`

### Development URLS

`^(?:http(?:s)?:\/\/)?(?:localhost|127\.0\.0\.1|0\.0\.0\.0)(?::\d{1,5})?$`

https://regex101.com/r/accq7h/4/tests

### Popular deployment options

`^(?:http(?:s)?:\/\/)?[a-zA-Z0-9-]{1,63}\.gitlab\.io$`

`^(?:http(?:s)?:\/\/)?[a-zA-Z0-9-]{1,63}\.github\.io$`

`^(?:http(?:s)?:\/\/)?[a-zA-Z0-9-]{1,63}\.herokuapp\.com$`

### Domains

There're two approaches to choose from when validating domains.

By-the-books FQDN matching (theoretical definition, rarely encountered in practice):
- max 253 character long (as per [RFC-1035/3.1](https://tools.ietf.org/html/rfc1035), [RFC-2181/11](https://tools.ietf.org/html/rfc2181#section-11))
- max 63 character long per label (as per [RFC-1035/3.1](https://tools.ietf.org/html/rfc1035), [RFC-2181/11](https://tools.ietf.org/html/rfc2181#section-11))
- any characters are allowed (as per [RFC-2181/11](https://tools.ietf.org/html/rfc2181#section-11))
- TLDs cannot be all-numeric (as per [RFC-3696/2](https://tools.ietf.org/html/rfc3696#section-2))
- FQDNs can be written in a complete form, which includes the root zone (the trailing dot)

Practical / conservative FQDN matching (practical definition, expected and supported in practice):
- by-the-books matching with the following exceptions/additions
- valid characters: `[a-zA-Z0-9.-]`
- labels cannot start or end with hyphens (as per [RFC-952](https://tools.ietf.org/html/rfc952) and [RFC-1123/2.1](https://tools.ietf.org/html/rfc1123#section-2.1))
- TLD min length is 2 character, max length is 24 character as per currently existing records
- don't match trailing dot

Regex for the practical use case:

`^(?!.*?_.*?)(?!(?:[\d\w]+?\.)?\-[\w\d\.\-]*?)(?![\w\d]+?\-\.(?:[\d\w\.\-]+?))(?=[\w\d])(?=[\w\d\.\-]*?\.+[\w\d\.\-]*?)(?![\w\d\.\-]{254})(?!(?:\.?[\w\d\-\.]*?[\w\d\-]{64,}\.)+?)[\w\d\.\-]+?(?<![\w\d\-\.]*?\.[\d]+?)(?<=[\w\d\-]{2,})(?<![\w\d\-]{25})$`

https://regex101.com/r/FLA9Bv/9 (_Note: currently only works in Chrome because the regex uses lookbehinds which are only supported in ECMA2018_)

See also: [TLD limitations](https://stackoverflow.com/questions/7411255/is-it-possible-to-have-one-single-character-top-level-domain-name), [domain limitations](https://stackoverflow.com/questions/32290167/what-is-the-maximum-length-of-a-dns-name/32294443), [list of TLDs](http://data.iana.org/TLD/tlds-alpha-by-domain.txt), [by-the-books regex and explanation](https://regexr.com/3g5j0)

## Semantic version

`^v[0-9]+\.[0-9]+\.[0-9]+$`

## Argument list

Extracts valid arguments from comma-separated argument list, supporting double-quoted arguments

`(?<=")[^"]+?(?="(?:\s*?,|\s*?$))|(?<=(?:^|,)\s*?)(?:[^,"\s][^,"]*[^,"\s])|(?:[^,"\s])(?![^"]*?"(?:\s*?,|\s*?$))(?=\s*?(?:,|$))`

https://regex101.com/r/UL8kyy/3/tests (_Note: currently only works in Chrome because the regex uses lookbehinds which are only supported in ECMA2018_)
