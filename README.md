linked data package registry
============================

A [CouchDB](http://couchdb.apache.org/) powered data registry for
linked data packages.

Inspired by the [npm registry](https://github.com/isaacs/npmjs.org)
but different because:

- build from the start for [linked data](http://en.wikipedia.org/wiki/Linked_data)
- data package are served as [JSON-LD](http://json-ld.org) or [JSON interpreded as JSON-LD](http://json-ld.org/spec/latest/json-ld/#interpreting-json-as-json-ld) and using the semantic of [schema.org](http://schema.org)
- semantic search is supported

A client is in development [here](https://github.com/standard-analytics/ldpm).

Installation
============

This module uses [gm](https://github.com/aheckmann/gm) so first you
need to download and install
[GraphicsMagick](http://www.graphicsmagick.org/) or
[ImageMagick](http://www.imagemagick.org/).


API
===

### GET /:dpkgname

Get a JSON array of all the [versions](http://semver.org/) of the data
package with name ```:dpkgname```.


### GET /:dpkgname/:version

Download a data package of name ```:dpkgname``` and
[version](http://semver.org/) ```:version``` as
[JSON interpreded as JSON-LD](http://json-ld.org/spec/latest/json-ld/#interpreting-json-as-json-ld). If
version is ```latest```, the latest version is returned.

Version range can be specified as an
([encoreURIComponent](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent))
[range](https://github.com/isaacs/node-semver#ranges) passed as query string parameter ```range```.

If a datapackage contains inline data, by default the inline data are
_not_ returned unless the query string parameter ```contentData``` is
set to ```true```.


### GET /:dpkgname/:version/dataset/:dataset

Download _metadata_ from a dataset named ```:dataset``` from a data
package with name ```:dpkgname``` and [version](http://semver.org/)
```:version```. Version can be specifyied as ```latest``` and a qery
string parameter ```range```.


### GET /:dpkgname/:version/dataset/:dataset/:content

Download _data_ (```:content```) from a dataset named ```:dataset```
from a data package with name ```:dpkgname``` and
[version](http://semver.org/) ```:version```. Version can be specifyied
as ```latest``` and a qery string parameter ```range```.


### GET /:dpkgname/:version/code/:code

Download _metadata_ from a code entry named ```:code``` from a data
package with name ```:dpkgname``` and [version](http://semver.org/)
```:version```. Version can be specifyied as ```latest``` and a qery
string parameter ```range```.

### GET /:dpkgname/:version/code/:code/:content

Download a _distribution_ (version of the code ready to be run)
(```:content```) from a code entry named ```:code``` from a data
package with name ```:dpkgname``` and [version](http://semver.org/)
```:version```. Version can be specifyied as ```latest``` and a qery
string parameter ```range```.


### GET /:dpkgname/:version/figure/:figure

Download _metadata_ from a figure named ```:figure``` from a data
package with name ```:dpkgname``` and [version](http://semver.org/)
```:version```. Version can be specifyied as ```latest``` and a qery
string parameter ```range```.


### GET /:dpkgname/:version/figure/:figure/:content

Download _image file_ (```:content```) from a figure named
```:figure``` from a data package with name ```:dpkgname``` and
[version](http://semver.org/) ```:version```. Version can be
specifyied as ```latest``` and a qery string parameter ```range```.



### PUT /adduser/:name

data:

    {
      name: name,
      salt: salt,
      password_sha: sha(password+salt),
      email: email
    }
    
Create an user of username ```:name```.


### PUT /:dpkgname/:version

data: Document with attachments in multipart/related format as needed
by CouchDb. See
[CouchDB multiple attachments](http://docs.couchdb.org/en/latest/api/document/common.html#creating-multiple-attachments)
for details. You might want to look at the
[couch-multipart-stream](https://github.com/standard-analytics/couch-multipart-stream)
node module.

Publish a specific ```:version``` of the data package of name ```:dpkgname```.


### DELETE /:dpkgname/:version?

Delete data package of name ```:dpkgname``` and version
```:version```. If version is omitted all the versions are deleted.


### GET /owner/ls/:dpkgname

List the maintainers of data package of name ```:dpkgname```.


### POST /owner/add

data:

    {
      username: name,
      dpkgName: dpkgname
    }


Add maintainer ```:name``` to the data package ```:dpkgname```.


### POST /owner/rm

data:

    {
      username: name,
      dpkgName: dpkgname
    }

Remove maintainer ```:name``` from the data package ```:dpkgname```.


### GET /search?keys=["search", "terms"]

Search by keywords.



Tests
=====

    couchdb
    npm run push
    npm start
    npm test


License
=======

GPL version 3 or any later version.

