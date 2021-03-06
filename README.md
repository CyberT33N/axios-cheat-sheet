# Axios Cheat Sheet
Axios Cheat Sheet with the most needed stuff..


<br><br>

# Github
- https://github.com/axios/axios














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


## POST with Image
```javascript
const FormData = require('form-data')

// notice that you must always create new form in case you want to use a function/method for future creations
const form = new FormData()

form.append('file', bufferFile, 'test.png')

const headers = {...form.getHeaders()}

const res = await axios.post(uri, form, {
    headers: headers
})

return res.data
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
const axiosRetry = require('axios-retry')
const retriesAmount = 30

module.exports = axios => {
    axiosRetry(axios, {
        retries: retriesAmount, // number of retries

        retryDelay: (retryCount) => {
            console.log(`retry attempt: ${retryCount}`)
            return retryCount * 10000 // time interval between retries
        },

        /* A callback to further control if a request should be retried. By default, it retries if it is a network error or a 5xx error on an idempotent request (GET, HEAD, OPTIONS, PUT or DELETE). */
        retryCondition: (error) => {
            if(!error.response) {
                return true
            } else if(!error.response.status) {
                return true
            }
        }

        /* retry no matter what */
        // retryCondition: (_error) => true
    })
}
```


