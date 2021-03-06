toy-mongo
=========

Try to implement mongodb internal data structure including index and extents. First of all,
I implement a BTreeMap instead of applying JDK TreeMap. The JDK TreeMap is implemented using
red-black tree, and it will have a large height if we have many, many nodes. Next, I will use
[google's in-memory file system](https://github.com/google/jimfs) to store data. Each document
is saved within an extent which is a logical container of a file. An index point to the position
of a document which resides in an extent.

### B-Tree

`Entry` and `Node` are two important classes inside the map. `Entry` holds key/value pair, and 
we compare each key to put entry objects into `Node` in the right order. If `Node` is full, it's
splitted into two children nodes which are stored in `Node.children` array.

#### CopyEntryIterator

The returned iterator provides a snapshot of the entries of this map when it's created. That is,
copy entries of every node and return. If the map structure is changed while iterating, it'll throw
ConcurrentModificationException.

### API

#### Database

| Method | Path                  | Action             |
| ------ | --------------------- | ------------------ |
| POST   | /databases            | create a database  |
| GET    | /databases            | list the databases |
| GET    | /databases/{database} | get the database   |

#### Collection

| Method | Path                                           | Action              |
| ------ | ---------------------------------------------- | ------------------- |
| POST   | /databases/{database}/collections              | create a collection |
| GET    | /databases/{database}/collections              | list collections in the specified database |
| GET    | /databases/{database}/collections/{collection} | get the collection in the specified database |

#### Document

| Method | Path                                                               | Action              |
| ------ | ------------------------------------------------------------------ | ------------------- |
| POST   | /databases/{database}/collections/{collection}/documents/insertAll | insert multiple documents at a time |
| GET    | /databases/{database}/collections/{collection}/documents           | find documents      |

#### Index

| Method | Path                                                           | Action          |
| ------ | -------------------------------------------------------------- | --------------- |
| POST   | /databases/{database}/collections/{collection}/indexes         | create an index |
| GET    | /databases/{database}/collections/{collection}/indexes         | list indexes    |
| GET    | /databases/{database}/collections/{collection}/indexes/{index} | get the index   |
