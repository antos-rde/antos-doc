# Virtual Database
The Virtual data-base eases the access to the server side data-base from AntOS application.
AntOS applications works on relational database at table level, it can freely create new table without any restriction.

The server side API MUST verify user permission before grant any access to the back-end relational database.

Table of content:

1. [Base URI](#h2_1)
2. [Save a record to table](#h2_2)
3. [Delete record from table](#h2_3)
4. [Get all records](#h2_4)
5. [Custom records selection](#h2_5)

-----

## 1. Base URI

### Request

```json
GET /VDB HTTP/1.1
Content-Type: application/json
```

### Response

API description:

```json
{
	"error": false,
	"result": {
		"description": "This api handle database operation",
		"actions": {
			"/delete": "Delete record(s) by condition or by id",
			"/select": "Select records by a condition",
			"/save": "Save a record to a table",
			"/get": "Get all records or Get a record by id"
		}
	}
}
```

## 2. Save a record to table
Save a record to a table, if the table does not exists, the server-side API should infer the table structure from the record, then create the table automatically.
There is no API for table creation on the client side.

### Request

```json
POST /VDB/save HTTP/1.1
Content-Type: application/json

{
	"table": TABLE_NAME,
	"data": DATA_RECORD
}
```

For example:

```json
POST /VDB/save HTTP/1.1
Content-Type: application/json

{
	"table": "user",
	"data": {
		"name": "John Doe",
		"age": 30
	}
}
```

### Response

The server side create the table if needed, then insert the data record to the table.
For example, with the previous request example, if the table `user` does not exist, a table with the following structure should be created:

```sql
CREATE TABLE user (
	id INTEGER PRIMARY KEY AUTOINCREMENT,
	name TEXT NOT NULL,
	age INTEGER
);
```

The `id` primary key is mandatory on all tables.

The server should responds with a result object that indicate whether the operation successes, for example:


```json
{
	"error": false,
	"result": true
}
```

##  Delete a record from table

### Request

The request should be in the following formats:

```json
POST /VDB/delete HTTP/1.1
Content-Type: application/json

{
	"table": TABLE_NAME,
	"id": RECORD_PRIMARY_KEY 
}
```

or 

```json
POST /VDB/delete HTTP/1.1
Content-Type: application/json

{
	"table": TABLE_NAME,
	"cond": CONDITIONAL_OBJECT
}
```

The `CONDITIONAL_OBJECT` is the object that represents a SQL condition.
More information on this object can be found at [https://doc.iohub.dev/antos/api/classes/os.api.db.html#delete](https://doc.iohub.dev/antos/api/classes/os.api.db.html#delete)

### Response

The server should responds with a result object that indicate whether the operation successes, for example:


```json
{
	"error": false,
	"result": true
}
```

## Select all records

Get the entire data in a table


### Request

The request should be in the following format:

```json
POST /VDB/get HTTP/1.1
Content-Type: application/json

{
	"table": TABLE_NAME
}
```

### Response

The table data in JSON format, for example:

```json
{
	"error": false,
	"result": [
		{
			"name": "John Doe",
			"age": 36
		},
		...
	]
}
```

## Custom records selection

Select data records using a custom conditional object.

### Request

The request should be in the following formats:

```json
POST /VDB/delete HTTP/1.1
Content-Type: application/json

{
	"table": TABLE_NAME,
	"cond": CONDITIONAL_OBJECT
}
```
The `CONDITIONAL_OBJECT` is the object that represents a SQL condition.
More information on this object can be found at [https://doc.iohub.dev/antos/api/classes/os.api.db.html#find](https://doc.iohub.dev/antos/api/classes/os.api.db.html#find)

### Response

The selected records in JSON format, for example:

```json
{
	"error": false,
	"result": [
		{
			"name": "John Doe",
			"age": 36
		},
		...
	]
}
```