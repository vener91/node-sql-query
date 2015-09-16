# NodeJS SQL query builder

[![Build Status](https://secure.travis-ci.org/dresende/node-sql-query.png?branch=master)](http://travis-ci.org/dresende/node-sql-query)

## Install

```sh
npm install sql-query --save
```

## Dialects

- MySQL
- PostgreSQL
- SQLite
- MSSQL

## About

This module is used by [ORM](http://dresende.github.com/node-orm2) to build SQL queries in the different supported dialects.
Sorry the API documentation is not complete. There are tests in ./test/integration that you can read.


#Usage

	var sqlQuery = require('sql-query');

##Create

	sqlQuery
		.Create()
		.table('table1')
		.build()
	"CREATE TABLE 'table1'()"

	sqlQuery
		.Create()
		.table('table1')
		.field('id','id')
		.build()
	"CREATE TABLE 'table1'('id' INTEGER PRIMARY KEY AUTO_INCREMENT)"

    sqlQuery
    	.Create()
    	.table('table1')
    	.fields({id: 'id', a_text: 'text'})
    	.build()
    "CREATE TABLE 'table1'('id' INTEGER PRIMARY KEY AUTO_INCREMENT,'a_text' TEXT)"

    sqlQuery
    	.Create()
    	.table('table1')
    	.fields({id: 'id', a_num: 'int'})
    	.build()
    "CREATE TABLE 'table1'('id' INTEGER PRIMARY KEY AUTO_INCREMENT,'a_num' INTEGER)"

    sqlQuery
    	.Create()
    	.table('table1')
    	.fields({id: 'id', a_num: 'float'})
    	.build()
    "CREATE TABLE 'table1'('id' INTEGER PRIMARY KEY AUTO_INCREMENT,'a_num' FLOAT(12,2))"

    sqlQuery
    	.Create()
    	.table('table1')
    	.fields({id: 'id', a_bool: 'bool'})
    	.build()
    "CREATE TABLE 'table1'('id' INTEGER PRIMARY KEY AUTO_INCREMENT,'a_bool' TINYINT(1))"

##Select

	sqlQuery
		.Select()
		.from('table1')
		.build();
	"SELECT * FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.select('id', 'name')
		.build();
	"SELECT `id`, `name` FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.select('id', 'name')
		.as('label')
		.build();
	"SELECT `id`, `name` AS `label` FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.select('id', 'name')
		.select('title')
		.as('label')
		.build();
	"SELECT `id`, `name`, `title` AS `label` FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.select('id', 'name')
		.as('label')
		.select('title')
		.build();
	"SELECT `id`, `name` AS `label`, `title` FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.select([ 'id', 'name' ])
		.build();
	"SELECT `id`, `name` FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.select()
		.build();
	"SELECT * FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.select(['abc','def', { a: 'ghi', sql: 'SOMEFUNC(ghi)' }])
		.build();
	"SELECT `abc`, `def`, (SOMEFUNC(ghi)) AS `ghi` FROM `table1`"

	sqlQuery
		.Select()
		.calculateFoundRows()
		.from('table1')
		.build();
	"SELECT SQL_CALC_FOUND_ROWS * FROM `table1`"

	sqlQuery
		.Select()
		.calculateFoundRows()
		.from('table1')
		.select('id')
		.build();
	"SELECT SQL_CALC_FOUND_ROWS `id` FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.select('id1', 'name')
		.from('table2', 'id2', 'id1')
		.select('id2')
		.build();
	"SELECT `t1`.`id1`, `t1`.`name`, `t2`.`id2` FROM `table1` `t1` JOIN `table2` `t2` ON `t2`.`id2` = `t1`.`id1`"

	sqlQuery
		.Select()
		.from('table1')
		.select('id1')
		.from('table2', 'id2', 'id1', { joinType: 'left inner' })
		.select('id2')
		.build();
	"SELECT `t1`.`id1`, `t2`.`id2` FROM `table1` `t1` LEFT INNER JOIN `table2` `t2` ON `t2`.`id2` = `t1`.`id1`"

	sqlQuery
		.Select()
		.from('table1')
		.select('id1', 'name')
		.from('table2', 'id2', 'table1', 'id1')
		.select('id2')
		.build();
	"SELECT `t1`.`id1`, `t1`.`name`, `t2`.`id2` FROM `table1` `t1` JOIN `table2` `t2` ON `t2`.`id2` = `t1`.`id1`"

	sqlQuery
		.Select()
		.from('table1')
		.from('table2', 'id2', 'table1', 'id1')
		.count()
		.build();
	"SELECT COUNT(*) FROM `table1` `t1` JOIN `table2` `t2` ON `t2`.`id2` = `t1`.`id1`"

	sqlQuery
		.Select()
		.from('table1')
		.from('table2', 'id2', 'table1', 'id1')
		.count(null, 'c')
		.build();
	"SELECT COUNT(*) AS `c` FROM `table1` `t1` JOIN `table2` `t2` ON `t2`.`id2` = `t1`.`id1`"

	sqlQuery
		.Select()
		.from('table1')
		.from('table2', 'id2', 'table1', 'id1')
		.count('id')
		.build();
	"SELECT COUNT(`t2`.`id`) FROM `table1` `t1` JOIN `table2` `t2` ON `t2`.`id2` = `t1`.`id1`"

	sqlQuery
		.Select()
		.from('table1')
		.count('id')
		.from('table2', 'id2', 'table1', 'id1')
		.count('id')
		.build();
	"SELECT COUNT(`t1`.`id`), COUNT(`t2`.`id`) FROM `table1` `t1` JOIN `table2` `t2` ON `t2`.`id2` = `t1`.`id1`"

	sqlQuery
		.Select()
		.from('table1')
		.from('table2', 'id2', 'table1', 'id1')
		.count('id')
		.count('col')
		.build();
	"SELECT COUNT(`t2`.`id`), COUNT(`t2`.`col`) FROM `table1` `t1` JOIN `table2` `t2` ON `t2`.`id2` = `t1`.`id1`"

	sqlQuery
		.Select()
		.from('table1')
		.from('table2', 'id2', 'table1', 'id1')
		.fun('AVG', 'col')
		.build();
	"SELECT AVG(`t2`.`col`) FROM `table1` `t1` JOIN `table2` `t2` ON `t2`.`id2` = `t1`.`id1`"

	sqlQuery
		.Select()
		.from('table1')
		.from('table2',['id2a', 'id2b'], 'table1', ['id1a', 'id1b'])
		.count('id')
		.build();
	"SELECT COUNT(`t2`.`id`) FROM `table1` `t1` JOIN `table2` `t2` ON `t2`.`id2a` = `t1`.`id1a` AND `t2`.`id2b` = `t1`.`id1b`"

##Where

	sqlQuery
		.Select()
		.from('table1')
		.where()
		.build();
	"SELECT * FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.where(null)
		.build();
	"SELECT * FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: 1 })
		.build();
	"SELECT * FROM `table1` WHERE `col` = 1"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: 0 })
		.build();
	"SELECT * FROM `table1` WHERE `col` = 0"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: null })
		.build();
	"SELECT * FROM `table1` WHERE `col` IS NULL"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.eq(null) })
		.build();
	"SELECT * FROM `table1` WHERE `col` IS NULL"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.ne(null) })
		.build();
	"SELECT * FROM `table1` WHERE `col` IS NOT NULL"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: undefined })
		.build();
	"SELECT * FROM `table1` WHERE `col` IS NULL"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: false })
		.build();
	"SELECT * FROM `table1` WHERE `col` = false"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: "" })
		.build();
	"SELECT * FROM `table1` WHERE `col` = ''"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: true })
		.build();
	"SELECT * FROM `table1` WHERE `col` = true"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: 'a' })
		.build();
	"SELECT * FROM `table1` WHERE `col` = 'a'"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: 'a\'' })
		.build();
	"SELECT * FROM `table1` WHERE `col` = 'a\\''"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: [ 1, 2, 3 ] })
		.build();
	"SELECT * FROM `table1` WHERE `col` IN (1, 2, 3)"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: [] })
		.build();
	"SELECT * FROM `table1` WHERE FALSE"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col1: 1, col2: 2 })
		.build();
	"SELECT * FROM `table1` WHERE `col1` = 1 AND `col2` = 2"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col1: 1 }, { col2: 2 })
		.build();
	"SELECT * FROM `table1` WHERE (`col1` = 1) AND (`col2` = 2)"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: 1 }).where({ col: 2 })
		.build();
	"SELECT * FROM `table1` WHERE (`col` = 1) AND (`col` = 2)"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col1: 1, col2: 2 }).where({ col3: 3 })
		.build();
	"SELECT * FROM `table1` WHERE (`col1` = 1 AND `col2` = 2) AND (`col3` = 3)"

	sqlQuery
		.Select()
		.from('table1')
		.from('table2', 'id', 'id')
		.where('table1', { col: 1 }, 'table2', { col: 2 })
		.build();
	"SELECT * FROM `table1` `t1` JOIN `table2` `t2` ON `t2`.`id` = `t1`.`id` WHERE (`t1`.`col` = 1) AND (`t2`.`col` = 2)"

	sqlQuery
		.Select()
		.from('table1')
		.from('table2', 'id', 'id')
		.where('table1', { col: 1 }, { col: 2 })
		.build();
	"SELECT * FROM `table1` `t1` JOIN `table2` `t2` ON `t2`.`id` = `t1`.`id` WHERE (`t1`.`col` = 1) AND (`col` = 2)"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.gt(1) })
		.build();
	"SELECT * FROM `table1` WHERE `col` > 1"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.gte(1) })
		.build();
	"SELECT * FROM `table1` WHERE `col` >= 1"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.lt(1) })
		.build();
	"SELECT * FROM `table1` WHERE `col` < 1"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.lte(1) })
		.build();
	"SELECT * FROM `table1` WHERE `col` <= 1"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.eq(1) })
		.build();
	"SELECT * FROM `table1` WHERE `col` = 1"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.ne(1) })
		.build();
	"SELECT * FROM `table1` WHERE `col` <> 1"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.between('a', 'b') })
		.build();
	"SELECT * FROM `table1` WHERE `col` BETWEEN 'a' AND 'b'"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.not_between('a', 'b') })
		.build();
	"SELECT * FROM `table1` WHERE `col` NOT BETWEEN 'a' AND 'b'"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.like('abc') })
		.build();
	"SELECT * FROM `table1` WHERE `col` LIKE 'abc'"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.not_like('abc') })
		.build();
	"SELECT * FROM `table1` WHERE `col` NOT LIKE 'abc'"

	sqlQuery
		.Select()
		.from('table1')
		.where({ col: sqlQuery.Query.not_in([ 1, 2, 3 ]) })
		.build();
	"SELECT * FROM `table1` WHERE `col` NOT IN (1, 2, 3)"

	sqlQuery
		.Select()
		.from('table1')
		.where({ __sql: [["LOWER(`stuff`) LIKE 'peaches'"]] })
		.build();
	"SELECT * FROM `table1` WHERE LOWER(`stuff`) LIKE 'peaches'"

	sqlQuery
		.Select()
		.from('table1')
		.where({ __sql: [["LOWER(`stuff`) LIKE ?", ['peaches']]] })
		.build();
	"SELECT * FROM `table1` WHERE LOWER(`stuff`) LIKE 'peaches'"

	sqlQuery
		.Select()
		.from('table1')
		.where({ __sql: [["LOWER(`stuff`) LIKE ? AND `number` > ?", ['peaches', 12]]] })
		.build();
	"SELECT * FROM `table1` WHERE LOWER(`stuff`) LIKE 'peaches' AND `number` > 12"

	sqlQuery
		.Select()
		.from('table1')
		.where({ __sql: [["LOWER(`stuff`) LIKE ? AND `number` == ?", ['peaches']]] })
		.build();
	"SELECT * FROM `table1` WHERE LOWER(`stuff`) LIKE 'peaches' AND `number` == NULL"

##Order

	sqlQuery
		.Select()
		.from('table1')
		.order('col')
		.build();
	"SELECT * FROM `table1` ORDER BY `col` ASC"

	sqlQuery
		.Select()
		.from('table1')
		.order('col', 'A')
		.build();
	"SELECT * FROM `table1` ORDER BY `col` ASC"

	sqlQuery
		.Select()
		.from('table1')
		.order('col', 'Z')
		.build();
	"SELECT * FROM `table1` ORDER BY `col` DESC"

	sqlQuery
		.Select()
		.from('table1')
		.order('col').order('col2', 'Z')
		.build();
	"SELECT * FROM `table1` ORDER BY `col` ASC, `col2` DESC"

	sqlQuery
		.Select()
		.from('table1')
		.order('col', [])
		.build();
	"SELECT * FROM `table1` ORDER BY col"

	sqlQuery
		.Select()
		.from('table1')
		.order('?? DESC', ['col'])
		.build();
	"SELECT * FROM `table1` ORDER BY `col` DESC"

	sqlQuery
		.Select()
		.from('table1')
		.order('ST_Distance(??, ST_GeomFromText(?,4326))', ['geopoint', 'POINT(-68.3394 27.5578)'])
		.build();
	"SELECT * FROM `table1` ORDER BY ST_Distance(`geopoint`, ST_GeomFromText('POINT(-68.3394 27.5578)',4326))"

##Limit

	sqlQuery
		.Select()
		.from('table1')
		.limit(123)
		.build();
	"SELECT * FROM `table1` LIMIT 123"

	sqlQuery
		.Select()
		.from('table1')
		.limit('123456789')
		.build();
	"SELECT * FROM `table1` LIMIT 123456789"

##Select Function

	sqlQuery
		.Select()
		.from('table1')
		.fun('myfun', 'col1')
		.build();
	"SELECT MYFUN(`col1`) FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.fun('myfun', [ 'col1', 'col2'])
		.build();
	"SELECT MYFUN(`col1`, `col2`) FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.fun('dbo.fnBalance', [ 80, null, null], 'balance')
		.build();
	"SELECT DBO.FNBALANCE(80, NULL, NULL) AS `balance` FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.fun('myfun', [ 'col1', 'col2'], 'alias')
		.build();
	"SELECT MYFUN(`col1`, `col2`) AS `alias` FROM `table1`"

	sqlQuery
		.Select()
		.from('table1')
		.fun('myfun', [ 'col1', sqlQuery.Text('col2') ], 'alias')
		.build();
	"SELECT MYFUN(`col1`, 'col2') AS `alias` FROM `table1`"

##Insert

	sqlQuery
		.Insert()
		.into('table1')
		.build();
	"INSERT INTO `table1`"
	
	sqlQuery
		.Insert()
		.into('table1')
		.set({})
		.build();
	"INSERT INTO `table1` VALUES()"
	
	sqlQuery
		.Insert()
		.into('table1')
		.set({ col: 1 })
		.build();
	"INSERT INTO `table1` (`col`) VALUES (1)"
	
	sqlQuery
		.Insert()
		.into('table1')
		.set({ col1: 1, col2: 'a' })
		.build();
	"INSERT INTO `table1` (`col1`, `col2`) VALUES (1, 'a')"

##Update

	sqlQuery
		.Update()
		.into('table1')
		.build();
	"UPDATE `table1`"

	sqlQuery
		.Update()
		.into('table1')
		.set({ col: 1 })
		.build();
	"UPDATE `table1` SET `col` = 1"

	sqlQuery
		.Update()
		.into('table1')
		.set({ col1: 1, col2: 2 })
		.build();
	"UPDATE `table1` SET `col1` = 1, `col2` = 2"
	
	sqlQuery
		.Update()
		.into('table1')
		.set({ col1: 1, col2: 2 }).where({ id: 3 })
		.build();
	"UPDATE `table1` SET `col1` = 1, `col2` = 2 WHERE `id` = 3"
