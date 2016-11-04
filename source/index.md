---
title: LetMeKnow API Reference

language_tabs:
  - shell


toc_footers:
  - <a href='mailto:tuki@letmeknow.fi'>Request for a Developer API Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the LetMeKnow API! You can use our API to automate your feedback processes and thus get more out of LetMeKnow service.

API contains now the following basic functions:
<ol>
<li>Create survey for a user</li>
<li>Send emails to survey's respondees</li>
<li>Get ratings (1-5) for surveys for a specific period</li>
</ol>

# Authentication

LetMeKnow uses API keys to allow access to the API. You can register a new LetMeKnow API key by contacting the support [LetMeKnow support](mailto:tuki@letmeknow.fi).

LetMeKnow expects for the API key and user's account id (email) to be included in all API requests to the server in a header that looks like the following:

`Authorization: 12345abcde` </br>
`X-Email: demo@demo.com`

<aside class="notice">
You must replace <code>12345abcde</code> with your personal API key and <code>demo@demo.com</code> with user's email address.
</aside>

# Surveys

## Create survey

```shell
curl "https://letmeknow.fi/api/v1/create_survey"
  -H "Authorization: meowmeowmeow"
  -H "X-Email: demo@demo.com"
  -H "Content-Type: application/json"
  -X POST -d '{
 "survey": 
 {
   "subject" : "Sales meeting on 22.11.2015",
   "question" : "Please rate our sales person N.N on your meeting on 22nd November",
   "start_date" : "2015-11-22",
   "end_date" : "2015-12-22",
   "email_topic" : "Please rate our sales person N.N",
   "email_text" : "<p>Hello</p> We would appreciate your feedback about our performance. </br> Please answer to survey linked in this email.</br> BR, Company X Sales lead",
   "internal_ref" : "salesperson.name",
   "surveytype" : "rating",
   "sendreminder": "true",
   "respondees_attributes" :
   {"0": 
    {"name" : "Salesperson Example", 
     "email" : "customer@demo.com",
     "_destroy" : "false"
 	}
   }
 }
}' 
```

> The above command returns JSON structured like this:

```json
{
"identification":"9Y9yRA",
"survey":
{
"id":299,
"subject":"Sales meeting on 22.11.2015",
"description":null,
"start_date":"2015-01-25",
"end_date":"2015-12-31",
"created_at":"2015-12-30T23:40:36.799+02:00",
"updated_at":"2015-12-30T23:40:36.799+02:00",
"identification":"9Y9yRA",
"user_id":17,
"question":"Please rate our sales person",
"email_topic":"Please rate our sales person",
"email_text":"\u003cp\u003eHello\u003c/p\u003e We would appreciate your feedback about our performance. \u003c/br\u003e Please answer to survey linked in this email.\u003c/br\u003e BR, Company X Sales lead",
"internal_ref":"salesperson.name",
"surveytype":"rating",
"sendreminder": "true"
}
}
```

This endpoint creates a new survey for the user.

### HTTP Request

`POST https://letmeknow.fi/api/v1/create_survey`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve
