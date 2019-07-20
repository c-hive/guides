# Etc / Good Enough™ regexes

- use non-capturing groups for [performance reasons](https://stackoverflow.com/questions/33243292/capturing-group-vs-non-capturing-group)
- [avoid greedy matching](https://mariusschulz.com/blog/why-using-the-greedy-in-regular-expressions-is-almost-never-what-you-actually-want): use lazy matching and avoid "any" matchers

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

By-the-books FQDN matching:
- max 253 character long (as per [RFC-2181/11](https://tools.ietf.org/html/rfc2181#section-11)
- max 63 character long per label (as per [RFC-2181/11](https://tools.ietf.org/html/rfc2181#section-11))
- any characters are allowed (as per [RFC-2181/11](https://tools.ietf.org/html/rfc2181#section-11))
- TLDs cannot be all-numeric (as per [RFC-3696/2](https://tools.ietf.org/html/rfc3696#section-2))

Practical / conservative FQDN matching
- valid characters: `[a-zA-Z0-9.-]`
- labels cannot start or end with hyphens (as per [RFC-952](https://tools.ietf.org/html/rfc952) and [RFC-1123/2.1](https://tools.ietf.org/html/rfc1123#section-2.1))
- TLD min length is 2 character, max length is 24 character as per currently existing records
- see also: [TLD limitations](https://stackoverflow.com/questions/7411255/is-it-possible-to-have-one-single-character-top-level-domain-name), [domain limitations](https://stackoverflow.com/questions/32290167/what-is-the-maximum-length-of-a-dns-name/32294443), [list of TLDs](http://data.iana.org/TLD/tlds-alpha-by-domain.txt)

`^(?!.*?_.*?)(?!(?:[\d\w]+?\.)?\-[\w\d\.\-]*?)(?![\w\d]+?\-\.(?:[\d\w\.\-]+?))(?=[\w\d])(?=[\w\d\.\-]*?\.+[\w\d\.\-]*?)(?![\w\d\.\-]{254})(?!(?:\.?[\w\d\-\.]*?[\w\d\-]{64,}\.)+?)[\w\d\.\-]+?(?<![\w\d\-\.]*?\.[\d]+?)(?<=[\w\d\-]{2,})(?<![\w\d\-]{25})$`

https://regex101.com/r/FLA9Bv/5

## Semantic version

`^v[0-9]\.[0-9]\.[0-9]$`

## Argument list

Extracts valid arguments from comma-separated argument list, supporting double-quoted arguments

`(?<=")[^"]+?(?="(?:\s*?,|\s*?$))|(?<=(?:^|,)\s*?)(?:[^,"\s][^,"]*[^,"\s])|(?:[^,"\s])(?![^"]*?"(?:\s*?,|\s*?$))(?=\s*?(?:,|$))`

https://regex101.com/r/UL8kyy/3/tests
