---
title: Thumbtack Partner API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Partner with Us!</a>

includes:
  - errors
  - terms_of_use

search: true
---

# Introduction

The purpose of Thumbtack's Partner API is to enable Partners to seamlessly serve Thumbtack content on Partner
platforms. Currently, the content being served by the API includes information about Thumbtack categories and Pros.
However, our API is under active development, so we are constantly adding more content to the API upon request.  

Your use of our API is subject to Thumbtack's <a href='#api-terms-of-use'>API Terms of Use</a>.

# Partnership Mechanics

1. Partners show Thumbtack content (Pros and/or categories) on their platforms.   
2. Thumbtack provides links for each piece of content. These links contain partner tracking information, which allow
Thumbtack to attribute the Partner's users to the Partner.  
3. By clicking the Thumbtack content, the Partner's users are directed to Thumbtack.   
4. When a Partner's user contacts a Pro on Thumbtack, the associated revenue is shared with the Partner.  

# Spotlight
## Nextdoor

<img src="images/nextdoor.png" alt="Nextdoor Example" title="Nextdoor Example" height="370px"
    style="display:block; margin-left:auto; margin-right:auto;""/>

## News Break

<div id="nb_images" style="display:flex; margin-right:50%">
    <img src="images/newsbreak_categories.jpg" alt="News Break Categories" title="News Break Categories Example" height="370px"
        style="margin-left: auto; margin-right: 10%;"/>
    <img src="images/newsbreak_services.jpg" alt="News Break Services" title="News Break Services Example" height="370px"
        style="margin-right: auto"/>
</div>

# Authentication

> To authorize, use this code:

 ```shell
 # With shell, you can just pass the correct header with each request
 curl "api_endpoint_here"
    -H "Authorization: AUTH_HEADER"
```

> Make sure to replace `AUTH_HEADER` with your personal header.
 
Where applicable, APIs use HTTP basic access authentication. Thumbtack will provide Partners with a username
and password. The basic header looks as follows:

`Authorization: Basic <encoding>`

`<encoding>` is the base64 encoding of the username followed by a colon, followed by the password.

Thumbtack will provide Partners with two sets of credentials - one for Thumbtack's test environment and 
one for Thumbtack's production environment.

# Endpoints

Thumbtack exposes the following endpoints for Partners. 
All endpoints are versioned to support future schema changes. 
Endpoints will use HTTP basic access authentication, and Thumbtack will provide 
Partners with username and password for Partners to call these endpoints.

## Pros

> Sample Request

```shell
curl https://api.thumbtack.com/v1/partners/discoverylite/pros?category=<category>&zip_code=<zip_code>&utm_source=<utm_source>
  -H "Authorization: AUTH_HEADER"
  -H "Content-Type:application/json" 
```

> The above command returns JSON structured like this:

```json
{
   "results":[
      {
         "service_id":"340819324875669527",
         "business_name":"José Daniel Ramirez Vazquez",
         "rating":4.541666666666667,
         "num_reviews":24,
         "years_in_business":5,
         "num_hires":36,
         "thumbtack_url": "https://www.thumbtack.com/ca/el-sobrante/furniture-refinishing/jos-daniel-ramirez-vazquez/service/281762703885780102?utm_source=<partnerID>",
         "image_url": "https://d1vg1gqh4nkuns.cloudfront.net/i/345226232596922395/desktop/standard/thumb",
         "background_image_url": "https://production-next-images-cdn.thumbtack.com/i/323234489549873159/small/standard/hero",
         "featured_review": "José was very thorough in his inspection of our new home.  His report was very helpful.  Thanks, Mike!",
         "quote": {
            "starting_cost": 15000
         }
      },
      {
         "service_id":"327730960171982897",
         "business_name":"HandiTechGuy -Kevin Jacob",
         "rating":4.433333333333334,
         "num_reviews":30,
         "years_in_business":6,
         "num_hires":38,
         "thumbtack_url": "https://www.thumbtack.com/ca/corte-madera/electricians/handitechguy-kevin-jacob/service/351982634073939991?utm_source=<partnerID>",
         "image_url": "https://d1vg1gqh4nkuns.cloudfront.net/i/351984685684138006/desktop/standard/thumb",
         "background_image_url": "https://production-next-images-cdn.thumbtack.com/i/323234489549873159/small/standard/hero",
         "featured_review": "Kevin is the most thorough, knowledgeable and friendly home inspector. He leaves no details out and responded to my inquiries super quickly.",
         "quote": {
            "starting_cost": 20000
         }
      },
      ...
  ]
}

```

This endpoint is used to fetch a list of Thumbtack Pros for a given market. Markets are defined to be the combination
of a category and a zip code. Thumbtack will handle query translation for the Partner such that the Partner can provide
free form text as the category and Thumbtack will convert the provided text to the corresponding Thumbtack category. 
If a Partner does not have a query, they can directly provide the category name as the query.

Parameters for the endpoint will be passed in as URL query parameters.

### Parameters (passed in via URL)

Parameter | Type | Description | Required
--------- | ---- | ----------- | --------
category | string | Query string the user is searching for | Y
zip_code | string | Zip code the user is located in | Y
utm_source | string | Partner ID for attribution purposes | Y


### HTTP Endpoint (Production Environment)

`GET https://api.thumbtack.com/v1/partners/discoverylite/pros?zip_code=<zip_code>&category=<category>&utm_source=<utm_source>`

### HTTP Endpoint (Test Environment)

`GET https://staging-pro-api.thumbtack.com/v1/partners/discoverylite/pros?zip_code=<zip_code>&category=<category>&utm_source=<utm_source>`

### Response

Thumbtack will provide a JSON response that contains the following fields.

Parameter | Type | Description | Required
--------- | ---- | ----------- | --------
results | array | Array of Thumbtack Pros | Y
results.service_id | string | Thumbtack ID of the Pro's business | Y
results.business_name | string | Name of the Thumbtack Pro's business | Y
results.rating | number | Rating (1-5 scale) of the Pro | Y
results.num_reviews | number | Number of reviews the Pro has on Thumbtack | Y
results.years_in_business | number | Number of years the Pro has been in business | N
results.num_hires | number | Number of times the Pro has been hired on Thumbtack | N
results.thumbtack_url | string | Thumbtack URL for the Pro | Y
results.image_url | string | Image URL for the Pro's Thumbtack profile | Y
results.background_image_url | string | Background Image URL for the Pro's Thumbtack profile | Y
results.featured_review | string | Review text Thumbtack has chosen to highlight about the Pro | N
results.quote | object | Information around the cost of the job | N
results.quote.starting_cost | number | The tentative cost of the quote in cents, given that we have minimal information about the customer’s request | N
results.quote.cost_unit | string | The unit of measurement corresponding to the starting_cost. May be temporal (“Hour”), non-temporal (“Dog”, “Session”, “Visit”, “sq ft”, etc.), or omitted if starting_cost is a flat rate | N


## Categories

> Sample Request

```shell
curl https://api.thumbtack.com/v1/partners/discoverylite/categories?zip_code=<zip_code>&utm_source=<utm_source>
  -H "Authorization: AUTH_HEADER"
  -H "Content-Type:application/json" 
```

> The above command returns JSON structured like this:

```json
{
   "results":[
      {
         "categoryName":"air_conditioning_installation_replacement",
         "categoryDisplayName":"Central Air Conditioning Installation or Replacement",
         "activeServices":21,
         "url":"https://thumbtack.com/k/air-conditioner-installation/near-me?utm_source=test&utm_medium=partnership",
         "imageURL":"https://production-next-images-cdn.thumbtack.com/i/327878539097268344/small/standard/hero",
      },
      {
         "categoryName":"exterior_painting",
         "categoryDisplayName":"Exterior Painting",
         "activeServices":15,
         "url":"https://thumbtack.com/k/exterior-painting/near-me?utm_source=test&utm_medium=partnership",
         "imageURL":"https://production-next-images-cdn.thumbtack.com/i/323468071300677642/small/standard/hero",
      },
      ...
  ]
}

```

This endpoint is used to fetch a list of Thumbtack Categories for a given zip code. This endpoint is useful if there
does not yet exist a search query and the Partner merely wants to showcase the variety of categories Thumbtack 
actively supports. 

Parameters for the endpoint will be passed in as URL query parameters.

### Parameters (passed in via URL)

Parameter | Type | Description | Required
--------- | ---- | ----------- | --------
zip_code | string | Zip code the user is located in | Y
utm_source | string | Partner ID for attribution purposes | Y


### HTTP Endpoint (Production Environment)

`GET https://api.thumbtack.com/v1/partners/discoverylite/categories?zip_code=<zip_code>&utm_source=<utm_source>`

### HTTP Endpoint (Test Environment)

`GET https://staging-pro-api.thumbtack.com/v1/partners/discoverylite/categories?zip_code=<zip_code>&utm_source=<utm_source>`

### Response

Thumbtack will provide a JSON response that contains the following fields.

Parameter | Type | Description | Required
--------- | ---- | ----------- | --------
results | array | Array of Thumbtack categories | Y
results.categoryName | string | Name of the category (can use this category name as the `category` query param for the aforementioned Pros endpoint) | Y
results.categoryDisplayName | string | The category name to display to users | Y
results.activeServices | number | Number of active Thumbtack Pros for the category and zip code (null means that we can not get the number of active services for the (category, zip code) combination) | Y
results.url | string | Thumbtack URL for the category | Y
results.imageURL | string | Thumbtack image URL for the category | Y


# Contact Us

If you have any questions, feel free to reach out to us at
partnerproducts-eng AT thumbtack DOT com
