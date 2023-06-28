# Time Format Codes

Output:

* ISO Date
  
  `%Y-%m-%d`
  
* ISO DateTime: 

  `%Y-%m-%dT%H:%M:%SZ`

* ISO DateTime with offset: 

  `%Y-%m-%dT%H:%M:%S%z`

## Year

| Code  | Description                        | Output           |
| :---- | :--------------------------------- | ---------------- |
| `%Y`  | Full Year                          | 1999, 2023, etc. |
| `%-y` | Year without century               | 0-99             |
| `%y`  | Year without century (zero-padded) | 00-99            |
| `%C`  | Century                            | 19,18,20         |


## Month

| Code           | Description           | Output                  |
| :------------- | :-------------------- | ----------------------- |
| `%B`[^*]       | Name                  | January, February, etc. |
| `%b`, `%h`[^*] | Short Name            | Jan, Feb, etc.          |
| `%-m`          | Decimal               | 1-12                    |
| `%m`           | Decimal (zero-padded) | 01-12                   |

## Date

| Code  | Description           | Output                 |
| :---- | :-------------------- | ---------------------- |
| `%-d` | Decimal               | 1, 2, 30, etc.         |
| `%d`  | Decimal (zero-padded) | 01, 02, 31, etc.       |
| `%e`  | Decimal(space-padded) | ` 1`, ` 2`, `22`, etc. |


## Hour

| Code  | Description           | Output |
| :---- | :-------------------- | ------ |
| `%-H` | 24 Hour               | 0-23   |
| `%H`  | 24 Hour (zero-padded) | 00-23  |
| `%-I` | 12 Hour               | 1-12   |
| `%I`  | 12 Hour (zero-padded) | 01-12  |

## Minute

| Code  | Description           | Output |
| :---- | :-------------------- | ------ |
| `%-M` | Decimal               | 0-59   |
| `%M`  | Decimal (zero-padded) | 00-59  |

## Second

| Code  | Description           | Output |
| :---- | :-------------------- | ------ |
| `%-S` | Decimal               | 0-60   |
| `%S`  | Decimal (zero-padded) | 00-60  |

## Microsecond

| Code | Description           | Output                 |
| :--- | :-------------------- | ---------------------- |
| `%f` | Decimal (zero-padded) | 000000, 000457, 999999 |

## Timezone

| Code | Description | Output       |
| :--- | :---------- | ------------ |
| `%Z` | Name        | AEST, PST    |
| `%z` | Offset      | +1000, -0700 |

## Meridiem

| Code     | Description | Output |
| :------- | :---------- | ------ |
| `%p`[^*] | Name        | AM, PM |


## Weekday

| Code     | Description            | Output                       |
| :------- | :--------------------- | ---------------------------- |
| `%A`[^*] | Name                   | Sunday, Monday, Friday, etc. |
| `%a`[^*] | Short Name             | Sun, Mon, Fri, etc.          |
| `%w`     | Decimal (zero-indexed) | 0-6                          |
| `%u`     | Decimal (one-indexed)  | 1-7                          |

## Day of the Year

| Code  | Description           | Output  |
| :---- | :-------------------- | ------- |
| `%-j` | Decimal               | 1-366   |
| `%j`  | Decimal (zero-padded) | 001-366 |


## Week

| Code           | Description                   | Output |
| :------------- | :---------------------------- | ------ |
| `%U`, `%G`[^†] | Decimal (Sunday as first day) | 00-53  |
| `%W`[^†]       | Decimal (Monday as first day) | 00-53  |
| '`%V`[^‡]      | ISO 8601 Week of the year     | 01-53  |

## Predefined Formats

Note: These will vary widely between different locales

| Code           | Description                  | Output                       |
| :------------- | :--------------------------- | ---------------------------- |
| `%x`[^*]       | Date                         | 01/26/1983, 26/01/2021, etc. |
| `%X`[^*]       | Time                         | 07:06:05                     |
| `%c`, `%+`[^*] | Date and Time                | Thu 15 Jun 10:49:50 2023     |
| `%D`           | Date                         | 26/01/2021 (%m/%d/%y)        |
| `%F`           | ISO Date                     | 2021-01-26                   |
| `%r`[^*]       | 12 Hour time with am/pm      | 11:00:22 am                  |
| `%R`           | 24 Hour time without seconds | 15:01                        |
| `%T`           | 24 Hour time with seconds    | 15:01:23                     |
| `%s`           | Seconds since the epoch      | 1686791574                   |

## Escape Characters

| Code      | Description       | Output   |
| :-------- | :---------------- | -------- |
| `%n`      | Newline           | `\n`     |
| `%t`      | Tab               | `\t`     |
| `%{char}` | Literal character | `{char}` |


## Footnotes

[^*]: * This value is locale specific
[^†]: † All days preceding the first instance of the "first day" are considered week 00.
[^‡]: ‡ If the week (starting on Monday) containing 1 January has four or more days in the new year, then it is considered week 1. Otherwise, it is the last week of the previous year, and the next week is week 1.
