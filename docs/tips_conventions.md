# TiPS Conventions
## What You Must Know
### Columns in Generated DMLs
TiPS automatically identifies common columns between the Source and Target, which are then used in generated DMLs. Therefore, ensure that columns required for generated DMLs in TiPS are present in both the source and target with the same name. Aliases can be used in select statements of views where applicable.

## What You Must Follow
### SCD (Slow Changing Dimension) Type 2
When using the PUBLISH_SCD2_DIM command type for populating SCD Type 2, ensure that the following columns are included in the Dimension Table:

- EFFECTIVE_START_DATE: DataType DATE, can additionally have a NOT NULL constraint.
- EFFECTIVE_END_DATE: DataType DATE, can additionally have a NOT NULL constraint.
- IS_CURRENT_ROW: DataType BOOLEAN, can additionally have a NOT NULL constraint.
- BINARY_CHECK_SUM: DataType NUMBER. This is a virtual column containing the hashed value of columns checked for changed values. <p>For example:</p><p>`BINARY_CHECK_SUM NUMBER (38,0) AS (HASH(CUSTOMER_NAME, CUSTOMER_ADDRESS, CUSTOMER_PHONE, CUSTOMER_MARKET_SEGMENT, CUSTOMER_COMMENT, COUNTRY, REGION))
`  

The View/Table that is the source for the Dimension should only have the EFFECTIVE_START_DATE, populated with the date the new record should be effective from. 

### MERGE - Key/ID Column Populated from Sequence
For a target table needing a Key/ID column populated from a sequence, ensure that the column name ends with `_KEY` or `_SEQ`. Also, ensure that the SEQUENCE name starts with `SEQ_` followed by the target table name and is created in the same schema as the target table.

### Schema Names Prepend to Objects
In the `PROCESS_CMD` metadata table, whenever database objects are referenced, include the schema name. For example, `[SCHEMA NAME].[TABLE NAME]`.

### Upper Case for DB Objects
In the `PROCESS_CMD` metadata table, ensure that object names (including schema names) are entered in UPPERCASE.

### No Whitespaces in Process Name
In the `PROCESS` table, or wherever the process name is referenced, avoid including any whitespaces. Instead, use an underscore to indicate a logical split between words.

## Good to Follow
* Use the `NOT NULL` constraint where possible.
* Consider not allowing NULL values in tables in the presentation layer (accessible by end users). If NULL values are likely from source systems, use a sensible replacement of null values as a standard.
* Adopt standards around casing (uppercase or lowercase or any other preferred) for text columns when data is populated in presentation layer tables (or tables accessed by end users).
