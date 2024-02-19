# Axios Cheat Sheet
Axios Cheat Sheet with the most needed stuff..


<br><br>

# Github
- https://github.com/axios/axios












<br><br>
_________________________________________________
_________________________________________________
<br><br>


# Instance

<br><br>


## Create custom axios instance
```javascript
const axiosP1 = axios.create({
  baseURL: `http://${process.env.HOST}:${process.env.PORT}`,
  headers: {'id': 'test'}
})
```




















<br><br>
_________________________________________________
_________________________________________________
<br><br>


# HEAD

<br><br>


## Example
```javascript
let res = await axios.head('http://webcode.me');
console.log(`Status: ${res.status}`)
console.log(`Server: ${res.headers.server}`)
console.log(`Date: ${res.headers.date}`)
```
























































<br><br>
_________________________________________________
_________________________________________________
<br><br>


# POST

<br><br>


## POST with JSON
```javascript
// await
const res = await axios.post(  window.location.origin + '/secure', { client_id: 'a', client_secret: 'b'  }, {
  headers: { authorization: accessToken.data['access_token'] }
});
```


<br><br>


## POST with form params
```javascript
const querystring = require('querystring')

const data = querystring.stringify({
    project: projectId,
    id: statisticId
})

const emailStatisticsDoc = await axios.post(uri, data, {headers: {'Content-Type': 'application/x-www-form-urlencoded' }})
```



<br><br>


## POST with Image
```javascript
const path = `${__dirname}/img.png`

const imageBuffer = await fs.readFile(path)

const img = await fs.readFile(path, "binary")
constimageUint8Array = Buffer.from(img)




/* ---- METHOD #1 ---- */
const FormData = require('form-data')

// notice that you must always create new form in case you want to use a function/method for future creations
const form = new FormData()
form.append('file', imageBuffer, 'test.png')

// you cann add additional data to your body aswell
form.append('name', 'bla blub')

const headers = {...form.getHeaders()}

const res = await axios.post(uri, form, {
    headers: headers
})





/* ---- METHOD #2 ---- */
const res = await axiosP1.post(`${url}/upload`, imageBuffer, {
    headers: {
        'Content-Type': 'image/png'
    }
})
```






<br><br>


## POST with Binary File at Elastic Searcg
```javascript
    const indexName = `test_${projectId}_c`
    const url = `http://127.0.0.1:9200/${indexName}/_bulk`
    const payload = await fs.readFile(filePath)
    const headers = { 'Content-Type': 'application/x-ndjson' }

    const res = await axios.post(url, payload, {
        headers, validateStatus: status => status < 600
    })
```







<br><br>
_________________________________________________
_________________________________________________
<br><br>


# GET

<br><br>

## Example
```javascript
const response = await axios.get('/user?ID=12345');
```




<br><br>

## Download an image and convert it to base64
```javascript
const imgBuffer = await axios.get(imgUrl, {
  responseType: 'arraybuffer'
})
```






































<br><br>
_________________________________________________
_________________________________________________
<br><br>


# Error
- For full details about the error use **e.response.data**
```javascript
 try {
  res = await axios.post(uri,{
      headers: headers,
      data: form_data
  })
} catch (e){
  console.log(e.response.data)
}
```

<br><br>
<br><br>

# Send status code instead of catch error
- If you get a 404 with axios you will get an error which you must catch. Alternative you can return a response
```javascript
// global
const axiosP1 = axios.create({
  baseURL: `http://${process.env.HOST}:${process.env.PORT0}`,
  headers: { 'project-id': 'test_project1' },
  validateStatus: function (status) {
    return status < 600;
  }
})

// directly
axios.post('/formulas/create', {
     name: "",
     parts: ""
}, { validateStatus: function (status) {
        return status < 600; // Reject only if the status code is greater than or equal to 500
      }})
  .then( 
 (response) => { console.log(response) },
 (error) => { console.log(error) }
 );
```





































































<br><br>
_________________________________________________
_________________________________________________
<br><br>


# axios-retry
```javascript
// app.js
const axios = require('axios')
require('./utils')(axios)

const response = await axios({
  method: 'GET',
  url: 'https://httpstat.us/503',
}).catch((err) => {
  if (err.response.status !== 200) {
    throw new Error(`API call failed with status code: ${err.response.status} after 3 retry attempts`);
  }
});










// utils.js
'use strict'

const axiosRetry = require('axios-retry')
const retriesAmount = 3

module.exports = (axios, retryCondition = undefined) => {
    /**
     * Will retry on any status code
     * @param {*} error 
     * @returns 
     */
    const retryConditionAll = error => {
        if(error.response?.status) {
            return true
        }
    }

    /**
     * 
     * @param {*} retryCount 
     * @returns 
     */
    const retryDelay = (retryCount, e) => {
        if(retryCount === retriesAmount) {
            // Add informations to the error to detect if axios retry was used
            e.axiosRetry = true
            throw e
        }

        console.log(`retry attempt: ${retryCount}`)
        return retryCount * 2000 // time interval between retries
    }

    const cfg = {
        retries: retriesAmount, // number of retries
        retryDelay,
        ...(retryCondition === 'all' ? { retryCondition: retryConditionAll } : {})
    }

    axiosRetry(axios, cfg)
}
```









<br><br>
_________________________________________________
_________________________________________________
<br><br>


# Caching
```
const axios = require('axios')
const memoize = require('memoizee')

const axiosP1 = axios.create({
    baseURL: `http://${process.env.HOST}:${process.env.PORT0}`,
    headers,
    validateStatus (status) {
        return status < 600
    }
})

const memoizedAxiosGet = memoize(url => {
    return axiosP1.get(url)
}, {
    // Definieren Sie eine Option für Memoizee, die die Cachegröße beschränkt.
    max: 100,
    // Definieren Sie eine Option für Memoizee, die verhindert, dass der Cache das Ergebnis von fehlgeschlagenen Anfragen speichert.
    promise: true,
})

const resSubscriptionTypePromise = memoizedAxiosGet(`/test/EmailSubscriptions/${channelInstanceId}`)
const resEmailMailingPromise = memoizedAxiosGet(`test/EmailMailing/${mailingId}`)

requests.push(resSubscriptionTypePromise, resEmailMailingPromise)

try {
    const responses = await Promise.all(requests)
} catch (e) {
    console.error(e)
    throw e
}
```













<br><br>
_________________________________________________
_________________________________________________
<br><br>


# Proxy
- https://www.scrapingbee.com/blog/nodejs-axios-proxy/
```javascript
axios.get('https://api.ipify.org/?format=json',
    {
        proxy: {
            protocol: 'http',
            host: '149.129.239.170',
            port: 8080
        }
    }
)
    .then(res => {
        console.log(res.data)
    }).catch(err => console.error(err))
```







































<br><br>
_________________________________________________
_________________________________________________
<br><br>


# FAQ Error

## [ERR_FR_MAX_BODY_LENGTH_EXCEEDED]: Request body larger than maxBodyLength limit

```javascript
await axios({
    method: 'post',
    url: posturl,
    data: formData,
    maxContentLength: Infinity,
    maxBodyLength: Infinity,
    headers: {'Content-Type': 'multipart/form-data;boundary=' + formData.getBoundary()}
})
```

