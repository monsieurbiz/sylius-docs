# Use localized dates in twig template

## Localized date
It's possible to use a localized date out of the box. Let's say in your database you have a `datetime` field containing
`2018-12-30 14:20:30`.

In your twig template if you do: `{{ foo.createdAt | format_date }}` it would output `30 déc. 2018`.

What if you want to output the complete month?
`{{ foo.createdAt | format_date(null, foo.translation.locale, null, constant('IntlDateFormatter::LONG')) }}` would output
`30 décembre 2018`.

- first parameter is the pattern.
- second parameter is the locale.
- third parameter is the timezone.
- forth parameter is the date type to use if no pattern has been provided. Possible constant values are `SHORT`, `MEDIUM`,
`LONG`, `FULL`. If `null` is provided the defaut value `MEDIUM` will be used.

In this example there is no pattern but a date type is used.

## Localized date with localized time

What if you want to add the time to the date?

`{{ resource.createdAt | format_datetime(null, resource.translation.locale, null, constant('IntlDateFormatter::LONG'), constant('IntlDateFormatter::SHORT')) }}`

`format_datetime()` was used instead of `format_date()`.
- first parameter is the pattern.
- second parameter is the locale.
- third parameter is the timezone.
- forth parameter is the date type to use if no pattern has been provided. Possible constant values are `SHORT`, `MEDIUM`,
`LONG`, `FULL`. If `null` is provided the defaut value `MEDIUM` will be used.
- fifth parameter is the time type to use if no pattern has been provided. Possible constant values are `SHORT`, `MEDIUM`,
`LONG`, `FULL`. If `null` is provided the defaut value `MEDIUM` will be used.

In this example there is no pattern but a date type & time type are used.