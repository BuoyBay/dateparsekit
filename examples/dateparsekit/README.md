# DateParseKit timestamp boundaries

Normalizes a Nepal-offset New Year timestamp, preserves a pre-epoch millisecond and rejects unsafe timestamps. Use `ParseTimestampBytes` for untrusted raw input before constructing Strings.

From this repository root, with Buoy and BPM installed:

```sh
package_source="$PWD"
example_dir=$(mktemp -d)
cp examples/dateparsekit/main.bui "$example_dir/main.bui"
(cd "$example_dir" && bpm init && bpm add dateparsekit --path "$package_source" && bpm build main.bui -o app && ./app)
```

BPM builds from this checkout's package source and installs its declared
dependencies into the consumer project.
