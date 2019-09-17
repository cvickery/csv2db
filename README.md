# csv2db - Create a database table from a CSV file.
## Description

This is a Python script that converts a “properly structured” CSV file to a Postgres database table.

The CSV file has to have a heading row in which each column name turns into a valid db column name when lowercased and with spaces replaced by underscores. Most database column types will be _text_, but certain column names will trigger the use of the _date_ or _integer_ data types.

## Usage Examples
```
csv2db.py -f CSVfile123.csv
```
- Create a table named _csvfile_ in the databse with the same name as the user’s username.
```
csv2db.py -p id region -t regions -db accounts < csvfile.csv
```
- Create a table named _regions_ in the _accounts_ database; the primary key is (id, region)

## Usage Details
- The `--table_name` (`-t`) option gives the name of the table to create. The table will be replaced if it already exists. If the table name is not specified, it will be the stem part of the CSV file name, lowercased and with digits and hyphens stripped. If input is from _stdin_ and no table name is specified, the table name is _test_.
- Column names from the header row are used to set the types of the database columns: names ending in `_date` will be of type _date_, names ending in `_id`, `_num_`, and `_nbr` will be of type _integer_. All other columns will be of type _text_.
    - The command line options `-date`, `-id`, `-num`, and `-nbr` suppress the use of date/integer types for the corresponding column name patterns.
- The `--modulus` (`-m`) _`n`_ option, with a value greater than 0 for _n_, turns on progress reporting: display the number of lines processed every _n_ lines (i.e., when the line count is zero modulo _n_).
- The `--primary_key` (`-p`) option gives a list of column names to use as the primary key for the resulting table. If omitted, the table is created with no primary key.
- The `--columns` (`-c`) option displays the column names and types for the generated table, and exits. Intended as a helper for setting up and checking the primary key settings.
- There is a `--debug` (`-d`) option, but it doesn’t do anything.

## Intended Use and Error Conditions
The program is intended as a development utility for loading spreadsheets into database tables so that standard sql queries can be used to explore the information in the sheet quickly rather than traditional spreadsheet pivot tables and lookup functions. Thus, rather than try to make the code robust in the face of anomalies, database errors are not handled, and the user is presumed equipped to make the necessary adjustments to the command line options or even the CSV file.
