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