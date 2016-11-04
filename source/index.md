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
   "question2": "Give us further ideas",
   "start_date" : "2015-11-22",
   "end_date" : "2015-12-22",
   "email_topic" : "Please rate our sales person N.N",
   "email_text" : "<p>Hello</p> We would appreciate your feedback about our performance. </br> Please answer to survey linked in this email.</br> BR, Company X Sales lead",
   "internal_ref" : "salesperson.name",
   "surveytype" : "rating",
   "sendreminder": "true",
   "picturepath": "http://path.to.picture/image.jpg",
   "locale": "en",   
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

> The above command returns HTTP status 200 and JSON structured like this:

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
"question2" : "Give us further ideas",
"email_topic":"Please rate our sales person",
"email_text":"\u003cp\u003eHello\u003c/p\u003e We would appreciate your feedback about our performance. \u003c/br\u003e Please answer to survey linked in this email.\u003c/br\u003e BR, Company X Sales lead",
"internal_ref":"salesperson.name",
"surveytype":"rating",
"sendreminder": "true",
"picturepath": "http://path.to.picture/image.jpg",
"locale": "en"  
}
}
```

> or in error proper HTTP status code and JSON:

```json
{
  "message": "some error message"
}
```

### HTTP Request

`POST https://letmeknow.fi/api/v1/create_survey`

### Query Parameters

Parameter | Type, M/O/C | Description
--------- | ------- | -----------
identification | String, optional | Unique identification of survey. Can be left blank, and will then be generated.
subject | String, mandatory | Summary of survey. Shown to respondee.
question | String, mandatory | Detaild question of survey. Shown to respondee.
start_date | Date, YYYY-MM-DD, mandatory | Survey’s activation date.
end_date | Date, YYYY-MM-DD, mandatory | Survey’s closing date, inclusive.
email_topic | String, optional | Topic in email that is sent to respondee.
email_text | String, optional | Text in email that is sent to respondee. Can include html tags for line breaks and paragraphs etc.
internal_ref | String, optional | Reference for sender’s system or later querying of results.
surveytype | rating, null; optional | Type of survey. If rating, respondee has to rate on scale 1-5 and can give free format comments. If null, respondee has only free format comments.
respondee_attributes.id | String, optional | Id of respondee, starting from 0. N respondees can be given.
respondee_attributes.id.name | String, optional | Full name of respondee
respondee_attributes.id.email | String, optional | Email of respondee
respondee_attributes.id.destroy | String, mandatory | Default attribute, set to “false” when using API. Can be later used for updating.

## Send emails

This endpoint sends emails to the respondees of survey identified by ‘survey_id’

<aside class="warning">Emails are sent to all respondees when this request is processed.</aside>


```shell
curl "https://letmeknow.fi/api/v1/send_emails"
  -H "Authorization: meowmeowmeow"
  -H "X-Email: demo@demo.com"
  -H "Content-Type: application/json"
  -X POST -d '
    {
        "survey_id": "hHm9Zw"
    }
}' 
```

> The above command returns HTTP status 200 and JSON structured like this:
```json
{
  "success": "true"
}
```

> or in error proper HTTP status code and JSON:
```json
{
  "success": "false",
  "message": "some error message"
}
```

### HTTP Request

`POST https://letmeknow.fi/api/v1/send_emails`

### POST Parameters

Parameter | Type, M/O/C | Description
--------- | ----------- | ----------- 
survey_id | String, mandatory | The ID of the survey


## Return survey ratings

This endpoint returns survey results for the given time period and given internal reference. Also the count of surveys during the given period is returned.


```shell
curl "https://letmeknow.fi/api/v1/return_survey_rating"
  -H "Authorization: meowmeowmeow"
  -H "X-Email: demo@demo.com"
  -H "Content-Type: application/json"
  -X POST -d '
    {
        "internal_ref": "salesperson.name",
        "start_date":"2015-01-25",
        "end_date":"2015-12-31",
    }' 
```

> The above command returns HTTP status 200 and JSON structured like this:
```json
{
    "internal_ref":"matti.myyja",
    "start_date":"2015-01-01",
    "end_date":"2015-12-31",
    "survey_count":19,
    "results":[4.0,2.0,1.0,3.0,3.0,2.0,3.0]
}
```

> or in error proper HTTP status code and JSON:
```json
{
  "message": "some error message"
}
```

### HTTP Request

`POST https://letmeknow.fi/api/v1/send_emails`

### POST Parameters

Parameter | Type, M/O/C | Description
--------- | ----------- | ----------- 
internal_ref | String, mandatory | The internal reference of survey, from source systems
start_date | Date, YYYY-MM-DD, mandatory | Result period start date
end_date | Date, YYYY-MM-DD, mandatory | Result period end date



