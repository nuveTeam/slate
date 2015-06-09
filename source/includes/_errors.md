# Errors

The Nuve API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Malformed request
401 | Unauthorized -- The provided API key is incorrect
404 | Not Found -- The specified resource could not be found
405 | Method Not Allowed -- A resource was access with an invalid method
406 | Not Acceptable -- Only JSON is supported by the API
422 | Unprocessable Entity -- The content of the request was invalid
500 | Internal Server Error -- There was an interal server error. Try again later
503 | Service Unavailable -- Server is temporarily unavailable
