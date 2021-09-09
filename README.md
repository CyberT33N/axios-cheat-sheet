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
const FormData = require('form-data')

const path = `${__dirname}/img.png`
const img = await fs.readFile(path, "binary")
const imageBuffer = Buffer.from(img)

// notice that you must always create new form in case you want to use a function/method for future creations
const form = new FormData()
form.append('file', imageBuffer, 'test.png')
const headers = {...form.getHeaders()}

const res = await axios.post(uri, form, {
    headers: headers
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


