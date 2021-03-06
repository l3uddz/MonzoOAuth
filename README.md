# MonzoOAuth
This is an OAuth client wrapper for quickly working with the monzo api.
 
## Usage
Create an instance of the MonzoOAuth client.
```python
from MonzoOAuth import MonzoOAuth
...
monzo = MonzoOAuth(
    client_id=client_id,
    client_secret=client_secret,
    redirect_uri='http://example.com/auth',
    credentials=monzo_credentials if monzo_credentials else None
)
```
The `monzo_credentials` is a variable used to store the authenticated credentials returned when logged in.

Redirect the user to the URL provided by `get_auth_link()`
```python
redirect_url = monzo.get_auth_link()
```

At the `/auth` url, store the code variable form the url query, in this example we will call this `code`.
Now we swap this code for an access_token using 
```python
monzo_credentials = monzo.exchange_code(code)
```

Storing the credentials against a users session will allow you to recreate the MonzoOAuth client when needed.

You can check if the monzo session is authorized by using the `authorized()` function, this returns a boolean.

If this returns `False`, it means that you need to add credentials or authorize the user again, you can add credentials with `set_credentials(credentials)`

If this returns `True`, it doesn't necessarily mean you are good to fire commands, you might still need to re authenticate depending on how much time has passed.

## Running queries
[Monzo API](https://monzo.com/docs/)
There is a handy function that you simply pass the path and any url query options to and it will return a response for you.
```
path = 'balance'
options = {
  'account_id': 'ACCOUNT_ID'
}
response = monzo.query(path=path, options=options)
```
