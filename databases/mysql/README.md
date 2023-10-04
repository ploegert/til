# mysql

[https://cheatography.com/davechild/cheat-sheets/mysql/](https://cheatography.com/davechild/cheat-sheets/mysql/)

## MySQL Data Types <a href="#title_16_134" id="title_16_134"></a>

| CHAR                                                     | String (0 - 255)                                              |
| -------------------------------------------------------- | ------------------------------------------------------------- |
| VARCHAR                                                  | String (0 - 255)                                              |
| TINYTEXT                                                 | String (0 - 255)                                              |
| TEXT                                                     | String (0 - 65535)                                            |
| BLOB                                                     | String (0 - 65535)                                            |
| MEDIUMTEXT                                               | String (0 - 16777215)                                         |
| MEDIUMBLOB                                               | String (0 - 16777215)                                         |
| LONGTEXT                                                 | String (0 - 429496­7295)                                      |
| LONGBLOB                                                 | String (0 - 429496­7295)                                      |
| TINYINT x                                                | Integer (-128 to 127)                                         |
| SMALLINT x                                               | Integer (-32768 to 32767)                                     |
| MEDIUMINT x                                              | Integer (-8388608 to 8388607)                                 |
| INT x                                                    | Integer (-2147­483648 to 214748­3647)                         |
| BIGINT x                                                 | Integer (-9223­372­036­854­775808 to 922337­203­685­477­5807) |
| FLOAT                                                    | Decimal (precise to 23 digits)                                |
| DOUBLE                                                   | Decimal (24 to 53 digits)                                     |
| DECIMAL                                                  | "­DOU­BLE­" stored as string                                  |
| DATE                                                     | YYYY-MM-DD                                                    |
| DATETIME                                                 | YYYY-MM-DD HH:MM:SS                                           |
| TIMESTAMP                                                | YYYYMM­DDH­HMMSS                                              |
| TIME                                                     | HH:MM:SS                                                      |
| [ENUM](http://dev.mysql.com/doc/refman/5.0/en/enum.html) | One of preset options                                         |
| [SET](http://dev.mysql.com/doc/refman/5.0/en/set.html)   | Selection of preset options                                   |

Integers (marked x) that are "­UNS­IGN­ED" have the same range of values but start from 0 (i.e., an UNSIGNED TINYINT can have any value from 0 to 255).

## MySQL Type Conversion <a href="#title_16_132" id="title_16_132"></a>

| [BINARY 'string'](http://dev.mysql.com/doc/refman/5.0/en/cast-functions.html#operator\_binary)                 |
| -------------------------------------------------------------------------------------------------------------- |
| [CAST (expression AS datatype)](http://dev.mysql.com/doc/refman/5.0/en/cast-functions.html#function\_cast)     |
| [CONVERT (expression, datatype)](http://dev.mysql.com/doc/refman/5.0/en/cast-functions.html#function\_convert) |

## MySQL Grouping Functions <a href="#title_16_128" id="title_16_128"></a>

| AVG            | MAX      |
| -------------- | -------- |
| BIT\_AND       | STD      |
| BIT\_OR        | STDDEV   |
| COUNT          | SUM      |
| GROUP\_­CONCAT | VARIANCE |
| MIN            |          |

## MySQL Mathem­atical Functions <a href="#title_16_126" id="title_16_126"></a>

| ABS              | COS         |
| ---------------- | ----------- |
| SIGN             | SIN         |
| MOD              | TAN         |
| FLOOR            | ACOS        |
| CEILING          | ASIN        |
| ROUND            | ATAN, ATAN2 |
| DIV              | COT         |
| EXP              | RAND        |
| LN               | LEAST       |
| LOG, LOG2, LOG10 | GREATEST    |
| POW              | DEGREES     |
| POWER            | RADIANS     |
| SQRT             | TRUNCATE    |
| PI               |             |

## MySQL String Functions <a href="#title_16_129" id="title_16_129"></a>

| [ASCII](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_ascii)              | [SUBSTRING](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_substring)              |
| -------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| [ORD](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_ord)                  | [MID](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_mid)                          |
| [CONV](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_conv)                | [SUBSTRING\_INDEX](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_substring-index) |
| [BIN](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_bin)                  | [LTRIM](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_ltrim)                      |
| [OCT](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_oct)                  | [RTRIM](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_rtrim)                      |
| [HEX](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_hex)                  | [TRIM](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_trim)                        |
| [CHAR](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_char)                | [SOUNDEX](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_soundex)                  |
| [CONCAT](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_concat)            | [SPACE](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_space)                      |
| [CONCAT\_WS](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_concat-ws)     | [REPLACE](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_replace)                  |
| [LENGTH](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_length)            | [REPEAT](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_repeat)                    |
| [CHAR\_LENGTH](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_char-length) | [REVERSE](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_reverse)                  |
| [BIT\_LENGTH](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_bit-length)   | [INSERT](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_insert)                    |
| [LOCATE](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_locate)            | [ELT](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_elt)                          |
| [INSTR](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_instr)              | [FIELD](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_field)                      |
| [LPAD](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_lpad)                | [LCASE](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_lcase)                      |
| [RPAD](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_rpad)                | [UCASE](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_ucase)                      |
| [LEFT](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_left)                | [LOAD\_FILE](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_load-file)             |
| [RIGHT](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_right)              | [QUOTE](http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function\_quote)                      |





## MySQL Date and Time Functions <a href="#title_16_127" id="title_16_127"></a>

| DAYOFWEEK     | DATE\_SUB         |
| ------------- | ----------------- |
| WEEKDAY       | ADDDATE           |
| DAYOFMONTH    | SUBDATE           |
| DAYOFYEAR     | EXTRACT           |
| MONTH         | TO\_DAYS          |
| DAYNAME       | FROM\_DAYS        |
| MONTHNAME     | DATE\_F­ORMAT     |
| QUARTER       | TIME\_F­ORMAT     |
| WEEK          | CURREN­T\_DATE    |
| YEAR          | CURREN­T\_TIME    |
| YEARWEEK      | NOW               |
| HOUR          | SYSDATE           |
| MINUTE        | UNIX\_T­IME­STAMP |
| SECOND        | FROM\_U­NIXTIME   |
| PERIOD\_ADD   | SEC\_TO­\_TIME    |
| PERIOD­\_DIFF | TIME\_T­O\_SEC    |
| DATE\_ADD     |                   |

## MySQL Control Flow Functions <a href="#title_16_130" id="title_16_130"></a>

| [IF](http://dev.mysql.com/doc/refman/5.0/en/control-flow-functions.html#function\_if)         | [NULLIF](http://dev.mysql.com/doc/refman/5.0/en/control-flow-functions.html#function\_nullif) |
| --------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [IFNULL](http://dev.mysql.com/doc/refman/5.0/en/control-flow-functions.html#function\_ifnull) |                                                                                               |

## MySQL Miscel­laneous Functions <a href="#title_16_131" id="title_16_131"></a>

| BIT\_COUNT     | DES\_EN­CRYPT      |
| -------------- | ------------------ |
| DATABASE       | DES\_DE­CRYPT      |
| USER           | LAST\_I­NSE­RT\_ID |
| SYSTEM­\_USER  | FORMAT             |
| SESSIO­N\_USER | VERSION            |
| CURREN­T\_USER | CONNEC­TION\_ID    |
| PASSWORD       | GET\_LOCK          |
| OLD\_PA­SSWORD | RELEAS­E\_LOCK     |
| ENCRYPT        | IS\_FRE­E\_LOCK    |
| DECODE         | BENCHMARK          |
| MD5            | INET\_NTOA         |
| SHA1           | INET\_ATON         |
| AES\_EN­CRYPT  | FOUND\_ROWS        |
| AES\_DE­CRYPT  | STRCMP             |
