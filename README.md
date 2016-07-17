# sbb_hrdf_format
(mostly) Complete formal definition of SBB's HRDF data format, used with `hrdf_parser`

## General informations
All INI must contain a `[GENERAL]` block, which contains the section `files`. Theses are the sections that will be parsed. They are separted by commas.

## Special fields
There is three special fields available.

### `_line_types`
This is used when files contails different types of lines. Each comma-separated value must match to a section. Use the `_pattern_match` or `_startswith` field to restrict line matching. Only the first match is parsed.

### `_pattern_match`
Only evaluates the line if the regex matches. Note that this is pretty slow on large files.

### `_startswith`
Only evalutes the line if it starts with the given value.

## Types
There is four basic field types. They all contain at least two parameters (even if they're empty). The first one represent the first `column + 1` of the first character to match. If empty, it will start with the first character. The second one represent the `last` character to match. If empty, it will end with the line. 

### `int`
This represents integer, and is interpreted as `INTEGER` in the SQLite database. Internally, they're converted to `int`s.

### `float`
This represents floating-precision numbers, and is interpreted as `FLOAT` in the SQLite database. Internally, they're converted to `float`s.

### `bool`
This represents booleans, and is intepreted as `INTEGER` in the SQLite database. Internally, they're converted to `bool`s. They require an extra parameter, and are set to `True` if and only if the string matches the parameter's value.

### `char`
This represents strings, is interpreted as `VARCHAR` in the SQLite database. Please note that the VARCHAR length corresponds to the column of the last character - the column of the first character + 1, as set in the INI file. For example, if you set `foo=char(12,27)`, you'll get a `VARCHAR(27-12+1)=VARCHAR(16)`.

### `FOO`
This is used to set explicilty that a field matches another file's value. This is not currently used, and is considered as `char` for now. We however use it as in a semantic manner.
