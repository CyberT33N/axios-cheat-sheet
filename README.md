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
const form = new FormData()

form.append('file', readableFileStream, {
    filename: 'test.png'
})

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

