# Ibotta Take-Home Project: Anagram API
Take-home project for Ibotta dev position where I create an anagram API.

## Table of Contents
- [About the API](#about-the-api)
- [Getting Started](#getting-started)
- [Endpoints](#endpoints)
    - [Get Anagrams](#get-anagrams)
    - [Get Metrics](#get-metrics)
    - [Add Words](#add-words)
    - [Are All Anagrams](#are-all-anagrams)
    - [Delete Word](#delete-word)
    - [Delete All Words](#delete-all-words)

## About the API
The API endpoints provide access to a data store of words.
Words can be added to and deleted from this data store using the API.
Anagrams can be fetched for a given word, and results from the data store will be returned.

## Getting Started

### For Ibotta: Using the Cloud Instance
Ibotta team, I have hosted this API on one of my AWS Lightsail servers.

It can be accessed at `18.220.164.93:3000`.

You shouldn't need to do anything with the files in the repo, but the documentation about the API endpoints below may be useful to you.

### Working with the Repo
This API uses Node. Make sure you have Node installed, then run the following commands:
```
npm install
npm run test
```
If you get errors you might need an updated version of Node. I built this using v7.8.0.

Once the tests are passing you can start the server with `npm run start`. It will host the service on port 3000 (configurable in "index.js").

#### The Files
- **index.js** - The entry-point for the server
- **words.route.js** - The route definitions for the API
- **WordStore.class.js** - The bulk of the logic is found here, the class that represents the data store.
- **WordStore.test.js** - Test file for logic in the WordStore class (and helper functions in that file). Called by `npm run test` command.
- **dictionary.txt.gz** - Compressed text file of all words in the English dictionary.
- **anagram_test.rb** - Test file that tests routes using an HTTP client in "anagram_client.rb". Run test using command `ruby anagram_test.rb` (requires ruby).

## Endpoints

### Get Anagrams

`GET /anagrams/:word.json?limit=n`

Anagrams of the given word in the data store are returned.
- Required Url Param: "word" (string) - the word for which anagrams will be fetched
- Optional Query Param: "limit" (integer) - limit the result set to a certain size
- Returns: (string[]) - the list of words in the store that are anagrams of the provided word

Examples:

`GET /anagrams/read.json`
- Result: `["dear", "dare"]`

`GET /anagrams/read.json?limit=1`
- Result: `["dear"]`

### Get Metrics
`GET /words/metrics.json`

Various numerical metrics about the words in the data store are returned.
- Returns: (object) - `{ count, min_length, max_length, median_length, average_length }`

Examples:

`GET /words/metrics.json`
- Result:
```
{
    "count": 4,
    "min_length": 1,
    "max_length": 4,
    "median_length": 4,
    "average_length": 3.25
}
```

### Add Words
`POST /words.json`

The provided words are added to the data store.
- Required Body: (string[]) - the words to be added to the data store
- Note: The array may also be provided through an object with a key of "words".

Examples:

```
POST /words.json
Body:
["dear", "dare", "read"]
```

```
POST /words.json
Body:
{ "words": ["dear", "dare", "read"] }
```

### Are All Anagrams
`POST /are-all-anagrams`

Determines whether all the provided words are anagrams of each other.
- Required Body: (string[]) - the words to be tested
- Returns: (boolean) - whether all the words are anagrams of each other
- Note: The array may also be provided through an object with a key of "words".

Examples:

```
POST /are-all-anagrams
Body:
["dear", "dare", "read"]
```
- Result: `true`

```
POST /are-all-anagrams
Body:
{ "words": ["dear", "dare", "read"] }
```
- Result: `true`

```
POST /are-all-anagrams
Body:
["dear", "a", "read"]
```
- Result: `false`

### Delete Word
`DELETE /words/:word.json?withAnagrams=false`

The provided word is removed from the data store, with an option to also remove its anagrams.
- Required Url Param: "word" (string) - the word to be removed from the data store
- Optional Query Param: "withAnagrams" (boolean) - if true, the word's anagrams will also be removed from the store [default: false]

Examples:

`DELETE /words/read.json`

`DELETE /words/dare.json?withAnagrams=true`

### Delete All Words
`DELETE /words.json`

All words are deleted from the data store.
