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

*   [schema](#schema)
*   [defaults](#defaults)
*   [headers](#headers)
*   [traktHistoryToCsv](#trakthistorytocsv)
    *   [Parameters](#parameters)
*   [options](#options)
*   [builder](#builder)
*   [mapTraktToLetterboxd](#maptrakttoletterboxd)
    *   [Parameters](#parameters-1)
*   [mergeWatchedWithRatings](#mergewatchedwithratings)
    *   [Parameters](#parameters-2)
*   [mapper](#mapper)
    *   [Parameters](#parameters-3)
*   [fetchMovies](#fetchmovies)
    *   [Parameters](#parameters-4)

### schema

[src/main/exporter.ts:7-21](https://github.com/bbeesley/trakt-to-letterboxd/blob/357568106da2624a752c4853e96816457d8b6da4/src/main/exporter.ts#L7-L21 "Source code on GitHub")

Schema for the output csv.
Based on <https://letterboxd.com/about/importing-data/>

### defaults

[src/main/mapper.ts:11-23](https://github.com/bbeesley/trakt-to-letterboxd/blob/357568106da2624a752c4853e96816457d8b6da4/src/main/mapper.ts#L11-L23 "Source code on GitHub")

Default values for letterboxd object shape

### headers

[src/main/fetcher.ts:14-18](https://github.com/bbeesley/trakt-to-letterboxd/blob/357568106da2624a752c4853e96816457d8b6da4/src/main/fetcher.ts#L14-L18 "Source code on GitHub")

HTTP headers to send with our request to trakt's api

### traktHistoryToCsv

[src/main/index.ts:20-54](https://github.com/bbeesley/trakt-to-letterboxd/blob/357568106da2624a752c4853e96816457d8b6da4/src/main/index.ts#L20-L54 "Source code on GitHub")

Export a trakt user's history to csv to be uploaded to letterboxd

#### Parameters

*   `props` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Properties passed from argv

    *   `props.userName` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The user whose data you want to export
    *   `props.fileName` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The name of the file to output the watchlist to
    *   `props.watchListFileName` &#x20;

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)\<void>** We dont return anything

### options

[src/main/fetcher.ts:23-25](https://github.com/bbeesley/trakt-to-letterboxd/blob/357568106da2624a752c4853e96816457d8b6da4/src/main/fetcher.ts#L23-L25 "Source code on GitHub")

The fetch options object (only really needs headers)

### builder

[src/main/exporter.ts:28-28](https://github.com/bbeesley/trakt-to-letterboxd/blob/357568106da2624a752c4853e96816457d8b6da4/src/main/exporter.ts#L28-L28 "Source code on GitHub")

The instance of CsvBuilder we'll use to export the data.
We need to remap the format of the last watched date to YYYY-MM-DD
to comply with letterboxd's formatting

### mapTraktToLetterboxd

[src/main/mapper.ts:31-44](https://github.com/bbeesley/trakt-to-letterboxd/blob/357568106da2624a752c4853e96816457d8b6da4/src/main/mapper.ts#L31-L44 "Source code on GitHub")

Maps a trakt history entry to a letterboxd history entry

#### Parameters

*   `movie` **TraktRatingMergedHistoryEntityType** A trackt movie history entity
*   `isWatchlist` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Whether to map the history or the watchlist, defaults to the history

Returns **LetterboxdHistoryEntityType** A letterboxd movie history entity

### mergeWatchedWithRatings

[src/main/fetcher.ts:34-55](https://github.com/bbeesley/trakt-to-letterboxd/blob/357568106da2624a752c4853e96816457d8b6da4/src/main/fetcher.ts#L34-L55 "Source code on GitHub")

Fetch the ratings of a user and merge them with the movie history based on the movie's Trakt ID

#### Parameters

*   `user` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The username we're getting data for
*   `watched` **TraktMovieHistoryType** A list of movies that have been watched

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)\<TraktRatingMergedHistoryType>** Promise that resolves to Trakt movie data with merged rating

### mapper

[src/main/mapper.ts:52-58](https://github.com/bbeesley/trakt-to-letterboxd/blob/357568106da2624a752c4853e96816457d8b6da4/src/main/mapper.ts#L52-L58 "Source code on GitHub")

Maps an array of trakt history entries to an array of letterboxd history entities

#### Parameters

*   `movieList` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)\<TraktRatingMergedHistoryEntityType>** Trakt History
*   `isWatchlist` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Whether to map the history or the watchlist, defaults to the history (optional, default `false`)

Returns **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)\<LetterboxdHistoryEntityType>** letterboxd history

### fetchMovies

[src/main/fetcher.ts:63-88](https://github.com/bbeesley/trakt-to-letterboxd/blob/357568106da2624a752c4853e96816457d8b6da4/src/main/fetcher.ts#L63-L88 "Source code on GitHub")

Fetches the user's history data from the trakt api

#### Parameters

*   `user` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The username we're getting data for
*   `isWatchlist` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Whether to fetch the history or the watchlist, defaults to the history (optional, default `false`)

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)<[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)\<LetterboxdHistoryEntityType>>** Promise that resolves to mapped Letterboxd data
