# trakt-to-letterboxd

[![CircleCI](https://circleci.com/gh/bbeesley/trakt-to-letterboxd/tree/master.svg?style=svg)](https://circleci.com/gh/bbeesley/trakt-to-letterboxd/tree/master) [![codecov](https://codecov.io/gh/bbeesley/trakt-to-letterboxd/branch/master/graph/badge.svg)](https://codecov.io/gh/bbeesley/trakt-to-letterboxd) [![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)

## Description

A package to migrate your trakt movie history and ratings to letterboxd. Optionally the trakt Watchlist can also be exported. Currently letterboxd only supports importing from a csv, so thats all this package does at the moment. When a proper API is added, I'll update this to push the data right into your letterboxd history.

## Usage

The easiest way to use this currently is with npx. You can install that globally with `yarn global add npx` or `npm i -g npx`. Once you have that, just run:

```sh
npx trakt-to-letterboxd -u username -f filename
```

where username is the user whose data you want to export, and filename is the name of the csv file you want to output to.

To export your watchlist use

```sh
npx trakt-to-letterboxy -u username -w watchListFilename
```

Both options can also be used together:

```sh
npx trakt-to-letterboxd -u username -f filename -w watchListFilename
```

If only a username is given it will correspond to the following command:

```sh
npx trakt-to-letterboxd -u username -f history.csv -w watchlist.csv
```

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### Table of Contents

-   [traktHistoryToCsv](#trakthistorytocsv)
    -   [Parameters](#parameters)
-   [headers](#headers)
-   [options](#options)
-   [mergeWatchedWithRatings](#mergewatchedwithratings)
    -   [Parameters](#parameters-1)
-   [fetchMovies](#fetchmovies)
    -   [Parameters](#parameters-2)
-   [defaults](#defaults)
-   [mapTraktToLetterboxd](#maptrakttoletterboxd)
    -   [Parameters](#parameters-3)
-   [mapper](#mapper)
    -   [Parameters](#parameters-4)
-   [schema](#schema)
-   [builder](#builder)

### traktHistoryToCsv

[src/main/index.js:21-54](https://github.com/baswag/trakt-to-letterboxd/blob/225302ec281782b2035f94bdca2fa4c9aac75a90/src/main/index.js#L21-L54 "Source code on GitHub")

Export a trakt user's history to csv to be uploaded to letterboxd

#### Parameters

-   `props` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Properties passed from argv
    -   `props.userName` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The user whose data you want to export
    -   `props.fileName` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The name of the file to output the watchlist to
    -   `props.watchListFileName`  

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;void>** We dont return anything

### headers

[src/main/fetcher.js:15-19](https://github.com/baswag/trakt-to-letterboxd/blob/225302ec281782b2035f94bdca2fa4c9aac75a90/src/main/fetcher.js#L15-L19 "Source code on GitHub")

HTTP headers to send with our request to trakt's api

### options

[src/main/fetcher.js:24-26](https://github.com/baswag/trakt-to-letterboxd/blob/225302ec281782b2035f94bdca2fa4c9aac75a90/src/main/fetcher.js#L24-L26 "Source code on GitHub")

The fetch options object (only really needs headers)

### mergeWatchedWithRatings

[src/main/fetcher.js:35-52](https://github.com/baswag/trakt-to-letterboxd/blob/225302ec281782b2035f94bdca2fa4c9aac75a90/src/main/fetcher.js#L35-L52 "Source code on GitHub")

Fetch the ratings of a user and merge them with the movie history based on the movie's Trakt ID

#### Parameters

-   `user` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The username we're getting data for
-   `watched` **TraktMovieHistoryType** A list of movies that have been watched

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;TraktRatingMergedHistoryType>** Promise that resolves to Trakt movie data with merged rating

### fetchMovies

[src/main/fetcher.js:60-81](https://github.com/baswag/trakt-to-letterboxd/blob/225302ec281782b2035f94bdca2fa4c9aac75a90/src/main/fetcher.js#L60-L81 "Source code on GitHub")

Fetches the user's history data from the trakt api

#### Parameters

-   `user` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The username we're getting data for
-   `isWatchlist` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Whether to fetch the history or the watchlist, defaults to the history (optional, default `false`)

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;LetterboxdHistoryEntityType>>** Promise that resolves to mapped Letterboxd data

### defaults

[src/main/mapper.js:12-24](https://github.com/baswag/trakt-to-letterboxd/blob/225302ec281782b2035f94bdca2fa4c9aac75a90/src/main/mapper.js#L12-L24 "Source code on GitHub")

Default values for letterboxd object shape

### mapTraktToLetterboxd

[src/main/mapper.js:32-45](https://github.com/baswag/trakt-to-letterboxd/blob/225302ec281782b2035f94bdca2fa4c9aac75a90/src/main/mapper.js#L32-L45 "Source code on GitHub")

Maps a trakt history entry to a letterboxd history entry

#### Parameters

-   `movie` **TraktRatingMergedHistoryEntityType** A trackt movie history entity
-   `isWatchlist` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Whether to map the history or the watchlist, defaults to the history

Returns **LetterboxdHistoryEntityType** A letterboxd movie history entity

### mapper

[src/main/mapper.js:53-56](https://github.com/baswag/trakt-to-letterboxd/blob/225302ec281782b2035f94bdca2fa4c9aac75a90/src/main/mapper.js#L53-L56 "Source code on GitHub")

Maps an array of trakt history entries to an array of letterboxd history entities

#### Parameters

-   `movieList` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;TraktRatingMergedHistoryEntityType>** Trakt History
-   `isWatchlist` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Whether to map the history or the watchlist, defaults to the history (optional, default `false`)

Returns **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;LetterboxdHistoryEntityType>** letterboxd history

### schema

[src/main/exporter.js:8-22](https://github.com/baswag/trakt-to-letterboxd/blob/225302ec281782b2035f94bdca2fa4c9aac75a90/src/main/exporter.js#L8-L22 "Source code on GitHub")

Schema for the output csv.
Based on <https://letterboxd.com/about/importing-data/>

### builder

[src/main/exporter.js:29-29](https://github.com/baswag/trakt-to-letterboxd/blob/225302ec281782b2035f94bdca2fa4c9aac75a90/src/main/exporter.js#L29-L29 "Source code on GitHub")

The instance of CsvBuilder we'll use to export the data.
We need to remap the format of the last watched date to YYYY-MM-DD
to comply with letterboxd's formatting
