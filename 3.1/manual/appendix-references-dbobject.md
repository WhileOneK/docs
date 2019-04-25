---
layout: default
description: The "db" Object
---
The "db" Object
===============

The `db` object is available in [arangosh](gettingstarted-arangosh.html) by
default, and can also be imported and used in Foxx services.

*db.name* returns a [collection object](appendix-references-collectionobject.html) for the collection *name*.

The following methods exists on the *_db* object:

*Database*

* [db._createDatabase(name, options, users)](datamodeling-databases-workingwith.html#create-database)
* [db._databases()](datamodeling-databases-workingwith.html#list-databases)
* [db._dropDatabase(name, options, users)](datamodeling-databases-workingwith.html#drop-database)
* [db._useDatabase(name)](datamodeling-databases-workingwith.html#use-database)

*Indexes*

* [db._index(index)](indexing-workingwithindexes.html#fetching-an-index-by-handle)
* [db._dropIndex(index)](indexing-workingwithindexes.html#dropping-an-index)

*Properties*

* [db._id()](datamodeling-databases-workingwith.html#id)
* [db._isSystem()](datamodeling-databases-workingwith.html#issystem)
* [db._name()](datamodeling-databases-workingwith.html#name)
* [db._path()](datamodeling-databases-workingwith.html#path)
* [db._version()](datamodeling-documents-documentmethods.html#get-the-version-of-arangodb)

*Collection*

* [db._collection(name)](datamodeling-collections-databasemethods.html#collection)
* [db._create(name)](datamodeling-collections-databasemethods.html#create)
* [db._drop(name)](datamodeling-collections-databasemethods.html#drop)
* [db._truncate(name)](datamodeling-collections-databasemethods.html#truncate)

*AQL*

* [db._createStatement(query)](../aql/invocation-witharangosh.html#with-createstatement-arangostatement)
* [db._query(query)](../aql/invocation-witharangosh.html#with-dbquery)
* [db._explain(query)](releasenotes-newfeatures28.html#miscellaneous-improvements)

*Document*

* [db._document(object)](datamodeling-documents-databasemethods.html#document)
* [db._exists(object)](datamodeling-documents-databasemethods.html#exists)
* [db._remove(selector)](datamodeling-documents-databasemethods.html#remove)
* [db._replace(selector,data)](datamodeling-documents-databasemethods.html#replace)
* [db._update(selector,data)](datamodeling-documents-databasemethods.html#update)