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

## Initialization and Caching

When a user first creates an account with GradeBook, there is an initialization process, which scrapes their StudentVUE account for quarter and class links, and caches them in our database. This may take up to 30 seconds.

This one-time delay is so that we can optimize the future query times of student grades.

## Synergy Cookie Expiry

Synergy cookies have an expiry time that is roughly 1-2 hours after being issued. Unfortunately, this is something GradeBook is unable to circumvent. This means that if you are using the API, you will have to prompt the user to reauthenticate when the user's session has expired.

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

<span class = 'request'>
  <span class = 'request-method'>POST</span><span class = 'request-endpoint'>https://grades.newportml.com/api/v1/authenticate</span>
</span>

### Fields

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

> The Account Resource in JSON:

```json
{
  "username": "s-washingtong",
  "initialized": true,
  "classLinks": {
    "0" : {
      "0" : "PXP_Gradebook.aspx?AGU=0&...",
      "1" : "PXP_Gradebook.aspx?AGU=0&..."
      .
      .
      .
      "6-8" : "PXP_Gradebook.aspx?AGU=0&..."
    }
    .
    .
    .
  },
  "quarterLinks" : [
    "PXP_Gradebook.aspx?AGU=0&...",
    "PXP_Gradebook.aspx?AGU=0&...",
    "PXP_Gradebook.aspx?AGU=0&...",
    "PXP_Gradebook.aspx?AGU=0&..."
  ],
  "profile": "https://wa-bsd405-psv.edupoint.com/Photos/2A...",
  "synergyCookies" : "{\"BellevuePVUECookie\": \"2459457951.1.3283215712.1678802432\", \"ASP.NET_SessionId\": \"rlctltubfwze2da4m3qqxcvj\"}"
}
```

A user contains a variety of information. In particular, each user stores a set of cookies and links to other quarters and classes.

## Logout
```python
import gradebook

gb = gradebook.authenticate('YOUR_API_KEY', 'BSD_USERNAME', 'BSD_PASSWORD')
gb.logout()
```

```javascript
const gradebook = require('gradebook');

gb = gradebook.authenticate('YOUR_API_KEY', 'BSD_USERNAME', 'BSD_PASSWORD')
gb.logout()
```

> The JSON response will be structured like this:

```json
{
  "you are" : "logged out! :)"
}
```

This endpoint logs the user out of the session the application is currently using. To log out of all sessions the user needs to visit [https://grades.newportml.com/profile](https://grades.newportml.com/profile) and click on the `log out` button.

The endpoint being `POST` is just a formality, and you do not need to supply any arguments.

### HTTP Request

<span class = 'request'>
  <span class = 'request-method'>POST</span><span class = 'request-endpoint'>https://grades.newportml.com/api/v1/logout</span>
</span>

<aside class="success">
A 200 response will <b>always</b> mean a successful request
</aside>

# Classes

## Assignment Resource

> The Assignment Resource in JSON:

```json
{
  "name" : "Group Quiz 3",
  "category" : "Test",
  "pointsEarned" : 50,
  "pointsTotal" : 0,
  "comments" : "Extra credit for showing me the other calculations.",
  "date" : "03/19/2002"
}
```

The assignment resource is not directly stored in the database, but is included in the class JSON response when querying the [`/class`](#class) endpoint.

### Fields

<table style = 'width: 50%'>
    <tr style = 'font-weight: 700; width: 100%'>
      <td style = 'text-align: right'>Fields</td><td style = 'width: 100%'>Description</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>name</code><br/><i>string</i>
      </td>
      <td>The name of the assignment.</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>category</code><br/><i>string</i>
      </td>
      <td>The category the assignment belongs to.</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>pointsTotal</code><br/><i>number</i>
      </td>
      <td>The total number of points possible for this assignment.</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>pointsEarned</code><br/><i>number</i>
      </td>
      <td>The number of points earned for this assignment.</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>comments</code><br/><i>string</i>
      </td>
      <td>Comments from the teacher for this assignment.</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>date</code><br/><i>string</i>
      </td>
      <td>The date that this assignment was added to the gradebook. This may not reflect the date the assignment was actually assigned or graded.</td>
    </tr>
</table>

## Category Resource

> The Category Resource in JSON:

```json
"Test" : {
  "percentage" : 50
}
```

The category resource is not directly stored in the database, but is included in the class JSON response when querying the [`/class`](#retreiving-a-specific-class) endpoint.

### Fields

<table style = 'width: 50%'>
    <tr style = 'font-weight: 700; width: 100%'>
      <td style = 'text-align: right'>Fields</td><td style = 'width: 100%'>Description</td>
    </tr>
    <tr style = 'width: 100%'>
      <td style = 'text-align: right;'>
        <code>percentage</code><br/><i>number</i>
      </td>
      <td>The percentage of the total grade that category is worth.</td>
    </tr>
</table>

## Class Resource
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

The class is the most basic component of the GradeBook responses. It contains on surface-level basic metadata, like grade percentage, class name, teacher, etc. 

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
    "grade": 74.2,
    "period": "2",
  },
  {
    "code": "LANG21B",
    "teacher": "Benjamin Franklin",
    "class": "AP Language Arts",
    "grade": 81.4,
    "period": "3",
  },
  .
  .
  .
]
```

This endpoint allows you to see the metadata and grades for all classes a student has. Keep in mind that this will **not** return the individual assignment grades for each class, as visiting each class to see its grades is too expensive.

To see individual assignment grades, see the `class` endpoint [here](#retreiving-a-specific-class)

### HTTP Request

<span class = 'request'>
  <span class = 'request-method'>GET</span><span class = 'request-endpoint'>https://grades.newportml.com/api/v1/classes</span>
</span>

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
  "categories" : {
    "Tests" : {
      "percentage" : 50
    },
    "Homework" : {
      "percentage" : 25
    },
    "In-Class Work" : {
      "percentage" : 25
    }
  }, 
  "assignments" : [
    {
      "name" : "Group Quiz 3",
      "category" : "Test",
      "pointsEarned" : 50,
      "pointsTotal" : 0,
      "comments" : "Extra credit for showing me the other calculations.",
      "date" : "03/19/2002"
    },
    .
    .
    .
  ]
  .
  .
  .
}
```

This endpoint allows you to see assignments and categories of a specific class. The class is identified uniquely by the period number of that class. 

The assignments returned will be in reverse-chronological order, i.e. the most recent assignment will be the first element in the array.

### HTTP Request

<span class = 'request'>
  <span class = 'request-method'>GET</span><span class = 'request-endpoint'>https://grades.newportml.com/api/v1/class/:class_period</span>
</span>

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
