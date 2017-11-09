## ORM Pagination Helper Plugin [![](https://badge.fury.io/js/orm-paging.png)](https://npmjs.org/package/orm-paging)

This plugin adds a pagination helper function for [ORM](http://dresende.github.io/node-orm2).

This is [jnduan's](https://github.com/jnduan/node-orm-paging) fix of [node-orm-paging](https://github.com/dresende/node-orm-paging). I'm putting it up as a package because [dresende](https://github.com/dresende) has been inactive and hadn't merged the pull request.

## Dependencies

Of course you need `orm` to use it. Other than that, no more dependencies.

## Install

```sh
npm install orm-paging2
```

## DBMS Support

Any driver supported by ORM is supported by this plugin.

## Usage

```js
Model.pages([conditions, ]cb)   // total pages
Model.page([conditions, ]page)  // get page
```

## Example

```js
var orm = require("orm");
var paging = require("orm-paging2");

orm.connect("mysql://username:password@host/database", function (err, db) {
	if (err) throw err;

	db.use(paging);

	var Person = db.define("person", {
		name      : String,
		surname   : String,
		age       : Number
	});
	Person.settings.set("pagination.perpage", 10); // default is 20

	Person.pages(function (err, pages) {
		console.log("Total pages: %d", pages);

		Person.page(3).order("name").run(function (err, people) {
			// should get you page 3, which means people from index 20 to 29 (ordered by name)
		});
	});
	
	Person.pages({age: orm.gt(3)}, function(err, pages) {
	    console.log("Total pages: %d", pages);
      
        Person.page({age: orm.gt(3)}, 3).order("name").run(function (err, people) {
            // should get you page 3, which means people who's age is greater than 3 from index 20 to 29 (ordered by name)
        });
	})
});
```

