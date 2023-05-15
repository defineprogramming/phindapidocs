Sure, here is the rewritten Github README in proper markdown:

# Unofficial Phind API Documentation

## Disclaimer

This documentation is for educational purposes only. It may be incorrect, outdated, or missing features. Some aspects are unconfirmed, such as the request option parameters.

## What is Phind?

Phind is an AI service that provides natural language responses to user questions and prompts. It has a low-powered model that rewrites user prompts and then uses search results to generate an answer with GPT-4, the higher powered model used when 'Use Best Model' is checked. This documentation does not cover intermediate mode, which is presumed to use `gpt-3.5-turbo` or `text-davinci-003`.

## Base URL

```
http://phind.com/api
```

## API Endpoints

### /api/web/search

Search the web based on a user's question or prompt.

**Method:** POST

**Request Body:** JSON

```
{
  "q": "String query",
  "browserLanguage": "String language code"
}
```

**Response:** JSON

```
[
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

**Request Body:** JSON

```
{
  "key": "String key",
  "value": "String value"
}
```

**Response:** No response

### /api/infer/answer

Generate an answer from the Phind AI model using web search results.

**Method:** POST

**Request Body:** JSON

```
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

Generate a follow-up answer from the Phind AI model using web search results and conversation context.

**Method:** POST

**Request Body:** JSON

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
