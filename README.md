# mongo-views

This is a MongoDB skunkworks project to enable queryable views within the shell. Views are like **virtual collections**, that can be queried as regular collections. They are comprised of queries themselves, and support db joins.

Why might you want this? Well lets say you want to save a query for regular reuse. Say you want all managers from an `employee` collection. Then you could create a view via:

```javascript
db.employees.createView('managers', { manager: true });
```

and query/sort/limit it as though it was a collection via

```
db._managers.find({ name: /Jane/ }).sort({ name: 1 }).pretty();
```

you can then create nested views via

```javascript
db._managers.createView('female_managers', { gender: 'F' });
```

Whenever you open the shell and go into that database, your views will be there for that database. They are virtual, and only save the state of the query used to create them. This means that each time a query is performed on a view, the latest collection data is fetched.

> underscore is required in order to allow immediate View lookup. Workaround involves modifying shell code to be view aware.

Installation
====

Symlink `index.js` to `~/.mongorc.js` or `load()` it within `.mongorc.js`

Basic Usage
=======

__Create__
```javascript
db.[collection].createView(view:String, query:Object)

//or

db._[view].createView(view:String, query:Object)
```

__See all views in DB__
```javascript
show views
```

__Query__
```javascript
db._[view].find(query:Object):DBQuery
```

__Drop__
```javascript
db._[view].drop()
```

Supports
=======

Saved queries (selects)
-------------

* Querying of View (query parameter only)

* Nested views (create a view from view)

* Persistence across Sessions<br />
Views are loaded when shell is started


Todo
----
1. Select
   * support fields in base `View`

1. Persistence
    * support for switching dbs

1. Inner Joins

1. Nice to Have
    * Create views from views

