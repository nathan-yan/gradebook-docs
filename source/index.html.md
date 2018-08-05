---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

This is the offical GradeBook API documentation. 

The documentation contains all endpoints used by Gradebook Web and Gradebook Mobile, and offers information about the structure and behavior of our application.

# API Keys and Authentication Endpoints

To use the GradeBook API, you must have an API key, which you can create [here](https://grades.newportml.com/get). In addition to an API key, GradeBook needs the credentials of a student to actually login into Synergy servers. 

<b>All requests</b> must have the API key in the request header under the name `gb-api-key`, and any requests other than the login request must have a set of cookies stored in the request header under the name `Cookie`. Login information will be sent as POST parameters.

## Authenticate

> To authorize, use this code:

```python
import gradebook

gb = gradebook.authenticate('YOUR_API_KEY', 'BSD_USERNAME', 'BSD_PASSWORD')
```

```javascript
const gradebook = require('gradebook');

gb = gradebook.authenticate('YOUR_API_KEY', 'BSD_USERNAME', 'BSD_PASSWORD')
```

This authenticates the user into the GradeBook API. The response of the API is a set of cookies that should be stored and kept securely in order to authenticate future requests to Synergy servers.

### HTTP Request

`POST https://grades.newportml.com/api/v1/authenticate`

<table style = 'width: 50%'>
    <tr style = 'font-weight: 700; width: 100%'>
      <td style = 'text-align: right'>Fields</td><td style = 'width: 100%'>Description</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>username</code><br/><i>string</i>
      </td>
      <td>The users's BSD username</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>password</code><br/><i>string</i>
      </td>
      <td>The users's BSD password</td>
    </tr>
</table>

<aside class="success">
A 200 response will <b>always</b> mean a successful request
</aside>

# Account

## Account Resource

A user contains a variety of information. In particular, each user stores a set of cookies and links to other quarters and classes.

> The Class Resource in JSON:

```json
{
  "code": "MAT10A",
  "teacher": "George Washington",
  "class": "AP Calc AB",
  "grade": "74.2%",
  "period": 2,
}
```

### Fields

<table style = 'width: 50%'>
    <tr style = 'font-weight: 700; width: 100%'>
      <td style = 'text-align: right'>Fields</td><td style = 'width: 100%'>Description</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>code</code><br/><i>string</i>
      </td>
      <td>The designation/identifier for a class</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>teacher</code><br/><i>string</i>
      </td>
      <td>The teacher for the particular class</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>class</code><br/><i>string</i>
      </td>
      <td>The name of the class</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>period</code><br/><i>string</i>
      </td>
      <td>The period of the class. The period type is a string because some classes span multiple periods, so their period names are strings, for example <code>5-7</code></td>
    </tr>
    <tr
     style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>letter</code><br/><i>character</i>
      </td>
      <td>The current letter grade for the class</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>grade</code><br/><i>number</i>
      </td>
      <td>The current percentage grade for the class</td>
    </tr>
    <tr style = 'width: 100%' class = 'optional'>
      <td style = 'text-align: right;'>
        <code>categories</code><br/><i>array [category]</i>
      </td>
      <td>A list of assignment categories for that class. This field is only included when retreiving a specific class.</td>
    </tr>
    <tr style = 'width: 100%' class = 'optional'>
      <td style = 'text-align: right;'>
        <code>assignments</code><br/><i>array [assignment]</i>
      </td>
      <td>A list of assignments through time for that class. This field is only included when retreiving a specific class.</td>
    </tr>
</table>

# Classes

## Class Resource

The class is the most basic component of the GradeBook responses. It contains on surface-level basic metadata, like grade percentage, class name, teacher, etc. 

> The Class Resource in JSON:

```json
{
  "code": "MAT10A",
  "teacher": "George Washington",
  "class": "AP Calc AB",
  "grade": "74.2%",
  "period": 2,
}
```

### Fields

<table style = 'width: 50%'>
    <tr style = 'font-weight: 700; width: 100%'>
      <td style = 'text-align: right'>Fields</td><td style = 'width: 100%'>Description</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>code</code><br/><i>string</i>
      </td>
      <td>The designation/identifier for a class</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>teacher</code><br/><i>string</i>
      </td>
      <td>The teacher for the particular class</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>class</code><br/><i>string</i>
      </td>
      <td>The name of the class</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>period</code><br/><i>string</i>
      </td>
      <td>The period of the class. The period type is a string because some classes span multiple periods, so their period names are strings, for example <code>5-7</code></td>
    </tr>
    <tr
     style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>letter</code><br/><i>character</i>
      </td>
      <td>The current letter grade for the class</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>grade</code><br/><i>number</i>
      </td>
      <td>The current percentage grade for the class</td>
    </tr>
    <tr style = 'width: 100%' class = 'optional'>
      <td style = 'text-align: right;'>
        <code>categories</code><br/><i>array [category]</i>
      </td>
      <td>A list of assignment categories for that class. This field is only included when retreiving a specific class.</td>
    </tr>
    <tr style = 'width: 100%' class = 'optional'>
      <td style = 'text-align: right;'>
        <code>assignments</code><br/><i>array [assignment]</i>
      </td>
      <td>A list of assignments through time for that class. This field is only included when retreiving a specific class.</td>
    </tr>
</table>


## Retreiving your classes
```python
import gradebook

gb = gradebook.authenticate('YOUR_API_KEY', 'BSD_USERNAME', 'BSD_PASSWORD')
gb.classes.get(
  quarter = 'current'    # Can be 1, 2, 3, 4. If it's anything other than that, retreive the current quarter
)
```

```javascript
const gradebook = require('gradebook');

gb = gradebook.authenticate('YOUR_API_KEY', 'BSD_USERNAME', 'BSD_PASSWORD')
gb.classes.get(
  'current'              // Can be 1, 2, 3, 4. If it's anything other than that, retreive the current quarter
)
```

> The JSON response will be structured like this:

```json
[
  {
    "code": "MAT10A",
    "teacher": "George Washington",
    "class": "AP Calc AB",
    "grade": "74.2%",
    "period": "2",
  },
  {
    "code": "LANG21B",
    "teacher": "Benjamin Franklin",
    "class": "AP Language Arts",
    "grade": "81.4%",
    "period": "3",
  },
  .
  .
  .
]
```

This endpoint allows you to see class metadata and grades. 

### HTTP Request

`GET https://grades.newportml.com/api/v1/classes`

### Query Parameters

<table style = 'width: 50%'>
    <tr style = 'font-weight: 700; width: 100%'>
      <td style = 'text-align: right'>Parameter</td><td style = ''>Possible Values</td><td style = 'width: 100%'>Description</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>quarter</code><br/><i>number</i>
      </td>
      <td><code>1, 2, 3, 4, current</code><br/>Default: <code>current</code></td>
      <td>The quarter from which to retreive your classes. A value of <code>current</code> will retreive classes from the current quarter</td>
    </tr>

</table>

<aside class="success">
A 200 response will <b>always</b> mean a successful request
</aside>

## Retreiving a Specific Class
This endpoint allows you to see assignments and categories of a specific class. The class is identified uniquely by the period number of that class. 

```python
import gradebook

gb = gradebook.authenticate('YOUR_API_KEY', 'BSD_USERNAME', 'BSD_PASSWORD')
gb.classes.get(
  quarter = 'current',    # Can be 1, 2, 3, 4. If it's anything other than that, retreive the current quarter
  period = '5-7'          # Generally you'll get valid period names by calling the /classes endpoint first
)
```

```javascript
const gradebook = require('gradebook');

gb = gradebook.authenticate('YOUR_API_KEY', 'BSD_USERNAME', 'BSD_PASSWORD')
gb.classes.get(
  'current',              // Can be 1, 2, 3, 4. If it's anything other than that, retreive the current quarter
  '5-7'                   // Generally you'll get valid period names by calling the /classes endpoint first
)
```

> The JSON response will be structured like this:

```json
{
  "categories" : [
    {
      "" : ""
    }
  ],
  .
  .
  .
}
```

### HTTP Request

`GET https://grades.newportml.com/api/v1/class/:class_period`

`:class_period` should be replaced by the period of the class.

### Query Parameters

<table style = 'width: 50%'>
    <tr style = 'font-weight: 700; width: 100%'>
      <td style = 'text-align: right'>Parameter</td><td style = ''>Possible Values</td><td style = 'width: 100%'>Description</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>quarter</code><br/><i>number</i>
      </td>
      <td><code>1, 2, 3, 4, current</code><br/>Default: <code>current</code></td>
      <td>The quarter from which to retreive your class. A value of <code>current</code> will retreive class information from the current quarter</td>
    </tr>
</table>

<aside class="success">
A 200 response will <b>always</b> mean a successful request
</aside>

# Messaging

GradeBook also exposes its messaging endpoints for public use. Messaging allows sending images, files and text.

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

