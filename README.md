# Unofficial Phind API Documentation
With help from reverse engineering and Chrome Dev Tools.
### DISCLAIMER: EDUCATIONAL PURPOSES ONLY. THIS DOCUMENTATION MAY BE INCORRECT, OUTDATED, OR MISSING FEATUEES. SOME ASPECTS ARE UNCONFIRMED - LIKE REQUEST OPTION PARAMETERS. 

Phind is an AI service that provides natural language responses to user questions and prompts. It has a low powered model rewrite user prompts and then uses search results to generate an answer with GPT-4.

## Base URL http://phind.com/api

## API Endpoints

### /api/web/search

Search the web based on a user's question or prompt.

**Method:** POST

**Request Body:**  json
```

 {

   "q": "String query", 

   "browserLanguage": "String language code" 

 }```

**Response:**  json

``` [

   {

     "title": "String title",

     "url": "String url",

     "is_source_local": Boolean,

     "is_source_both": Boolean,

     "description": "String description",

     "profile": {

       "name": "String name",

       "url": "String url",

       "long_name": "String long name",

       "img": "String image url"

     },

     "language": "String language code",

     "family_friendly": Boolean,

     "type": "String type", 

     "subtype": "String subtype",

     "meta_url": {

       "scheme": "String scheme",

       "netloc": "String netloc",

       "hostname": "String hostname", 

       "favicon": "String favicon url",

       "path": "String path"

     },

     "thumbnail": {

       "src": "String thumbnail url",

       "original": "String original thumbnail url",

       "logo": Boolean

     },

     "age": "String published date",

     "article": {

       "author": [

         {

           "type": "String type", 

           "name": "String name",

           "url": "String url"

         }

       ],

       "date": "String published date",

       "publisher": {

         "type": "String type",

         "name": "String name",

         "url": "String url",

         "thumbnail": {

           "src": "String publisher logo url",

           "original": "String original publisher logo url"

         }

       }

     }

   }

 ]
```

### /api/db/cache

Cache data in the Phind database. Used internally.

**Method:** POST

**Request Body:** json
```

 {

   "key": "String key",

   "value": "String value" 

 } 

**Response:** No response

### /api/infer/answer 

Generate an answer from the Phind AI model using web search results.

**Method:** POST

**Request Body:** json

 {

   "question": "String question",

   "webResults": [web search response],

   "options": {

     "temperature": Float,

     "max_tokens": Integer,

     "top_p": Float,

     "repetition_penalty": Float,

     "presence_penalty": Float,

     "frequency_penalty": Float,

     "curiosity_bonus": Float 

   }

 } 
```

**Response:**  
```

data: String generated response, by word (10 word output = 10x data: word lines)
```
### /api/infer/followup/answer

Generate a follow up answer from the Phind AI model using web search results and conversation context.

**Method:** POST 

**Request Body:**  json
```

 {

   "question": "String question", 

   "questionHistory": ["String previous questions"],

   "answerHistory": ["String previous answers"],

   "webResults": [web search response],

   "options": {

     "temperature": Float, 

     "max_tokens": Integer,

     "top_p": Float,

     "repetition_penalty": Float,

     "presence_penalty": Float,

     "frequency_penalty": Float,

     "curiosity_bonus": Float

   }

 }
```

**Response:**

Same as /api/infer/answer

### /api/infer/followup/rewrite

Have the Phind AI model rewrite a user's follow up question using conversation context.

**Method:** POST

**Request Body:** json 
```

 {

   "questionToRewrite": "String question to rewrite",

   "questionHistory": ["String previous questions"],

   "answerHistory": ["String previous answers"]  

 }
```

**Response:** json
```

 {

   "query": "String rewritten query" 

 }
```


