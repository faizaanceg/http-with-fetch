# http-with-fetch
A tiny abstraction over the `fetch` API.

## Description

`http-with-fetch` provides an easier way to write APIs using `fetch`.

## Installation

```
npm i http-with-fetch --save
```

## Usage

Intended to be used as a ES6 module.

```
import http from 'http-with-fetch'
let service = new http('/resource')
```

## Motivation
Consider the following code
```
let options = { mode: 'cors' }

fetch('/resource1', options)

fetch('/resource2', options)

fetch('/resource3', options)
  
fetch('/resource4', options)
```

If you were to do a POST at '/resource3', then you can't reuse `options` in the fetch call and the code would then become

```
let options = { mode: 'cors' }

let data = { 'some': 'data' }

fetch('/resource1', options)

fetch('/resource2', options)

fetch('/resource3', {
    mode: 'cors',
    method: 'POST',
    headers: {
        'content-type': 'application/json'
    }
    body: JSON.stringify(data)
})
  
fetch('/resource4', options)
```

This problem increases when each `fetch` does some common things and also its own specific stuff.

With `http-with-fetch` it can be rewritten as,

```
const data = { 'some': 'data' }

http.use(http.add('mode', 'cors'))

http.get('/resource1')

http.get('/resource2')

http.post('/resource3', JSON.stringify(data), http.add('headers', {'content-type': 'application/json'}))

http.get('/resource4')
```