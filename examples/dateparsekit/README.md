# DateParseKit timestamp boundaries

This example normalizes a Nepal-offset New Year's timestamp into the previous
UTC year, preserves an exact pre-epoch millisecond, and handles two invalid
inputs. Use `ParseTimestampBytes` for untrusted payloads before converting them
to String: conversion may discard a NUL and everything after it. Use
`RequireTimestamp` when an `InvalidArgumentException` fits the caller's error
handling; `ParseTimestamp` instead returns a Result with the rejection reason.

From the repository root:

```sh
example_dir=$(mktemp -d)
mkdir "$example_dir/packages"
cp examples/dateparsekit/main.bui "$example_dir/main.bui"
cp packages/dateparsekit-0.1.0.buipkg "$example_dir/packages/"
(cd "$example_dir" && buoy main.bui --check --diagnostic-format json && buoy main.bui -o app && ./app)
```

Expected output:

```text
2027-01-01T00:15:00+05:45
2026-12-31T18:30:00.000Z
-1
Rejected unsafe wire timestamp
Rejected unknown offset
```

Maintainers run the private validation suite before release. See the repository README for public build instructions.
