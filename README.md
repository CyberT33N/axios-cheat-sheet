# Axios Cheat Sheet
Axios Cheat Sheet with the most needed stuff..


<br><br>

# Github
- https://github.com/axios/axios















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

## GET
```javascript
// await
const response = await axios.get('/user?ID=12345');
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
const axios = require('axios');
const axiosRetry = require('axios-retry');

axiosRetry(axios, {
  retries: 3, // number of retries
  retryDelay: (retryCount) => {
    console.log(`retry attempt: ${retryCount}`);
    return retryCount * 2000; // time interval between retries
  },
  retryCondition: (error) => {
    // if retry condition is not specified, by default idempotent requests are retried
    return error.response.status === 503;
  },
  // retryCondition: (_error) => true // retry no matter what
});

const response = await axios({
  method: 'GET',
  url: 'https://httpstat.us/503',
}).catch((err) => {
  if (err.response.status !== 200) {
    throw new Error(`API call failed with status code: ${err.response.status} after 3 retry attempts`);
  }
});
```


