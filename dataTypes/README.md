# Supported Data Types

### BOOLEAN

Captures boolean values `true` or `false`

### Integer

- `tinyint` - 8 bit signed integer with range -128 to 127

- `smallint` - 16 bit signed integer with range 32,768 to 32,767

- `integer` - 32 bit signed integer with range -4,294,967,296 to 4,294,967,297

- `bigint` - 64 bit signed integer with range -18,446,744,073,709,551,616 to 18,446,744,073,709,551,615

### Floating Point

- `real` - 32 bit inexact, variable-precision implementing the IEEE Standard 754 for Binary Floating-Point Arithmetic.

- `double` - 64-bit inexact, variable-precision implementing the IEEE Standard 754 for Binary Floating-Point Arithmetic.

### Fixed Precision

- `decimal` - Precision up to 38 digits but performance is best up to 18 digits

### String

- `varchar` - Variable length character data with optional length between 1 and 65535 characters

    Example: `VARCHAR` , `VARCHAR(20)`

- `char` - Fixed length character data. Defaults to 1. CHAR(x) value always has x characters which means that trailing spaces are added if length of value is less than x.

    Example: `CHAR`, `CHAR(10)`

- `json` - Can be JSON object, JSON array, JSON number, JSON string, true, false or null.

- `varbinary` - Variable length Binary Data. Only used to store Well-Known Binary for Spatial Data.

### Date and Time

- `date` - Calendar date (year, month day)

    Example: `DATE '2019-05-01'`

- `time` (with or without timezone) -

    - Time of day (hour, month, second, millisecond) without a timezone.

        Example: `TIME '01:02:03.456'`

    - Time of day (hour, month, second, millisecond) with a timezone.

        Example: `TIME '01:02:03.456 America/Los_Angeles'`

- `timestamp` -
    - Instant in time that includes date and time of day without timezone.

        Example: `TIMESTAMP '2000-01-01 00:00:00.00'`

    - Instant in time that includes date and time of day with timezone.

        Example: `TIMESTAMP '2000-01-01 00:00:00.00 America/Los_Angeles'`

### Complex

- ARRAY - An array/list of given component.

    Example: `ARRAY[1, 2, 3, 4]`

- MAP - A key value map between given component.

    Example: `MAP(VARCHAR, VARCHAR)`



Back to [Tables of Content](../README.md#getting-started-guide-to-host)
