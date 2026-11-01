---
title: "DateParseKit"
summary: "Strict explicit-offset timestamps with exact millisecond UTC instants."
category: "Overview"
order: 0
keywords: ["timestamp", "RFC3339", "ISO8601", "parse", "UTC"]
related: ["getting-started.md", "api.md", "typed-api.md"]
---

# DateParseKit

Import dateparsekit for strict Gregorian extended-calendar timestamps with an
explicit numeric offset or Z. Parsing is independent of locale and native date
parsers. Results retain the original text, numeric offset and fractional digit
count alongside exact signed UTC milliseconds and a built-in DateTime instant.
Portable UTC calendar properties and canonical ToISO8601 rendering avoid native
calendar limits for pre-1970 dates and years below 1000.

The complete input must match the supported profile. Nonzero sub-millisecond
precision, unknown offsets and leap seconds are rejected instead of being silently
rounded or reinterpreted. See the [API reference](api.md).
