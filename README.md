# Confluence API
This project contains a Node.js module which wraps Atlassian's [Confluence API](https://docs.atlassian.com/atlassian-confluence/REST/latest/).

## Getting Started
Install confluence-api via npm:
```
$ npm install @illia.kudria/confluence-api
```

Create an instance of Confluence by providing a username and password (or token) and a baseUrl used for all future requests.  Confluence uses [basic http authentication](https://developer.atlassian.com/cloud/confluence/basic-auth-for-rest-apis/). 
REST API V2 is used by default.
For instance:
```javascript
var Confluence = require("confluence-api");
var config = {
    username: "testuser",
    password: "test-user-pw-or-rest-api-token",
    baseUrl:  "https://confluence-api-test.atlassian.net/wiki",
    useApiV1: true, // set to use REST API V1, optional
    version: 4 // Confluence major version, optional
};
var confluence = new Confluence(config);
confluence.getPageById("page-id", function(err, data) {
    // do something interesting with data; for instance,
    // data.body.storage.value contains the stored markup for the first
    console.log(data);
});
```

Confluence currently exposes the following API...

<a name="Confluence"></a>

## Confluence
**Kind**: global class
**this**: <code>{Confluence}</code>

* [Confluence](#Confluence)
* REST API V2
  * [.getSpaces(callback)](#Confluence+getSpaces)
  * [.getSpaceById(id, callback)](#Confluence+getSpaceById)
  * [.createPage(data, callback)](#Confluence+createPage)
  * [.getPageById(id, callback)](#Confluence+getPageById)
  * [.updatePage(data, callback)](#Confluence+updatePage)
  * [.deletePage(id, callback)](#Confluence+deletePage)
  * [.getPageVersions(id, callback)](#Confluence+getPageVersions)
* REST API V1
  * [new Confluence(config)](#new_Confluence_new)
  * [.getSpace(space, callback)](#Confluence+getSpace)
  * [.getSpaceHomePage(space, callback)](#Confluence+getSpaceHomePage)
  * [.getContentById(id, callback)](#Confluence+getContentById)
  * [.getCustomContentById(options, callback)](#Confluence+getCustomContentById)
  * [.getContentByPageTitle(space, title, callback)](#Confluence+getContentByPageTitle)
  * [.postContent(space, title, content, parentId, callback, representation)](#Confluence+postContent)
  * [.putContent(space, id, version, title, content, callback, minorEdit, representation)](#Confluence+putContent)
  * [.deleteContent(id, callback)](#Confluence+deleteContent)
  * [.getAttachments(space, id, callback)](#Confluence+getAttachments)
  * [.createAttachment(space, id, filepath, callback)](#Confluence+createAttachment)
  * [.updateAttachmentData(space, id, attachmentId, filepath, callback)](#Confluence+updateAttachmentData)
  * [.getLabels(id, callback)](#Confluence+getLabels)
  * [.postLabels(id, labels, callback)](#Confluence+postLabels)
  * [.deleteLabel(id, label, callback)](#Confluence+deleteLabel)
  * [.search(query, callback)](#Confluence+search)

<a name="new_Confluence_new"></a>

### new Confluence(config)
Construct Confluence.

| Param           | Type                 | Description                                                                                                                      |
|-----------------|----------------------|----------------------------------------------------------------------------------------------------------------------------------|
| config          | <code>Object</code>  |                                                                                                                                  |
| config.username | <code>string</code>  |                                                                                                                                  |
| config.password | <code>string</code>  | The password or REST API Token for the user ([docs](https://developer.atlassian.com/cloud/confluence/basic-auth-for-rest-apis/)) |
| config.baseUrl  | <code>string</code>  |                                                                                                                                  |
| config.useApiV2 | <code>boolean</code> | Default true, Optional                                                                                                           |
| config.version  | <code>number</code>  | Optional                                                                                                                         |

---
REST API V2 METHODS
---
<a name="Confluence+getSpaces"></a>

### confluence.getSpaces(callback)
Get spaces.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param    | Type                   |
|----------|------------------------|
| callback | <code>function</code>  |

<a name="Confluence+getSpaceById"></a>

### confluence.getSpaceById(id, callback)
Get space by id.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param    | Type                   | Value           | 
|----------|------------------------|-----------------|
| id       | <code>string</code>    | ID of the space |
| callback | <code>function</code>  |                 |

<a name="Confluence+createPage"></a>

### confluence.createPage(data, callback)
Creates a page in the space.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param                    | Type                  | Value                                                                                                  |
|--------------------------|-----------------------|--------------------------------------------------------------------------------------------------------|
| data                     | <code>object</code>   | data of new page                                                                                       |
| data.spaceId             | <code>string</code>   | ID of the space                                                                                        |
| data.status              | <code>string</code>   | the status of the page, published or draft                                                             |
| data.title               | <code>string</code>   | title of the page, required if page status is not draft                                                |
| data.parentId            | <code>string</code>   | the parent content ID of the page                                                                      |
| data.body                | <code>object</code>   |                                                                                                        |
| data.body.representation | <code>string</code>   | type of content representation used for the value field. Valid values: storage, atlas_doc_format, wiki |
| data.body.value          | <code>string</code>   | body of the page, in the format found in the representation field                                      |
| callback                 | <code>function</code> |                                                                                                        |

<a name="Confluence+getPageById"></a>

### confluence.getPageById(id, callback)
Get page by id.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param    | Type                   | Value          | 
|----------|------------------------|----------------|
| id       | <code>string</code>    | ID of the page |
| callback | <code>function</code>  |                |

<a name="Confluence+updatePage"></a>

### confluence.updatePage(data, callback)
Update a page by id.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param                    | Type                  | Value                                                                                                  |
|--------------------------|-----------------------|--------------------------------------------------------------------------------------------------------|
| data                     | <code>object</code>   | page data                                                                                              |
| data.id                  | <code>string</code>   | ID of the page                                                                                         |
| data.status              | <code>string</code>   | the updated status of the page. Valid values: current, draft                                           |
| data.title               | <code>string</code>   | title of the page                                                                                      |
| data.body                | <code>object</code>   |                                                                                                        |
| data.body.representation | <code>string</code>   | type of content representation used for the value field. Valid values: storage, atlas_doc_format, wiki |
| data.body.value          | <code>string</code>   | body of the page, in the format found in the representation field                                      |
| data.version             | <code>object</code>   |                                                                                                        |
| data.version.number      | <code>number</code>   | the new version of the updated page                                                                    |
| data.version.message     | <code>string</code>   | an optional message to be stored with the version                                                      |
| callback                 | <code>function</code> |                                                                                                        |

<a name="Confluence+deletePage"></a>

### confluence.deletePage(id, callback)
Delete a page by id.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param    | Type                   | Value          | 
|----------|------------------------|----------------|
| id       | <code>string</code>    | ID of the page |
| callback | <code>function</code>  |                |

<a name="Confluence+getPageVersions"></a>

### confluence.getPageVersions(id, callback)
Returns the versions of specific page.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param    | Type                   | Value          | 
|----------|------------------------|----------------|
| id       | <code>string</code>    | ID of the page |
| callback | <code>function</code>  |                |

---
REST API V1 METHODS
---
<a name="Confluence+getSpace"></a>

### confluence.getSpace(space, callback)
Get space information.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type |
| --- | --- |
| space | <code>string</code> |
| callback | <code>function</code> |

<a name="Confluence+getSpaceHomePage"></a>

### confluence.getSpaceHomePage(space, callback)
Get space home page.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type |
| --- | --- |
| space | <code>string</code> |
| callback | <code>function</code> |

<a name="Confluence+getContentById"></a>

### confluence.getContentById(id, callback)
Get stored content for a specific space and page title.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type |
| --- | --- |
| id | <code>string</code> |
| callback | <code>function</code> |

<a name="Confluence+getCustomContentById"></a>

### confluence.getCustomContentById(options, callback)
Get stored content for a specific page id with optional custom expanders.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | for the custom content request |
| callback | <code>function</code> |  |

<a name="Confluence+getContentByPageTitle"></a>

### confluence.getContentByPageTitle(space, title, callback)
Get stored content for a specific space and page title.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type |
| --- | --- |
| space | <code>string</code> |
| title | <code>string</code> |
| callback | <code>function</code> |

<a name="Confluence+postContent"></a>

### confluence.postContent(space, title, content, parentId, callback, representation)
Post content to a new page.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type | Description |
| --- | --- | --- |
| space | <code>string</code> |  |
| title | <code>string</code> |  |
| content | <code>string</code> |  |
| parentId | <code>number</code> | A null value will cause the page to be added under the space's home page |
| callback | <code>function</code> |  |
| representation | <code>string</code> | Optional |

<a name="Confluence+putContent"></a>

### confluence.putContent(space, id, version, title, content, callback, minorEdit, representation)
Put/update stored content for a page.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type | Description |
| --- | --- | --- |
| space | <code>string</code> |  |
| id | <code>string</code> |  |
| version | <code>number</code> |  |
| title | <code>string</code> |  |
| content | <code>string</code> |  |
| callback | <code>function</code> |  |
| minorEdit | <code>boolean</code> | Optional |
| representation | <code>string</code> | Optional |

<a name="Confluence+deleteContent"></a>

### confluence.deleteContent(id, callback)
Delete a page.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type |
| --- | --- |
| id | <code>string</code> |
| callback | <code>function</code> |

<a name="Confluence+getAttachments"></a>

### confluence.getAttachments(space, id, callback)
Get attachments

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type |
| --- | --- |
| space | <code>string</code> |
| id | <code>string</code> |
| callback | <code>function</code> |

<a name="Confluence+createAttachment"></a>

### confluence.createAttachment(space, id, filepath, callback)
This allows you to post attachments to the pages you create.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type | Description |
| --- | --- | --- |
| space | <code>string</code> |  |
| id | <code>string</code> |  |
| filepath | <code>string</code> | absolute path of the file you are sending |
| callback | <code>function</code> |  |

<a name="Confluence+updateAttachmentData"></a>

### confluence.updateAttachmentData(space, id, attachmentId, filepath, callback)
This allows you to update posted attachments data

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type |
| --- | --- |
| space | <code>string</code> |
| id | <code>string</code> |
| attachmentId | <code>string</code> |
| filepath | <code>string</code> |
| callback | <code>function</code> |

<a name="Confluence+getLabels"></a>

### confluence.getLabels(id, callback)
Get labels from content

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type |
| --- | --- |
| id | <code>string</code> |
| callback | <code>function</code> |

<a name="Confluence+postLabels"></a>

### confluence.postLabels(id, labels, callback)
Post content labels to a existing page.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type |
| --- | --- |
| id | <code>string</code> |
| labels | <code>Array.&lt;{prefix:string, name:string}&gt;</code> |
| callback | <code>function</code> |

<a name="Confluence+deleteLabel"></a>

### confluence.deleteLabel(id, label, callback)
Delete a label from a page.

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type |
| --- | --- |
| id | <code>string</code> |
| label | <code>string</code> |
| callback | <code>function</code> |

<a name="Confluence+search"></a>

### confluence.search(query, callback)
Search by query

**Kind**: instance method of [<code>Confluence</code>](#Confluence)

| Param | Type |
| --- | --- |
| query | <code>string</code> |
| callback | <code>function</code> |

<a name="request"></a>

## request
Node.js wrapper for Atlassian's Confluence API.
See https://developer.atlassian.com/confdev/confluence-rest-api

Copyright (c) 2015, John Duane
Released under the MIT License
