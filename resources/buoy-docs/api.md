---
title: "DateParseKit API"
summary: "Timestamp results, supported grammar and error behavior."
category: "Reference"
order: 20
keywords: ["timestamp", "RFC3339", "precision", "offset"]
symbols: ["ParseTimestamp", "ParseTimestampBytes", "RequireTimestamp", "ParsedTimestamp", "ParsedTimestamp.Instant", "ParsedTimestamp.UnixMillis", "ParsedTimestamp.OffsetSeconds", "ParsedTimestamp.FractionDigits", "ParsedTimestamp.OriginalText", "ParsedTimestamp.UTCYear", "ParsedTimestamp.UTCMonth", "ParsedTimestamp.UTCDay", "ParsedTimestamp.UTCHour", "ParsedTimestamp.UTCMinute", "ParsedTimestamp.UTCSecond", "ParsedTimestamp.UTCMillisecond", "ParsedTimestamp.ToISO8601"]
related: ["index.md", "getting-started.md", "typed-api.md"]
---

# API reference

ParseTimestamp(text As String) returns Result Of ParsedTimestamp, String.
RequireTimestamp(text As String) returns the same parsed value or throws
InvalidArgumentException carrying the reason. Errors identify syntax, calendar,
offset, precision or range problems. No input is trimmed or partially consumed.

ParseTimestampBytes(data As Bytes) returns the same Result and validates every
byte as printable ASCII before String conversion; Nil, NUL and control bytes
return errors. Use this for untrusted wire data because Buoy String conversion
may discard content at a NUL before a String parser can observe it. Returned
values retain no mutable caller-owned buffer.

Read-only ParsedTimestamp fields are OriginalText, UnixMillis, OffsetSeconds,
FractionDigits and Instant. Instant is a fresh DateTime presented in UTC and has
the same exact milliseconds as UnixMillis. OffsetSeconds retains the original
local-minus-UTC numeric offset; Z/z is known zero. FractionDigits is 0..9.

UTCYear, UTCMonth, UTCDay, UTCHour, UTCMinute, UTCSecond and UTCMillisecond are
read-only portable Gregorian calendar fields covering the complete supported
range. ToISO8601() renders canonical UTC text with a four-digit year, three
fraction digits and Z. These operations do not use native DateTime accessors.
Instant remains exact for numeric milliseconds; for calendar presentation use
the package fields/formatter because Buoy 0.72's Windows DateTime accessors
have pre-1970 limits and its Linux formatter omits padding for years below 1000.

Supported form: `YYYY-MM-DD[Tt]HH:MM:SS[.fraction](Z|z|+HH:MM|-HH:MM)`.
Digits must be ASCII. Gregorian years are 0001..9999, including the resulting
UTC instant. Offsets have hours 00..23 and minutes 00..59. Fractions have 1..9
digits, with every digit beyond the third required to be zero. Milliseconds are
computed exactly; rounding is not performed.

Leap seconds, 24:00, unknown `-00:00` offset, year 0000, spaces/whitespace,
trailing data, local/date-only inputs, basic/week/ordinal dates and offset seconds
are rejected. Inputs are at most 35 characters. This profile is a subset of ISO
8601/RFC3339, not a general locale or timezone-name parser.

[Overview](index.md)


[Full typed API: properties, parameters, defaults, and returns](typed-api.md)
