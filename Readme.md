[![Build Status](https://api.travis-ci.org/chmp/framequery.svg?branch=master)](https://travis-ci.org/chmp/framequery)

# framequery - SQL on dataframes

- [Usage](#usage)
- [SQL support](#sql-support)
- [Changelog](#changelog)
- [Internals](#internal)
- [Running tests](#running-tests)
- [License](#license)

## Usage

Install framequery via:

```bash
pip install framequery
```

Use `framequery.select` to execute queries against dataframes in your scope:

```python
import pandas as pd

stores = pd.read_csv('stores.csv')
sales = pd.read_csv('sales.csv')

import framequery as fq

sales_by_country = fq.execute("""
    SELECT country, sum(sales)

    FROM sales

    JOIN stores
    ON sales.store_id = stores.id

    GROUP BY country
""")
```

By passing the `scope` parameters, the dataframes to use can be specified
explicitly. The example would read
`fq.select(..., scope={'stores': stores, 'sales': sales})`.

Note, that per default framequery uses pandas semantics in groupby and joins.
This will result in behavior diffrent from SQL. To enable strict mode, pass
`strict=True` as an argument to select. Then, additional steps will be performed
to replicate the SQL behavior as close as possible.

**TODO: highlight dask support**

**TODO: highlight sqlalchemy support**

**TODO: highlight table-valued functions and lateral joins (once available)**

## SQL Support

The following operations are supported:

- Select using where, group-by, having, order-by, limit
- Inner and outer joins using equality conditions
- Inner joins with in-equality conditions (currently with high performance
  costs)
- Cross joins (currently, with high performance costs)
- Subqueries
- Common table expressions
- Numeric expressions

The following limitations do exist:

- no support for non-numeric expressions
- no support for outer joins with inequality conditions
- no support for over-clauses
- no support for non select statements (update, insert, ...)
- many, many more, SQL is crazy complex. The topics listed explicitly, however,
  are on the agenda to be fixed.

See the tests, in particular `tests/test_framequery.py`, for examples of
supported queries.

## Changelog

### Development

- refactor code
- aim for postgres compatibility
- first-class dask support
- sqlalchemy support

### 0.1.0

- initial release

## Running tests

Inside the project root, execute

```bash
pip install tox
tox
```

To run conformance tests against a database, set the environment variable 
`FQ_TEST_DB` to sqlalchemy connection string.

## License

>  The MIT License (MIT)
>  Copyright (c) 2016 Christopher Prohm
>
>  Permission is hereby granted, free of charge, to any person obtaining a copy
>  of this software and associated documentation files (the "Software"), to
>  deal in the Software without restriction, including without limitation the
>  rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
>  sell copies of the Software, and to permit persons to whom the Software is
>  furnished to do so, subject to the following conditions:
>
>  The above copyright notice and this permission notice shall be included in
>  all copies or substantial portions of the Software.
>
>  THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
>  IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
>  FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
>  AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
>  LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
>  FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
>  DEALINGS IN THE SOFTWARE.
