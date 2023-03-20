LOTR Movie Quotes
==================

This app generates a random quote from the "Lord of the Rings" film series. We depend on [The One Ring API](https://the-one-api.dev/), which we use as our source of truth for quotes and character information. To generate character images from movie stills, we used [elmah.io's free image to Base64 encoder](https://elmah.io/tools/base64-image-encoder/) for the base64 image encoder.

![Example screenshot gif showing Boromir in his 'One does not simply' speech](screenshot.gif)

## Development

Feel free to make changes to this app. You simply need to request an API key from The One Ring API to get started.

The following commands will be useful to you when developing (e.g. when running `pixlet render`):

1. `dev_api_key` (required): the API key to use when making requests to the One Ring API when running locally
2. `character_id` (optional): include this if you want to only generate a quote for a particular character. The character ids must exist in [this github gist](https://gist.github.com/pandincus/af0e64d66c646613d0d7081a1183c964).
3. `quote_id` (optional): include this if you want to only generate a very specific quote. The quote id must exist in The One Ring API's database, e.g. the above screenshot is quote id `5cd96e05de30eff6ebccef87`.
4. `debug` (optional): set to this to `true` if you want to print debug statements to the console
5. `bypass_cache` (optional): Set to this to `true` if you DO NOT want to cache data and instead want to always pull from network sources.

Here's an example of using all of these paramters to render the app locally:

```
pixlet render lotr_movie_quotes.star dev_api_key=my_api_key character_id=5cd99d4bde30eff6ebccfc57 debug=true bypass_cache=true
```

You can also render the app using `pixlet serve` (the above parameters simply need to be passed to the url)

## What the App Does

Assuming the cache is empty, the app makes the following requests in order populate the cache:

1. We load [this github gist](https://gist.github.com/pandincus/af0e64d66c646613d0d7081a1183c964) to pull supported character ids and images.
2. We connect to [The One Ring API](https://the-one-api.dev/) and make two requests:
  1. First, we use their Movies API to get basic movie profile information.
  2. Then, we use their Quotes API to get all quotes for the supported characters.

We then render the experience.

The cache TTL is 4 hours, and we maintain 3 separate caches:
1. `lotr_characters`: The LOTR characters and images that we load from the CSV file (stored in a GitHub gist)
2. `lotr_quotes`: The LOTR quotes that we load from the One Ring API (we load all quotes at once, up to 2500. This does take about a second to load)
3. `lotr_movies`: The LOTR movies that we load from the One Ring API