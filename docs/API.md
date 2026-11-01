# DateParseKit 0.1.0

Strict timestamp parsing for Buoy, returning an exact UTC instant together with
the source offset and fractional precision. Copy `dateparsekit-0.1.0.buipkg` into
a consumer's packages directory, then `Import dateparsekit`. No other package,
native parser, locale setting or operating-system timezone database is required.

| API | Contract |
| --- | --- |
| `ParseTimestamp(text)` | `Result Of ParsedTimestamp, String`; bad inputs return a nonempty reason |
| `ParseTimestampBytes(data)` | Same result, validating every wire byte before String conversion; rejects Nil and NUL/control/non-ASCII bytes |
| `RequireTimestamp(text)` | Return ParsedTimestamp or throw InvalidArgumentException with the same reason |
| `ParsedTimestamp.UnixMillis` | Exact signed UTC milliseconds since 1970-01-01 |
| `Instant` | Fresh built-in DateTime with UTC presentation and the same exact milliseconds |
| `OffsetSeconds` | Original numeric local-minus-UTC offset; Z/z is zero |
| `FractionDigits` | Number of supplied fractional digits, zero when absent |
| `OriginalText` | Exact original input, retaining spelling and offset |
| `UTCYear`, `UTCMonth`, `UTCDay`, `UTCHour`, `UTCMinute`, `UTCSecond`, `UTCMillisecond` | Portable UTC calendar fields computed directly from the exact milliseconds |
| `ToISO8601()` | Canonical UTC text with a four-digit year, three millisecond digits and Z |

All ParsedTimestamp properties are read-only. Parser arithmetic uses Gregorian
calendar rules directly rather than native strptime or permissive DateTime
parsing. The source input and resulting UTC instant must both fall in years
0001 through 9999. Numeric offsets can cross day, month and year boundaries.
UTC fields and ToISO8601 also use portable Gregorian arithmetic for the entire
range. Instant is a numeric interoperability convenience: Buoy 0.72's native
Windows DateTime calendar accessors cannot represent every pre-1970 instant,
and built-in ISO formatting does not pad years below 1000 on Linux. Use the
package's UTC fields/formatter for portable calendar presentation.

## Accepted profile

The supported ISO 8601 extended-calendar/RFC 3339 profile is:

`YYYY-MM-DD[Tt]HH:MM:SS[.fraction](Z|z|+HH:MM|-HH:MM)`

Exactly four ASCII year digits, exact two-digit components, mandatory seconds,
and an explicit offset are required. Gregorian dates are checked, including
century leap rules. Hours are 00..23; minutes/seconds are 00..59. Numeric offset
hours are 00..23 and minutes 00..59, as permitted by the RFC 3339 grammar; the
offset is retained independently of Buoy's narrower fixed-TimeZone factory.

Fractions have 1..9 ASCII digits. One/two digits are padded to milliseconds.
Digits beyond the third are accepted **only when zero**: `.123000` is exact,
whereas `.123001` returns a precision error. No rounding or silent truncation is
performed. The original precision is retained even when trailing digits are zero.

Leap seconds and `24:00` are rejected. RFC 3339 `-00:00` denotes an unknown local
offset and is deliberately rejected instead of being relabeled as a known offset.
Use Z/z or +00:00 for known UTC. Year 0000, expanded/basic/ordinal/week dates,
offset seconds, named zones, date-only inputs, comma fractions, local timestamps,
spaces, surrounding whitespace and trailing content are outside this profile.
The parser consumes the complete input, with a maximum length of 35 characters.
This is a documented subset, not an implementation of every ISO 8601 production
or RFC 3339's leap-second/unknown-offset forms.

For untrusted file/network bytes, use ParseTimestampBytes before converting to
String. Buoy's String conversion can discard content starting at a NUL byte;
ParseTimestamp cannot recover bytes already lost by that conversion. The byte
entrypoint validates the complete 20..35-byte printable-ASCII payload first,
including embedded/trailing NUL and controls. Parsed values retain no mutable
caller-owned byte buffer.

## Verification and build

```sh
/usr/bin/buoy pkg seal . -o packages/dateparsekit-0.1.0.buipkg
```

The verifier seals a fresh isolated package, compiler-checks module and consumer
for all six desktop targets, and runs Linux regression tests plus 4,096
deterministic randomized comparisons with Python datetime. Boundary vectors cover
years 0001/9999, pre-epoch values, month/year transitions, zero fractions and large
offsets. Invalid cases cover leap dates, leap seconds, unknown offsets, precision,
ASCII lookalikes, embedded NUL, trailing controls and suffix garbage. Tests compare
exact milliseconds, portable UTC calendar properties and canonical UTC text
without relying on built-in calendar accessors or date formatting. The same
4,153-case consumer passes native macOS ARM64 and Windows x86_64. Reproduce with
`tests/standard_library_macos.py` and `tests/standard_library_windows.py`.

`BUOY` and `BPM_PACKAGER` override the compiler and sealer. The implementation is
new Buoy code informed by the public [RFC 3339](https://www.rfc-editor.org/rfc/rfc3339.txt)
format and Gregorian arithmetic; no declaresub implementation is copied.

Test suites and fixtures are maintainer-local and are not part of this public repository.
