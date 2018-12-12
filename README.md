# Vuex Rest API Cache

[![Build Status](https://travis-ci.org/netsells/vuex-rest-api-cache.svg?branch=master)](https://travis-ci.org/netsells/vuex-rest-api-cache)
[![codecov](https://codecov.io/gh/netsells/vuex-rest-api-cache/branch/master/graph/badge.svg)](https://codecov.io/gh/netsells/vuex-rest-api-cache)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/d8741c7400b54067b52fb8f2c2745a1f)](https://www.codacy.com/app/samboylett/vuex-rest-api-cache?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=netsells/vuex-rest-api-cache&amp;utm_campaign=Badge_Grade)

### Vuex Rest API action creator and model cacher

This module makes it easier to create Vuex store actions for communicating with

## Installation

```sh
yarn add @netsells/vuex-rest-api-cache
```

## Setup

```javascript
import Vuex from 'vuex';
import Vrac from 'vuex-rest-api-cache';

const posts = new Vrac({
    baseUrl: `${ API_URL }/posts`,
    children: {
        comments: {
            baseUrl: `${ API_URL }/posts/:post_id/comments`
        },
    },
}).store;

const store = new Vuex.Store({
    modules: {
        posts,
    },
});
```

## Usage

This includes usage examples for root models (e.g. `/api/v1/posts`) and for child models (e.g. `/api/v1/posts/:post_id/comments`)

### Index

Get a list of models from the API. Will cache each model. Will always create a
new API request instead of using the cache.

```javascript
const posts = await this.$store.dispatch('posts/index');

const comments = await this.$store.dispatch('posts/comments/index', {
    fields: {
        post_id: 1,
    },
});
```

### Create

Create a model. Will cache the model returned by the API. Will always create a
new API request.

```javascript
const post = await this.$store.dispatch('posts/create', {
    fields: {
        text: 'Foo bar',
    },
});

const comment = await this.$store.dispatch('posts/comments/create', {
    fields: {
        post_id: post.id,
        message: 'Foo bar',
    },
});
```

### Read

Read a model. Will cache the model returned by the API. Will always use a cached
model over reading from the API.

```javascript
const post = await this.$store.dispatch('posts/read', {
    fields: {
        id: 45,
    },
});

const comment = await this.$store.dispatch('posts/comments/read', {
    fields: {
        post_id: post.id,
        id: 3,
    },
});
```

### Update

Update a model. Will cache the model returned by the API. Will always create a
new API request.

```javascript
const post = await this.$store.dispatch('posts/update', {
    fields: {
        id: 45,
        text: 'Bar foo',
    },
});

const comment = await this.$store.dispatch('posts/comments/update', {
    fields: {
        post_id: post.id,
        id: 3,
        message: 'Hello world',
    },
});
```

### Destroy

Destroy a model. Will remove the model from the cache. Will always create a
new API request.

```javascript
await this.$store.dispatch('posts/destroy', {
    fields: {
        id: 45,
    },
});

await this.$store.dispatch('posts/comments/destroy', {
    fields: {
        post_id: post.id,
        id: 3,
    },
});
```
