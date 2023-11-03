# CheatSheets SQL

## Table of Contents
- [SQL online](#sql-online)
- [DDL](#ddl-data-definition-language)
- [DML](#dml-data-manipulation-language)
- [DCL](#dcl-data-control-language)
- [Miscellaneous](#miscellaneous)

## SQL online
* http://rextester.com/l/sql_server_online_compiler

---
---
## DDL: Data Definition Language
DDL or Data Definition Language actually consists of the SQL commands that can be used to define the database schema. It simply deals with descriptions of the database schema and is used to create and modify the structure of database objects in the database

* CREATE
* ALTER
* DROP
* TRUNCATE
* COMMENT
* RENAME

### Modify Schema
```sql
ALTER SCHEMA dbo TRANSFER [myschema\myuser].Table
```

### Alter table ADD / DROP
```sql
ALTER TABLE [dbo].MyTable
ADD new_column int null

ALTER TABLE MyTable 
ADD new_column bit 
DEFAULT 0 NOT NULL;

ALTER TABLE [dbo].MyTable
DROP column old_column 

ALTER TABLE dbo.MyTable
ALTER COLUMN my_column VARCHAR(8000) NOT NULL

ALTER TABLE MyTable
DROP CONSTRAINT fk_MyTable

ALTER TABLE dbo.MyTable
DROP CONSTRAINT [fk_MyTable_column1]

ALTER TABLE MyTable
ADD CONSTRAINT [fk_MyTable_column1]
DEFAULT 2 FOR column1
```

### Check if the object exists
```sql
IF OBJECT_ID('Acl.GetAclObjects', 'IF') IS NOT NULL
DROP FUNCTION Acl.GetAclObjects
GO

IF OBJECT_ID('uf_My_Function', 'FN') IS NOT NULL
DROP FUNCTION uf_My_Function
GO

IF OBJECT_ID('p_ARQ_My_Procedure', 'P') IS NOT NULL
DROP PROC p_ARQ_My_Procedure
GO

IF OBJECT_ID('trgUpdate_MyTable', 'TR') IS NOT NULL
DROP TRIGGER trgUpdate_MyTable
GO

IF OBJECT_ID('MyTable', 'U') IS NOT NULL
DROP TABLE MyTable
GO

IF COL_LENGTH('MyTable', 'chrStreetNumber') IS NULL
BEGIN
     ALTER TABLE dbo.MyTable
     ADD chrStreetNumber CHAR(12) NULL
END
ELSE
BEGIN
     PRINT 'Column "chrStreetNumber" exists in "MyTable"'
END

IF NOT EXISTS (SELECT TOP 1 1 FROM sys.foreign_keys  WHERE object_id = OBJECT_ID(N'dbo.FK_MyTable_ColumnState') AND parent_object_id = OBJECT_ID(N'dbo.MyTable')	)
     ALTER TABLE [dbo].[MyTable]  WITH CHECK ADD CONSTRAINT [FK_MyTable_ColumnState] FOREIGN KEY([ColumnStateId])
     REFERENCES [dbo].[TableState] ([Id])
GO
```

## Create User Defined Types
```sql
CREATE TYPE [dbo].[ArrayOfIntDataType] AS TABLE
(
	[Value] INT NOT NULL
)
```

---
---
## DML: Data Manipulation Language
DML commands are SQL commands that perform operations like storing data in database tables, modifying and deleting existing rows, retrieving data, or updating data.

* SELECT
* INSERT
* UPDATE
* DELETE
* CALL
* EXPLAIN PLAN
* LOCK


### Delete Inner Join
```sql
DELETE w
FROM WorkRecord w
INNER JOIN Employee e
  ON w.EmployeeRun = e.EmployeeNo
Where w.Company = '1' AND e.Birthday = '2013-05-06'
```

### Insert without constrain
```sql
SET IDENTITY_INSERT dbo.MyTable OFF
-- Now you can insert the primary key value
```

### Search index
```sql
select i.name, o.name 
from sys.indexes i
	INNER JOIN sys.objects o ON o.object_id = i.object_id
where o.name like 'AnyText%'
 AND (i.name like 'AnyText%' OR i.name like 'AnyText%')
order by i.name desc
```

## Search Type
```sql
SELECT DISTINCT type, type_desc FROM sys.objects
```

---
---
## DCL: Data Control Language
* Grant
* Revoke

### Grant EXECUTE permission for a Stored Procedure
```sql
GRANT EXECUTE ON Info.Report_ActiveBugs TO TeamLeader;
```

### Revoke SELECT permission from a table
```sql
REVOKE SELECT ON employee FROM user1
```

---
---
## Miscellaneous

### Query version of the instance
```sql
select @@version
```

### SQL Process, session, connection
```sql
sp_who2
select * from sys.dm_exec_sessions
select * from sys.dm_exec_connections
select * from sys.sysprocesses
```

### Sleep
```sql
WAITFOR DELAY '00:00:02'
```

### Temporal Table in a variable
```sql
declare @tmp table (Col1 int, Col2 int);
```

### GUID
```sql
DECLARE @GuidIdUser uniqueidentifier;
SET @GuidIdUser = NEWID();
```

### HASH password
```sql
DECLARE @Pwd nvarchar(20) = 'password1234';
DECLARE @HashedPwd binary(20) = HashBytes('SHA1', @Pwd);
```

### Ident seed
```sql
SELECT IDENT_CURRENT('MyTable')  
```

### Reset seed
```sql
DBCC CHECKIDENT ('MyTable', RESEED, 0);

/* @LastIdent -> Must be used for loading the last identity number*/
DBCC CHECKIDENT ('MyTable', RESEED, @LastIdent);
```

### Insert table variable and execute the Stored Procedure
```sql
DECLARE @IDs dbo.[ArrayOfIntDataType]
INSERT INTO @IDs ([Value]) VALUES (90)
INSERT INTO @IDs ([Value]) VALUES (91)

EXEC dbo.GetObjectsByIDs @IDs
```

### Results concatenate
```sql
DECLARE @csv VARCHAR(1000)

SELECT @csv = COALESCE(CAST(u.Id AS char(6)) + ',' +  @csv , '')
FROM [User] u

SELECT @csv
```

### OpenQuery LinkServer
```sql
UPDATE OPENQUERY (LnkMyBackOfficeAzure, 'SELECT UidCrm, CodeUF, Name FROM [MyBackOffice-DBqa].[dbo].UF')
SET CodeUF = s.crmCodeUF, Name = s.crmName
FROM  	
	( 
	SELECT crmUf.UidCrm as bkoUidCrm,
		ISNULL(convert(varchar(30),crmUf.[MyColumn2]), 'No Available') as crmCodeUF,
		crmUf.[new_name] as crmName
	FROM 
		[dbo].[MyTable1] crmUf
		LEFT JOIN [LnkMyBackOfficeAzure].[MyBackOffice-DBprod].[dbo].MyTable2 bkoE ON crmUf.[MyId] = bkoE.UidCrm		
	WHERE 
		bkoE.CreatedOnCrm IS NULL AND crmUf.UidCrm IS NOT NULL    
	) s 
WHERE UidCrm = s.bkoUidCrm
```

### Search examples
```sql
--------------------------
---------- search table in stored procedures
SELECT DISTINCT Name FROM sys.procedures
WHERE OBJECT_DEFINITION(OBJECT_ID) LIKE '%MyTable1 %'
  
--------------------------
---------- search dependencies

EXEC sp_depends @objname = N'MyTable1' ;

--------------------------
---------- search columns

SELECT  sysobjects.name AS table_name, syscolumns.name AS column_name,
           systypes.name AS datatype, syscolumns.LENGTH AS LENGTH3
FROM sysobjects 
     INNER JOIN syscolumns ON sysobjects.id = syscolumns.id 
     INNER JOIN systypes ON syscolumns.xtype = systypes.xtype
WHERE (sysobjects.xtype = 'U')
     and (UPPER(syscolumns.name) like upper('%myFieldName%'))
ORDER BY sysobjects.name, syscolumns.colid

--------------------------
---------- search all objects

SELECT DISTINCT
    o.name AS Object_Name,o.type_desc
FROM sys.sql_modules        m 
     INNER JOIN sys.objects  o ON m.object_id=o.object_id
WHERE m.definition Like '%AnyText%'
ORDER BY 2,1
```

### Search today records
```sql
SELECT  YEAR(myDate),MONTH(myDate), DAY(myDate),* 
FROM dbo.MyTable1  
WHERE YEAR(myDate) = 2015 AND
	MONTH(myDate) = 7 AND 
	DAY(myDate) = 6
ORDER BY 1 desc
```

### Replace text
```sql
REPLACE ( 'string_expression1' , 'string_expression2' ,'string_expression3' )

' string_expression1 '
	Expresión de cadena en la que se van a realizar búsquedas. 
	El argumento string_expression1 puede ser de un tipo de datos
	que se puede convertir implícitamente en nvarchar o ntext.
' string_expression2 '
	Expresión de cadena que se intenta encontrar. El argumento 
	string_expression2 puede ser de un tipo de datos que se puede 
	convertir implícitamente en nvarchar o ntext.
' string_expression3 '
	Expresión de cadena sustituta. El argumento string_expression3 
	puede ser de un tipo de datos que se puede convertir implícitamente 
	en nvarchar o ntext.
```

### Rename column
```sql
EXEC sp_rename 'dbo.MyTable1.columnOld', 'columnNew', 'COLUMN';  
```

### Insert image at image column
```sql
insert into MyTable (ImageColumn) 
SELECT BulkColumn 
FROM Openrowset( Bulk 'image..Path..here', Single_Blob) as img
```

### Update with select from
```sql
UPDATE MyTable1 SET quantity = s.quantity
FROM 
( SELECT columnSuc, EXTRACT(YEAR FROM columnDate) AS myYear, 
         EXTRACT(MONTH FROM columnDate) AS myMoth, COUNT(*) AS quantity 
  FROM MyTable2
  GROUP BY columnSuc, myYear, myMoth
) s 
WHERE s.myYear = MyTable1.myYear 
  AND s.myMoth = MyTable1.myMoth
  AND s.columnSuc = MyTable1.columnSuc
```

### Coalesce + Concatenate row

Obtiene el primer valor valido (que no sea null), de una lista:
```sql
SELECT COALESCE (NULL,'02/01/2015','03/01/2015') -> '02/01/2015'
```
Genera una sola fila con los valores concatenados por el caracter "coma":
```sql
DECLARE @Names VARCHAR(8000) 

SELECT @Names = COALESCE(@Names + ',', '') + ISNULL(columnFirstName, 'N/A')
FROM dbo.MyTable
SELECT @Names
```

### Insert multiple Data
```sql
INSERT INTO dbo.MyTable (chrCode,varDescription)
SELECT 'CC01','The error is produced by....'
UNION ALL	
SELECT 'CC01','Input number invalid'
UNION ALL
SELECT 'ES01','No Internet Connection'
```

### Get Identity after insert
```sql
SELECT @MyId = SCOPE_IDENTITY()
```

### PIVOT and ROW_NUMBER
```sql
SELECT 
	Id, 
	[1] AS Code1,
	[2] AS Code2,
	[3] AS Code3,
	[4] AS Code4,
	[5] AS Code5
FROM 
	(	
	    SELECT ROW_NUMBER() OVER(PARTITION BY MyIdColumn ORDER BY MyCode ASC) AS RowId,
			MyId AS Id,
			MyCode AS Code
	    FROM dbo.MyTable 
	    WHERE MyId = 100
	) AS SourceTable
	PIVOT
	(
	    max(Code)
		FOR RowId IN ([1],[2],[3],[4],[5])
	) AS PivotTable;
```

### CONVERT string to datetime
```sql
CONVERT(datetime,'2014-05-14 11:24:30.950',120)
```

### CONVERT datetime to dd/mm/yyyy
```sql
CONVERT(VARCHAR(10),table.colum_mydate,103)
```

### GET ALL FOREING KEY
```sql
SELECT
     Origin_Table = FK.TABLE_NAME,
     Constraint_Name = C.CONSTRAINT_NAME,
     FK_Column = CU.COLUMN_NAME,
     Reference_Table = PK.TABLE_NAME,
     Reference_Column = PT.COLUMN_NAME
FROM INFORMATION_SCHEMA.REFERENTIAL_CONSTRAINTS C
     INNER JOIN INFORMATION_SCHEMA.TABLE_CONSTRAINTS FK ON C.CONSTRAINT_NAME = FK.CONSTRAINT_NAME
     INNER JOIN INFORMATION_SCHEMA.TABLE_CONSTRAINTS PK ON C.UNIQUE_CONSTRAINT_NAME = PK.CONSTRAINT_NAME
     INNER JOIN INFORMATION_SCHEMA.KEY_COLUMN_USAGE CU ON C.CONSTRAINT_NAME = CU.CONSTRAINT_NAME
     INNER JOIN (
          SELECT i1.TABLE_NAME, i2.COLUMN_NAME
          FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS i1
               INNER JOIN INFORMATION_SCHEMA.KEY_COLUMN_USAGE i2 ON i1.CONSTRAINT_NAME = i2.CONSTRAINT_NAME
          WHERE i1.CONSTRAINT_TYPE = 'PRIMARY KEY'
     ) PT ON PT.TABLE_NAME = PK.TABLE_NAME
WHERE CU.COLUMN_NAME like '%ANY_TEXT%'
ORDER BY origin_table
```

### LEFT JOIN
LeftJoin in case there is no data in any of the tables brings the record the same but with null in the fields of the tables in which I had no data.

```sql
SELECT
     a.column1,
     b.column3,
     c.column4,
     f.column1
FROM dbo.MyTable1 a
     LEFT JOIN dbo.MyTable2 b ON a.column2 = b.column2      
     LEFT JOIN dbo.MyTable3 c ON a.column4 = c.column3 
     LEFT JOIN dbo.MyTable4 f ON c.column2 = f.column4
WHERE  a.columnId = @Id
```