# Performance-of-DAX-Data-Analysis-Expressions-in-Power-BI
Improving the performance of DAX (Data Analysis Expressions) in Power BI is essential for faster visuals and scalable dashboards. Here's a comprehensive checklist to help optimize your DAX

1)Use Measures Instead of Calculated Columns : Measures are evaluated at query time, whereas calculated columns are computed during data refresh.
2)Use Variables (VAR) : Reduces engine computation and improves readability.
3)Minimize Use of DISTINCT, VALUES, and FILTER in Large Tables : DISTINCT on a large column can slow down queries.Try to filter at source or with relationships if possible.
4)Use SUMMARIZECOLUMNS instead of SUMMARIZE : SUMMARIZECOLUMNS is optimized for performance and context-aware.
5)Minimize Row Context with Iterators : Avoid using iterators (SUMX, FILTER, COUNTX) over large datasets when you can use aggregation.
6)Use SELECTEDVALUE over VALUES for slicer selections
7)Reduce Cardinality : Split high cardinality columns (e.g., DateTime) into Date and Time.Avoid long text fields unless used in visuals.
8)Calculate Vs Calculate with filter

| Feature                   | `CALCULATE(SUM(...), Column = Value)` | `CALCULATE(SUM(...), FILTER(...))` |
| ------------------------- | ------------------------------------- | ---------------------------------- |
| Filter Evaluation         | Column-based, optimized               | Row-by-row evaluation              |
| Performance               | Fast                                  | Slower on large datasets           |
| Use Case                  | Simple filters                        | Complex filters needing conditions |
| Can use related tables    | Yes                                   | Yes                                |
| Good for High Cardinality | Yes                                   | Not recommended unless unavoidable |

Basic CALCULATE with direct filter

CALCULATE(SUM(Sales[Amount]), Sales[Region] = "South")
Meaning: Adds a filter directly on Sales[Region] = "South" before calculating the sum.
Optimized: This is internally translated into efficient query plans.
Best for performance: When filters are simple and column-based.

CALCULATE with FILTER() function
Meaning: Applies row context filtering using a table expression.
Slower: Because it creates a row-by-row scan (row context → filter context transition).
Use this only when simple filter syntax cannot be used.




