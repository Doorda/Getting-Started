# Functions and Operators

## Logical Operators
1) Comparison Operators

    | Operator | Description | Example |
    |-----|------------------------------|---------|
    | AND | True if both values are true | a AND b |
    | OR  | True if either value is true | a OR b  |
    | NOT | True if the value is false   | NOT a   |

2) Comparison Operators

    | Operator | Description           |
    |----------|-----------------------|
    | <        | Less than             |
    | >        | Greater than          |
    | <=       | Less than or equal to |
    | >=       | More than or equal to |
    | =        | Equal                 |
    | <>       | Not equal             |
    | !=       | Not equal             |

## Conversion Functions

- **`cast`**(*value AS type*) → type

    Explicitly cast a value as a type. This can be used to cast a varchar to a numeric value type and vice versa.

- **`try_cast`**(*value AS type*) → type

    Like **`cast()`**, but returns null if the cast fails.

## Data and Time Functions/Operators

1) Operators

    | ﻿Operator | Example                                           | Result                  |
    |----------|---------------------------------------------------|-------------------------|
    | +        | date '2012-08-08' + interval '2' day              | 10/08/2012              |
    | +        | time '01:00' + interval '3' hour                  | 00:00.0                 |
    | +        | timestamp '2012-08-08 01:00' + interval '29' hour | 2012-08-09 06:00:00.000 |
    | +        | timestamp '2012-10-31 01:00' + interval '1' month | 2012-11-30 01:00:00.000 |
    | +        | interval '2' day + interval '3' hour              | 2 03:00:00.000          |
    | +        | interval '3' year + interval '5' month            | 03-May                  |
    | -        | date '2012-08-08' - interval '2' day              | 06/08/2012              |
    | -        | time '01:00' - interval '3' hour                  | 00:00.0                 |
    | -        | timestamp '2012-08-08 01:00' - interval '29' hour | 2012-08-06 20:00:00.000 |
    | -        | timestamp '2012-10-31 01:00' - interval '1' month | 2012-09-30 01:00:00.000 |
    | -        | interval '2' day - interval '3' hour              | 1 21:00:00.000          |
    | -        | interval '3' year - interval '5' month            | 02-Jul                  |

2) Functions

    `date_trunc` function supports the following units:
    
    | ﻿Unit    | Example Truncated Value |
    |---------|-------------------------|
    | second  | 2001-08-22 03:04:05.000 |
    | minute  | 2001-08-22 03:04:00.000 |
    | hour    | 2001-08-22 03:00:00.000 |
    | day     | 2001-08-22 00:00:00.000 |
    | week    | 2001-08-20 00:00:00.000 |
    | month   | 2001-08-01 00:00:00.000 |
    | quarter | 2001-07-01 00:00:00.000 |
    | year    | 2001-01-01 00:00:00.000 |
    
    The above examples use the timestamp `2001-08-22 03:04:05.321` as the input.

    - **`date_trunc`**(*unit*, *x*) → [same as input]
    
        Returns `x` truncated to `unit`.

## Aggregate/Statistical

- **`avg`**(*x*) → double

    Returns the average (arithmetic mean) of all input values.

- **`count`**(***) → bigint

    Returns the number of input rows.

- **`max`**(*x*, *n*) → array<[same as x]>

    Returns `n` largest values of all input values of `x`.

- **`min`**(*x*) → [same as input]

    Returns the minimum value of all input values.

- **`min`**(*x*, *n*) → array<[same as x]>

    Returns `n` smallest values of all input values of `x`.

- **`sum`**(*x*) → [same as input]

    Returns the sum of all input values.

- **`histogram`**(*x) -> map(K*, *bigint*)

    Returns a map containing the count of the number of times each input value occurs.

- **`corr`**(*y*, *x*) → double

    Returns correlation coefficient of input values.

    Example:

        // Getting trend of crime in a particular postcode
        SELECT a.postcode, corr(a.period, a.crime_count)
        FROM (SELECT postcode, period, count(*) as crime_count FROM eng_area_reported_crime where postcode = 'E3 4RL' GROUP BY postcode, period) a
        group by a.postcode;

- **`covar_pop`**(*y*, *x*) → double

    Returns the population covariance of input values.

- **`covar_samp`**(*y*, *x*) → double

    Returns the sample covariance of input values.

- **`kurtosis`**(*x*) → double

    Returns the excess kurtosis of all input values.

- **`regr_intercept`**(*y*, *x*) → double

    Returns linear regression intercept of input values. `y` is the dependent value. `x` is the independent value.

- **`regr_slope`**(*y*, *x*) → double

    Returns linear regression slope of input values. `y` is the dependent value. `x` is the independent value.

- **`skewness`**(*x*) → double

    Returns the skewness of all input values.

- **`stddev_pop`**(*x*) → double

    Returns the population standard deviation of all input values.

- **`stddev_samp`**(*x*) → double

    Returns the sample standard deviation of all input values.

- **`var_pop`**(*x*) → double

    Returns the population variance of all input values.

- **`var_samp`**(*x*) → double

    Returns the sample variance of all input values.

## Array

1) Subscript Operator: `[]`

    The `[]` operator is used to access an element of an array and is indexed starting from one:

        SELECT my_array[1] AS first_element

2) Concatenation Operator: `||`

    The `||` operator is used to concatenate an array with an array or an element of the same type:

        SELECT ARRAY [1] || ARRAY [2]; -- [1, 2]
        SELECT ARRAY [1] || 2; -- [1, 2]
        SELECT 2 || ARRAY [1]; -- [2, 1]

3) Functions

    - **`array_distinct`**(*x*) → array
    
        Remove duplicate values from the array `x`.
    
    - **`array_intersect`**(*x*, *y*) → array
    
        Returns an array of the elements in the intersection of `x` and `y`, without duplicates.
    
    - **`array_union`**(*x*, *y*) → array
    
        Returns an array of the elements in the union of `x` and `y`, without duplicates.
    
    - **`array_except`**(*x*, *y*) → array
    
        Returns an array of elements in `x` but not in `y`, without duplicates.
    
    - **`array_join`**(*x*, *delimiter*, *null_replacement*) → varchar
    
        Concatenates the elements of the given array using the delimiter and an optional string to replace nulls.
    
    - **`array_max`**(*x*) → x
    
        Returns the maximum value of input array.
    
    - **`array_min`**(*x*) → x
    
        Returns the minimum value of input array.
    
    - **`array_position`**(*x*, *element*) → bigint
    
        Returns the position of the first occurrence of the `element` in array `x` (or 0 if not found).
    
    - **`array_remove`**(*x*, *element*) → array
    
        Remove all elements that equal `element` from array `x`.
    
    - **`array_sort`**(*x*) → array
    
        Sorts and returns the array `x`. The elements of `x` must be orderable. Null elements will be placed at the end of the returned array.
    
    - **`cardinality`**(*x*) → bigint
    
        Returns the cardinality (size) of the array `x`.
    
    - **`contains`**(*x*, *element*) → boolean
    
        Returns true if the array `x` contains the `element`.

## Map

1) Subscript Operator: `[]`

    Retrieves value from given key:
    
        SELECT name_to_age_map['Bob'] AS bob_age;

2) Functions

    - **`cardinality`**(*x*) → bigint
    
        Returns the cardinality (size) of the map `x`.
    
    - **`element_at`**(*map(K*, *V)*, *key*) → V
    
        Returns value for given `key`, or `NULL` if the key is not contained in the map.
    
    - **`map`**(*array(K)*, *array(V)) -> map(K*, *V*)
    
        Returns a map created using the given key/value arrays.
    
            SELECT map(ARRAY[1,3], ARRAY[2,4]); -- {1 -> 2, 3 -> 4}
    
    - **`map_keys`**(*x(K*, *V)) -> array(K*)
    
        Returns all the keys in the map `x`.
    
    - **`map_values`**(*x(K*, *V)) -> array(V*)
    
        Returns all the values in the map `x`.

## Regular Expression

- **regexp_extract_all**(*string*,*pattern*) -> `array(varchar)`

    Returns the substrings matched by regex `pattern` in `string`

    ```sql
    SELECT regexp_extract_all('1a 2b 14m', '\d+'); -- [1, 2, 14]
    ```


- **regexp_extract_all**(*string*,*pattern*, *group*) -> `array(varchar)`

    Finds all occurrences of the regex `pattern` in `string` and returns `group`

    ```sql
    SELECT regexp_extract_all('1a 2b 14m', '(\d+)([a-z]+)', 2); -- ['a', 'b', 'm']
    ```


- **regexp_extract**(*string*, *pattern*) -> `varchar`

    Returns the first substring matched by the regex `pattern` in `string`

    ```sql
    SELECT regexp_extract('1a 2b 14m', '\d+'); -- 1
    ```


- **regexp_extract**(*string*, *pattern*, *group*) -> `varchar`

    Finds the first occurrence of the regex `pattern` in `string` and returns the `group`

    ```sql
    SELECT regexp_extract('1a 2b 14m', '(\d+)([a-z]+)', 2); -- 'a'
    ```


- **regexp_like**(*string*, *pattern*) -> `boolean`

    Evaluates the regex `pattern` and determines if it is contained within `string`.

    This is similar to the `LIKE` operator but performs a `contains` operation` rather than a `match` operation.

    ```sql
    SELECT regexp_like('1a 2b 14m', '\d+b'); -- true
    ```


- **regexp_replace**(*string*, *pattern*) -> `varchar`

    Removes every instance of the substring matched by the regex `pattern` from `string`:

    ```sql
    SELECT regexp_replace('1a 2b 14m', '\d+[ab] '); -- '14m'
    ```


- **regexp_replace**(*string*, *pattern*, *replacement*) -> `varchar`

    Removes every instance of the substring matched by the regex `pattern` from `string` with `replacement`

    ```sql
    SELECT regexp_replace('1a 2b 14m', '(\d+)([ab]) ', '3c$2 '); -- '3ca 3cb 14m'
    ```


- **regexp_replace**(*string*, *pattern*, *function*) -> `varchar`

    Removes every instance of the substring matched by the regex `pattern` from `string` using `function`

    ```sql
    SELECT regexp_replace('new york', '(\w)(\w*)', x -> upper(x[1]) || lower(x[2])); --'New York'
    ```


- **regexp_split**(*string*, *pattern*) -> `array(varchar)`

    Splits `string` using the regex `pattern` and returns an array. Trailing empty strings are preserved.

    ```sql
    SELECT regexp_split('1a 2b 14m', '\s*[a-z]+\s*'); -- [1, 2, 14]
    ```




Back to [Tables of Content](../README.md#getting-started-guide-to-host)