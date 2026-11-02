# Strict Gregorian timestamps

Strict timestamp parsing, returning an exact UTC instant together with
the source offset and fractional precision. Copy `dateparsekit-0.1.0.buipkg` into
a consumer's packages directory, then `Import dateparsekit`. No other package,
native parser, locale setting or operating-system timezone database is required.

All ParsedTimestamp properties are read-only. Parser arithmetic uses Gregorian
calendar rules directly rather than native strptime or permissive DateTime
parsing. The source input and resulting UTC instant must both fall in years
0001 through 9999. Numeric offsets can cross day, month and year boundaries.
UTC fields and ToISO8601 also use portable Gregorian arithmetic for the entire
range. Instant is a numeric interoperability convenience: Buoy 0.72's native
Windows DateTime calendar accessors cannot represent every pre-1970 instant,
and built-in ISO formatting does not pad years below 1000 on Linux. Use the
package's UTC fields/formatter for portable calendar presentation.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add dateparsekit --git https://github.com/BuoyBay/dateparsekit.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## API entrypoints

- `Import dateparsekit`
- `ParseTimestamp(text)`
- `ParseTimestampBytes(data)`
- `RequireTimestamp(text)`
- `Instant`

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer dateparsekit
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```


## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
