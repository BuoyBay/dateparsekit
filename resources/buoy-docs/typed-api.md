---
title: "dateparsekit typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["ParsedTimestamp"]
dependencies: []
symbols: ["ParsedTimestamp"]
related: ["api.md", "getting-started.md", "index.md"]
---

# dateparsekit typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## ParsedTimestamp

`Public Class ParsedTimestamp`

A validated timestamp, retaining source offset and precision alongside its UTC instant.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `OriginalText` | `String` | Yes | No | — | — |
| `UnixMillis` | `Integer` | Yes | No | — | — |
| `Instant` | `DateTime` | Yes | No | — | — |
| `OffsetSeconds` | `Integer` | Yes | No | — | — |
| `FractionDigits` | `Integer` | Yes | No | — | — |
| `UTCYear` | `Integer` | Yes | No | — | — |
| `UTCMonth` | `Integer` | Yes | No | — | — |
| `UTCDay` | `Integer` | Yes | No | — | — |
| `UTCHour` | `Integer` | Yes | No | — | — |
| `UTCMinute` | `Integer` | Yes | No | — | — |
| `UTCSecond` | `Integer` | Yes | No | — | — |
| `UTCMillisecond` | `Integer` | Yes | No | — | — |

### ToISO8601

`Public Function ToISO8601() As String`

Canonical UTC rendering using portable Gregorian fields, not native DateTime formatting.

No parameters.

#### Returns

| Type |
| --- |
| `String` |

## DateParseKit functions

### ParseTimestamp

`Public Function ParseTimestamp(text As String) As Result Of ParsedTimestamp, String`

Parse an exact extended-calendar timestamp; invalid/unsupported forms return Err(String).

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Result Of ParsedTimestamp, String` |

### ParseTimestampBytes

`Public Function ParseTimestampBytes(data As Bytes) As Result Of ParsedTimestamp, String`

Validate every wire byte before conversion, including embedded/trailing NUL.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `data` | `Bytes` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Result Of ParsedTimestamp, String` |

### RequireTimestamp

`Public Function RequireTimestamp(text As String) As ParsedTimestamp`

Throw InvalidArgumentException with the parse reason instead of returning Err.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `text` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `ParsedTimestamp` |
