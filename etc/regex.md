# Etc / Good Enough™ regexes

- use non-capturing groups for [performance reasons](https://stackoverflow.com/questions/33243292/capturing-group-vs-non-capturing-group)
- [avoid greedy matching](https://mariusschulz.com/blog/why-using-the-greedy-in-regular-expressions-is-almost-never-what-you-actually-want), use lazy matching or "more of something" quantifiers

## URLs

- Optional port: `(?::\d{1,5})?`
- Optional http scheme: `(?:http(?:s)?:\/\/)?`

### Development URLS

`^(?:http(?:s)?:\/\/)?(?:localhost|127\.0\.0\.1|0\.0\.0\.0)(?::\d{1,5})?$`

https://regex101.com/r/accq7h/3/tests

### Popular deployment options

`^(?:http(?:s)?:\/\/)?[a-zA-Z0-9-]{1,63}\.gitlab\.io$`

`^(?:http(?:s)?:\/\/)?[a-zA-Z0-9-]{1,63}\.github\.io$`

`^(?:http(?:s)?:\/\/)?[a-zA-Z0-9-]{1,63}\.herokuapp\.com$`

## Semantic version

`^v[0-9]\.[0-9]\.[0-9]$`
