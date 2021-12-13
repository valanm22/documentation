# MeiliSearch 101

The purpose of this section is to introduce you to the main features of MeiliSearch. You can refer to the links in each chapter to dive deeper.

## Chapter 1: Filtering and sorting

### Filtering

MeiliSearch allows you to refine your search using filters. You can use any of the document fields for filtering by adding them to `filterableAttributes`.

Let's say you only want to view meteors that weigh less than 200g.

<CodeSamples id= "getting_started_filtering_md" />

```json
{"hits":[{"name":"Aachen","mass":21},{"name":"Emmaville","mass":127},{"name":"Erakot","mass":113},{"name":"Erevan","mass":107.2},{"name":"Fenghsien-Ku","mass":82},{"name":"Galapian","mass":132.7},{"name":"Galim (a)","mass":36.1},{"name":"Galim (b)","mass":28},{"name":"Garland","mass":102},{"name":"Grefsheim","mass":45.5},{"name":"Gurram Konda","mass":28},{"name":"Hachi-oji","mass":0.2},{"name":"Hotse","mass":180},{"name":"Hungen","mass":112},{"name":"Jamkheir","mass":22},{"name":"Jodzie","mass":30},{"name":"Kadonah","mass":89},{"name":"Karewar","mass":180},{"name":"Khetri","mass":100},{"name":"Kikino","mass":195}],"nbHits":114,"exhaustiveNbHits":false,"query":"","limit":20,"offset":0,"processingTimeMs":0}%
```

### Sorting

By default, MeiliSearch focuses on ordering results according to their relevancy. You can alter this sorting behavior so users can decide at search time what type of results they want to see first.

You can use any of the document fields as long as they contain numbers, strings, arrays of numeric values, or arrays of string values by adding them to `sortableAttributes`. Let's sort the meteors in the previous example  based on mass.

<CodeSamples id= "getting_started_sorting_md" />

```json
{"hits":[{"name":"Silistra","mass":0.15},{"name":"Hachi-oji","mass":0.2},{"name":"Chail","mass":0.5},{"name":"Delhi","mass":0.8},{"name":"Revelstoke","mass":1},{"name":"Natal","mass":1.4},{"name":"Perth","mass":2},{"name":"Niger (L6)","mass":3.3},{"name":"Niger (LL6)","mass":3.3},{"name":"Kusiali","mass":5},{"name":"Ras Tanura","mass":6.1},{"name":"Caratash","mass":8},{"name":"Cumulus Hills 04075","mass":9.6},{"name":"Patti","mass":12},{"name":"Piancaldoli","mass":13.1},{"name":"Bethlehem","mass":13.9},{"name":"Banswal","mass":14},{"name":"Barntrup","mass":17},{"name":"Bhagur","mass":18},{"name":"Red Canyon Lake","mass":18.41}],"nbHits":114,"exhaustiveNbHits":false,"query":"","limit":20,"offset":0,"processingTimeMs":1}%
```

- Is it a good idea to link this example to the previous one? Wouldn't the user expect the same results? Is it a good idea to mention limit at the beginning?

You will see all meteors weighing less than 200g sorted based on increasing mass. If you use `mass:desc`, MeiliSearch will sort them based on decreasing mass.

To learn more about `sortableAttributes` and how to configure them, refer to our [dedicated guide](/reference/features/sorting.md).

### Geosearch

- Can't demonstrate this using a gif so I added the code sample and I don't like this
- Do I add the result for each query? If so, I need to fix the indentation
- Where do I link the dataset?

MeiliSearch allows you to filter and sort results based on their geographic location. To use this feature, your documents need to have the `_geo` field.

Let's say you want to find out the what meteors crashed within a 210km radius of Bern:

<CodeSamples id= "getting_started_geoRadius_md" />

You should get the following meteors:

```json
[
  {
    "name":"Ensisheim",
  "id":"10039",
  "nametype":"Valid",
  "recclass":"LL6",
  "mass":"127000",
  "fall":"Fell",
  "year":"1492-01-01T00:00:00.000",
  "_geo": {
      "lat":47.86667,
      "lng":7.35}
  },
  {
    "name":"Épinal",
  "id":"10041",
  "nametype":"Valid",
  "recclass":"H5",
  "mass":"277",
  "fall":"Fell",
  "year":"1822-01-01T00:00:00.000",
  "_geo": {
      "lat":48.18333,
      "lng":6.46667}
  },
  {
    "name":"Menziswyl",
  "id":"15486",
  "nametype":"Valid",
  "recclass":"L5",
  "mass":"28.9",
  "fall":"Fell",
  "year":"1903-01-01T00:00:00.000",
  "_geo": {
      "lat":46.81867,
      "lng":7.21817}
  },
  {
    "name":"Ornans",
  "id":"18030",
  "nametype":"Valid",
  "recclass":"CO3.4",
  "mass":"6000",
  "fall":"Fell",
  "year":"1868-01-01T00:00:00.000",
  "_geo": {
      "lat":47.11667,
      "lng":6.15}
  },
  {
    "name":"Ortenau",
  "id":"18033",
  "nametype":"Valid",
  "recclass":"Stone-uncl",
  "mass":"4500",
  "fall":"Fell",
  "year":"1671-01-01T00:00:00.000",
  "_geo": {
      "lat":48.5,
      "lng":8.0}
  },
  {
    "name":"Alby sur Chéran",
  "id":"458",
  "nametype":"Valid",
  "recclass":"Eucrite-mmict",
  "mass":"252",
  "fall":"Fell",
  "year":"2002-01-01T00:00:00.000",
  "_geo": {
      "lat":45.82133,
      "lng":6.01533}
  },
  {
    "name":"Chervettaz",
  "id":"5341",
  "nametype":"Valid",
  "recclass":"L5",
  "mass":"705",
  "fall":"Fell",
  "year":"1901-01-01T00:00:00.000",
  "_geo": {
      "lat":46.55,
      "lng":6.81667}
  }
  ]
  ```

You can also use geosearch to sort results based on their distance from a specific location. Let's say you want to view meteors based on how close they were to the Taj Mahal:

<CodeSamples id= "getting_started_geoPoint_md" />

You should get the following meteors:

```json
[
  {
    "name":"Nagaria",
    "id":"16892",
    "nametype":"Valid",
    "recclass":"Eucrite-cm",
    "mass":"20",
    "fall":"Fell",
    "year":"1875-01-01T00:00:00.000",
    "_geo": {
      "lat":26.98333,
      "lng":78.21667
      },
      "_geoDistance":27449
  },
  {
    "name":"Kheragur",
    "id":"12294",
    "nametype":"Valid",
    "recclass":"L6",
    "mass":"450",
    "fall":"Fell",
    "year":"1860-01-01T00:00:00.000",
    "_geo": {
      "lat":26.95,
      "lng":77.88333
      },
      "_geoDistance":29558
  },
  {
    "name":"Kadonah",
    "id":"12221",
    "nametype":"Valid",
    "recclass":"H6",
    "mass":"89",
    "fall":"Fell",
    "year":"1822-01-01T00:00:00.000",
    "_geo": {
      "lat":27.08333,
      "lng":78.33333
      },
      "_geoDistance":30574
   },
  {
    "name":"Ambapur Nagla",
    "id":"2290",
    "nametype":"Valid",
    "recclass":"H5",
    "mass":"6400",
    "fall":"Fell",
    "year":"1895-01-01T00:00:00.000",
    "_geo": {
      "lat":27.66667,
      "lng":78.25},
      "_geoDistance":58385},
  {
    "name":"Moti-ka-nagla",
    "id":"16759",
    "nametype":"Valid",
    "recclass":"H6",
    "mass":"1500",
    "fall":"Fell",
    "year":"1868-01-01T00:00:00.000",
    "_geo": {
      "lat":26.83333,
      "lng":77.33333},
      "_geoDistance":79843},
  {
    "name":"Chandpur",
    "id":"5321",
    "nametype":"Valid",
    "recclass":"L6",
    "mass":"1100",
    "fall":"Fell",
    "year":"1885-01-01T00:00:00.000",
    "_geo": {
      "lat":27.28333,
      "lng":79.05},
      "_geoDistance":100378},
  {
    "name":"Ekh Khera",
    "id":"7777",
    "nametype":"Valid",
    "recclass":"H6",
    "mass":"840",
    "fall":"Fell",
    "year":"1916-01-01T00:00:00.000",
    "_geo": {
      "lat":28.26667,
      "lng":78.78333},
      "_geoDistance":141617},
  {
    "name":"Bahjoi",
    "id":"4922",
    "nametype":"Valid",
    "recclass":"Iron, IAB-sLL",
    "mass":"10322",
    "fall":"Fell",
    "year":"1934-01-01T00:00:00.000",
    "_geo": {
      "lat":28.48333,
      "lng":78.5},
      "_geoDistance":152277},
  {
    "name":"Delhi",
    "id":"6642",
    "nametype":"Valid",
    "recclass":"L5",
    "mass":"0.8",
    "fall":"Fell",
    "year":"1897-01-01T00:00:00.000",
    "_geo": {
      "lat":28.56667,
      "lng":77.25},
      "_geoDistance":173219},
  {
    "name":"Raghunathpura",
    "id":"22371",
    "nametype":"Valid",
    "recclass":"Iron, IIAB",
    "mass":"10200",
    "fall":"Fell",
    "year":"1986-01-01T00:00:00.000",
    "_geo": {
      "lat":27.72528,
      "lng":76.465},
      "_geoDistance":167213},
  {
    "name":"Rewari",
    "id":"22593",
    "nametype":"Valid",
    "recclass":"L6",
    "mass":"3332",
    "fall":"Fell",
    "year":"1929-01-01T00:00:00.000",
    "_geo": {
      "lat":28.2,
      "lng":76.66667},
      "_geoDistance":176996},
  {
    "name":"Bansur",
    "id":"4936",
    "nametype":"Valid",
    "recclass":"L6",
    "mass":"15000",
    "fall":"Fell",
    "year":"1892-01-01T00:00:00.000",
    "_geo": {
      "lat":27.7,
      "lng":76.33333},
      "_geoDistance":178446},
  {
    "name":"Moradabad",
    "id":"16740",
    "nametype":"Valid",
    "recclass":"L6",
    "mass":"70",
    "fall":"Fell",
    "year":"1808-01-01T00:00:00.000",
    "_geo": {
      "lat":28.78333,
      "lng":78.83333},
      "_geoDistance":194975},
  {
    "name":"Meerut",
    "id":"15469",
    "nametype":"Valid",
    "recclass":"H5",
    "mass":"22",
    "fall":"Fell",
    "year":"1861-01-01T00:00:00.000",
    "_geo": {
      "lat":29.01667,
      "lng":77.8},
      "_geoDistance":206145},
  {
    "name":"Kaee",
    "id":"12222",
    "nametype":"Valid",
    "recclass":"H5",
    "mass":"230",
    "fall":"Fell",
    "year":"1838-01-01T00:00:00.000",
    "_geo": {
      "lat":27.25,
      "lng":79.96667},
      "_geoDistance":190496},
  {
    "name":"Khetri",
    "id":"12296",
    "nametype":"Valid",
    "recclass":"H6",
    "mass":"100",
    "fall":"Fell",
    "year":"1867-01-01T00:00:00.000",
    "_geo": {
      "lat":28.01667,
      "lng":75.81667},
      "_geoDistance":238430},
  {
    "name":"Kasauli",
    "id":"30740",
    "nametype":"Valid",
    "recclass":"H4",
    "mass":"16820",
    "fall":"Fell",
    "year":"2003-01-01T00:00:00.000",
    "_geo": {
      "lat":29.58333,
      "lng":77.58333},
      "_geoDistance":271517},
  {
    "name":"Kusiali",
    "id":"12382",
    "nametype":"Valid",
    "recclass":"L6",
    "mass":"5",
    "fall":"Fell",
    "year":"1860-01-01T00:00:00.000",
    "_geo": {
      "lat":29.68333,
      "lng":78.38333},
      "_geoDistance":280891},
  {
    "name":"Akbarpur",
    "id":"427",
    "nametype":"Valid",
    "recclass":"H4",
    "mass":"1800",
    "fall":"Fell",
    "year":"1838-01-01T00:00:00.000",
    "_geo": {
      "lat":29.71667,
      "lng":77.95},
      "_geoDistance":282753},
  {
    "name":"Haripura",
    "id":"11829",
    "nametype":"Valid",
    "recclass":"CM2",
    "mass":"315",
    "fall":"Fell",
    "year":"1921-01-01T00:00:00.000",
    "_geo": {
      "lat":28.38333,
      "lng":75.78333},
      "_geoDistance":259664}
]
```

This response returns an additional `_geoDistance` field. `_geoDistance` represents the distance between the Taj Mahal and each meteor in meters.

To learn more about geosearch and how to configure it, refer to our [dedicated guide](/reference/features/geosearch.md).

## Chapter 2: Customizing relevancy

### Ranking rules

- What kind of example goes here?

Relevancy refers to the accuracy and effectiveness of search results. If search results are almost always appropriate, then they can be considered relevant, and vice versa. MeiliSearch has a number of features for fine-tuning the relevancy of search results. The most important tool among them is ranking rules.

MeiliSearch sorts search responses based on a set of consecutive rules called ranking rules. These rules are stored in an array of strings called `rankingRules`. You can update these ranking rules for each index. The default order for the ranking rules is as follows:

1. Words
2. Typo
3. Proximity
4. Attribute
5. Sort
6. Exactness

The order in which ranking rules are applied matters. The first rule in the array has the most impact, and the last rule has the least.

You can read more about ranking rules in our [dedicated guide](/learn/core_concepts/relevancy.md).

-> How do we go from ranking rules to explaining all these different attributes. Need some introduction or something

- Do we have any order for these attributes? Or do we go with alphabetical order?

### Displayed attributes

- Do we need the json responses here as well? or is the interface enough?

By default, all attributes are displayed in each matching document but you can update the settings to change that. If you access the MeiliSearch web interface at `http://127.0.0.1:7700/`, you will notice that you can view all of the attributes in the `movies` index.

![Web interface with default displayed attributes](/getting-started/default_displayed_attributes.png)

If you only want to view the `title`, `poster`, and `overview`, you can do so with the following query:

<CodeSamples id= "getting_started_update_displayedAttributes_md" />

![Web interface with updated displayed attributes](/getting-started/updated_displayed_attributes.png)  

### distinctAttribute

- no idea what the example should be

MeiliSearch lets you set one field per index as the distinct attribute. The distinct attribute will always be unique among returned documents. This means there will never be more than one occurrence of the same value in the distinct attribute field among the returned documents.

### Searchable attributes

By default, all attributes are searched for matching query words but you can configure the settings to change that.

Let's look at MeiliSearch's web interface for this example. When we search for `2012` with the default settings, MeiliSearch searches for it everywhere.

![default searchableAttributes](/getting-started/default_searchableAttributes.gif)

If you update the `searchableAttributes` to only contain `title`, MeiliSearch will only consider `title` during search. You will see fewer results.

![title as the only searchableAttribute](/getting-started/title_searchableAttributes.gif)

Please note that **MeiliSearch will still highlight matches in other attributes, but they won’t be used to compute results.**

-> Moving from the different attributes to stop words and synonyms? Not sure if this is the right order

### stop words and synonyms

- Is it a good idea to link one-way association or should it be the Synonyms page? This link does take the user to the synonyms page so it shouldn't be a problem
- Do I need to include examples? I don't think so. The Synonyms and Stop words pages are pretty brief, too much info here will make them useless. But is this detail enough for demonstration purposes? Maybe add a gif and update the examples I used in the text accordingly?
- Not sure if gifs is a good idea for stop words. Any suggestions?

MeiliSearch allows you to create a list of words that is ignored in your search queries. These words are called stop words. A good example is the word `the` in English.

If you search the `movies` index for `the cat` with the default settings, MeiliSearch will return a lot of results but not all of them will be relevant. After adding `the` to your list of stop words, MeiliSearch will ignore all documents containing `the` and return the ones with `cat` improving the speed and relevancy of your search.

You can read more about stop words in our [dedicated guide](/reference/features/stop_words.md).

A list of synonyms is useful if you have multiple words with the same meaning in your dataset. This will make your search results more relevant. So if you have `winnie` and `piglet` set as synonyms, searching for either words will show the same results.

-> Need to create a gif

The only exception is one-way association, you can read more about it in our [dedicated guide](/reference/features/synonyms.md#one-way-association).

## Chapter 3: Adding a visual UI

### Instant MeiliSearch

### Manipulating results (highlight, crop, limit)

Even though the search is relevant by default, MeiliSearch offers many parameters that you can play with to refine your search or change the format of the returned document.

This section covers some of the important search parameters but you can read about all of them in our [search parameters guide](/reference/features/search_parameters.md).

#### attributesToHighlight (Is this that important?)

This highlights matching query terms in the specified attributes by enclosing them in `<em>` tags. `attributesToHighlight` only works on strings, numbers, arrays, and objects.

When this parameter is set, returned documents include a `_formatted` object containing the highlighted terms.

<CodeSamples id= "getting_started_attributesToHighlight_md" />

```json
{
  "id": "50393",
  "title": "Kung Fu Panda Holiday",
  "poster": "https://image.tmdb.org/t/p/w1280/gp18R42TbSUlw9VnXFqyecm52lq.jpg",
  "overview": "The Winter Feast is Po's favorite holiday. Every year he and his father hang decorations, cook together, and serve noodle soup to the villagers. But this year Shifu informs Po that as Dragon Warrior, it is his duty to host the formal Winter Feast at the Jade Palace. Po is caught between his obligations as the Dragon Warrior and his family traditions: between Shifu and Mr. Ping.",
  "release_date": 1290729600,
  "_formatted": {
    "id": "50393",
    "title": "Kung Fu Panda Holiday",
    "poster": "https://image.tmdb.org/t/p/w1280/gp18R42TbSUlw9VnXFqyecm52lq.jpg",
    "overview": "The <em>Winter Feast</em> is Po's favorite holiday. Every year he and his father hang decorations, cook together, and serve noodle soup to the villagers. But this year Shifu informs Po that as Dragon Warrior, it is his duty to host the formal <em>Winter Feast</em> at the Jade Palace. Po is caught between his obligations as the Dragon Warrior and his family traditions: between Shifu and Mr. Ping.",
    "release_date": 1290729600
  }
}
```

#### attributesToCrop

By default, MeiliSearch responses return the entire value of all attributes. You can use the `attributesToCrop` parameter to crop the value of selected attributes.

Let's take `overview` as an example, MeiliSearch will return the whole value for it. If you want to only view the first `10` bytes, you can use:

<CodeSamples id= "getting_started_attributesToCrop_md" />

You will get the following response with the cropped text in the `_formatted` object:

```json
{
  "id": "50393",
  "title": "Kung Fu Panda Holiday",
  "poster": "https://image.tmdb.org/t/p/w1280/gp18R42TbSUlw9VnXFqyecm52lq.jpg",
  "overview": "The Winter Feast is Po's favorite holiday. Every year he and his father hang decorations, cook together, and serve noodle soup to the villagers. But this year Shifu informs Po that as Dragon Warrior, it is his duty to host the formal Winter Feast at the Jade Palace. Po is caught between his obligations as the Dragon Warrior and his family traditions: between Shifu and Mr. Ping.",
  "release_date": 1290729600,
  "_formatted": {
    "id": "50393",
    "title": "Kung Fu Panda Holiday",
    "poster": "https://image.tmdb.org/t/p/w1280/gp18R42TbSUlw9VnXFqyecm52lq.jpg",
    "overview": "this year Shifu informs",
    "release_date": 1290729600
  }
}
```

#### limit

The `limit` decides the maximum number of documents MeiliSearch returns for a query. The default is `20` but you can change it.

If you search the `movies` index for `shifu`, MeiliSearch will return the first 20 movies. If you were to update the limit to `10`:

<CodeSamples id= "getting_started_limit_md" />

MeiliSearch would now return the first ten results.

### Faceted search

- can we use this with the interface?

**The following content is currently homeless:**

======================================================================

Placeholder search (this is a behavior needs to mentioned briefly)

If you make a search without inputting any query words, MeiliSearch will return all the documents in that index sorted by its custom [ranking rules](/reference/features/settings.md#ranking-rules) and [sorting rules](/reference/features/sorting.md#sorting). This feature is called placeholder search.

Phrase search (Don't know where this goes)

If you enclose search terms in double quotes ("), MeiliSearch will only return documents that contain those terms in the order they were given. This gives users the option to make more precise search queries.

======================================================================

## Chapter 4: Configuration and security

### Configuration options

- I added too many, maybe we need fewer ones
- Do we want to list these in any particular order? Or alphabetical order?

MeiliSearch allows you to configure your entire instance through **environment variables** and **command-line options**. You can configure your instance with environment variables before launch and with command line options at launch.

This chapter covers some of the important configuration options but you can read about all of them in our [configuration guide](/reference/features/configuration.md).

#### Database path

By default, all your database files will be created in a folder called `data.ms`. You can configure this using the `MEILI_DB_PATH` environment variable or the `--db-path` CLI option.

#### Environment

You can run your MeiliSearch instance in `production` or `development`. By default, it runs in `development`, you can change that using the `MEILI_ENV` environment variable or the `--env` CLI option.

#### Master key

You can protect your MeiliSearch instance by setting a master key. You can configure this using the `MEILI_MASTER_KEY` environment variable or the `--master-key` CLI option. MeiliSearch requires a master key when the `--env` is set to `production`.

#### Disable analytics (I think we can use this here instead of payload limit size)

By default, MeiliSearch automatically collects data from all instances that do not opt out using this flag. You can configure it using the `MEILI_NO_ANALYTICS` environment variable or the `--no-analytics` CLI option.

You can read more about our data collection policy [here](/learn/what_is_meilisearch/telemetry.md).

#### Payload limit size

MeiliSearch accepts JSON, NDJSON, and CSV payloads. The default payload limit is 104857600 (~100MB) but you can update it using the `MEILI_HTTP_PAYLOAD_SIZE_LIMIT` environment variable or the `--http-payload-size-limit` CLI option.

#### Dumps (Can we mention this here?)

By default, MeiliSearch creates dump files in the `dumps/` directory. You can configure this using the `MEILI_DUMPS_DIR` environment variable or the `--dumps-dir` CLI option.

#### Snapshots (Can we mention this here?)

- Which one do we mention here? Activating schedules snapshots or snapshot destination?

### Protecting MeiliSearch

- not sure how much content we need here

MeiliSearch allows you to protect your instances by using API keys. API keys give you fine-grained control over which users can access which indexes, endpoints, and routes.

You can protect your MeiliSearch instance by supplying it with an alphanumeric string representing your `master` key. MeiliSearch requires a master key when the `--env` is set to `production`. When you launch a secured instance for the first time, MeiliSearch creates two default API keys: `Default Search API Key` and `Default Admin API Key`.

The `master` key is the only key with access to the [`/keys` endpoint](/reference/api/keys.md). You can use this endpoint to create, update, list, and delete API keys.

You can read more about security in our [dedicated guide](/reference/features/authentication.md).
