# Errors

The Discify API will return a errors object for any error.
It uses the following error codes:

> Example Errors Response:

```json
{
  "errors": "This is the message for why your request failed"
}
```

Error Code | Meaning
---------- | -------
404 | Not Found -- The specified resource could not be found.
422 | Unprocessable Entity -- Your request is invalid.
