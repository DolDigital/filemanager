# Summary

| Method                                                  | REST   | URL                    | Request                             | Response                              |
|---------------------------------------------------------+--------+------------------------+-------------------------------------+---------------------------------------|
| [Create new file/dir](#create-new-file-or-directory)    | POST   | api/files/:id          | { isDirectory, title, fileContent } | :file-stats-resource                  |
| [Delete file/dir](#delete-file-or-directory)            | DELETE | api/files/:id          | -                                   | -                                     |
| [Get dir children list](#get-directory-children-list)   | GET    | api/files/:id/children | { orderBy, orderDirection, ... }    | { items: [... :file-stats-resource] } |
| [Get file/dir stats](#get-file-or-directory-statistics) | GET    | api/files/:id/stats    | -                                   | :file-stats-resource                  |
| Copy file/dir to destination                            | POST   | api/files/:id/copy/    | { destination: :id }                |                                       |
| Move file to destination                                | POST   | api/files/:id/move/    | { destination: :id }                |                                       |
| Get file(s)/compressed dir                              | GET    | api/download           | [... :id]                           | :binary-data                          |
| [Get client config](#get-client-configuration)          | GET    | api/client-config      | -                                   | :client-config-resource               |

NOTE: file/dir ID is its path+name in base64.

# Files

## File stats resouce

```javascript
{
  id: <string>,
  title: <string>,
  isDirectory: <bool>, // TBD type
  createDate: <string>,
  modifyDate: <string>,
  size: <string>,
  md5Checksum: <string>, // TODO in v2,
  downloadUrl: <string>,  // TBD
}
```

## Create new file or directory

* URL: `api/files/:id`  // TBD: parentId instead of id.
* Method: POST

### Request

```javascript
{
  // TBD title: <string> must be added if parentId is used in URL.
  // TBD parentId: <string> must be added if neither id nor parentId are used in URL.
  isDirectory: <bool>,  // TBD type
  ?content: <binary-data> // Must be specified for files only.
}
```

### Response

```javascript
<file stats resource>
```

## Delete file or directory

* URL: `api/files/id`
* Method: DELETE

### Request

None.

### Response

If successful, this method returns an empty response body.

## Get directory children list

* URL: `api/files/:id/children`
* Method: GET

### Request

```javascript
{
  orderBy: <string>, // one of 'createdDate', 'folder', 'modifiedDate', 'quotaBytesUsed', 'title'.
  orderDirection: <string>, // ASC/DESC
  maxResults: <number>, // TODO in v2
  pageToken: <string>, // TODO in v2
  searchQuery: <string>, // TODO in v2
  searchResursively: <bool> // TODO in v2
}
```

### Response

```javascript
{
  items: [<file stats resource>, ...],  // TBD list of ids.
  nextPageToken // TODO in v2
}
```

## Get file or directory statistics

* URL: `api/files/:id/stats`
* Method: GET

### Request

None.

### Response

```
<file stats resource>
```

# Client config

## Client config resource

```javascript
{
  "layout": {
    "readOnly": false,
  },
  "fileIcons": {
    "wordDocument": {
      "pattern": ["\\.(doc|docx)", "i"],
      "uri": "/img/file-icons/word-file.svg"
    },
    "excelDocument": {
      "pattern": ["\\.(xls|xlsx)", "i"],
      "uri": "/img/file-icons/excel-file.svg"
    },
    "pdfDocument": {
      "pattern": ["\\.(pdf)", "i"],
      "uri": "/img/file-icons/pdf-file.svg"
    },
    "textDocument": {
      "pattern": ["\\.(txt)", "i"],
      "uri": "/img/file-icons/text-file.svg"
    },
    "jsCode": {
      "pattern": ["\\.(js|jsx|mjs)", "i"],
      "uri": "/img/file-icons/js-file.svg"
    },
    "gspCode": {
      "pattern": ["\\.(gsp)", "i"],
      "uri": "/img/file-icons/gsp-file.svg"
    },
    "sound": {
      "pattern": ["\\.(wav|wma|mp3|ogg|flac|aiff)", "i"],
      "uri": "/img/file-icons/sound-file.svg"
    },
    "video": {
      "pattern": ["\\.(webm|mkv|flv|vob|avi|wmv|mpg|mpeg|mpv|m4v)", "i"],
      "uri": "/img/file-icons/video-file.svg"
    },
    "compressed": {
      "pattern": ["\\.(gz|tar|rar|g?zip)$", "i"],
      "uri": "/img/file-icons/compressed.svg"
    },
    "directory": {
      "uri": "/img/file-icons/directory.svg"
    },
    "unknown": {
      "pattern": ["\\.*", "i"],
      "uri": "/img/file-icons/unknown-file.svg"
    }
  }
}
```

## Get client configuration

* URL: `api/client-config`
* METHOD: GET

### Request

None.

### Response

```javascript
<client config resource>
```