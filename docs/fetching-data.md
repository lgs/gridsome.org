# Fetching data
Fetch content from local files or external APIs and store the data in a local database. A unified GraphQL Data layer lets you extract only the data you need from the database and use it in your Vue.js components.

![Fetching data](./images/fetching-data.png)


## Use data source plugins
Gridsome data source plugins are added in `gridsome.config.js`. You can find available data source plugins in the [Plugins directory](/plugins).


Here is an example of the [file-system](/plugins/source-filesystem) source added to config:
```js
module.exports = {
  plugins: [
    {
      use: '@gridsome/source-filesystem',
      options: {
        index: ['README'],
        path: 'docs/**/*.md',
        typeName: 'DocPage',
      }
    },
    {
      // another data source
    },
  ]
}
```

`typeName` will be the name of the GraphQL collection and needs to be unique. This example will add a **DocPage** collection.

Every data source has different options, so take a look at their documentation to learn more.


## Add data from APIs

Gridsome adds data to the GraphQL data layer with the **Data store API** and the `api.loadSource` function. To use the API you need a `gridsome.server.js` file in the root folder of your Gridsome project.



Learn more about the [Data store API here](/docs/data-store-api)

A typical `gridsome.server.js` will look something like this:

```js
const axios = require('axios')

module.exports = function (api) {
  api.loadSource(async store => {
    const { data } = await axios.get('https://api.example.com/posts')

    const contentType = store.addContentType({
      typeName: 'BlogPosts'
    })

    for (const item of data) {
      contentType.addNode({
        id: item.id,
        title: item.title
      })
    }
  })
}
```

> Data is feched when starting a development server or start of a production build. You need to restart server for changes in **gridsome.server.js** to take affect.


## Add data from local files
..

### Markdown
..

### Images
..

### YAML
..

### CSV
..

### JSON

Fetching content from local json files as a data source is easy, just like all the other data format (YAML, Markdown ...).

1. First install `@gridsome/source-filesystem` and `@gridsome/transformer-json` plugins
2. Edit `gridsome.config.js` :

```
module.exports = {
  siteName: 'My Project',
  plugins: [
    {
      use: '@gridsome/source-filesystem',
      options: {
        path: 'data/**/*.json',
        typeName: 'JsonData',
        route: '/data/:slug',
        create: true,
        json: {
          plugins: [
            '@gridsome/transformer-json'
          ]
        }
      }
    },
  ]
}
```

3. Put your JSON file and directoy (data/your-json-file.json) in the root of your app, which is the src parent.
4. Start exploring & testing your data model with [GraphQL explorer (Playground)](https://gridsome.org/docs/querying-data/#explore--test-queries) 
5. When you are happy with query axploration, bump it in your app like the following [Query data in Pages](https://gridsome.org/docs/querying-data/#query-data-in-pages)
6. Profit
