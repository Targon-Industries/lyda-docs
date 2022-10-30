Uploading audio is not available via an HTTP endpoint, only through websockets. This enables a more detailed and enhanced communication about certain states.

A typical flow sends the following message types:
- [Authentication](#authentication)
- [Initialize file upload](#initialize-file-upload)
- [File upload](#file-upload)
- [Finish file upload](#finish-file-upload)

# Error responses

## Invalid session token

This might occur when a user does not have access to the track they're trying to access.

```js
{  
    "type": "invalidSessionTokenResponse",
    "message": "Session token is not valid"
}
```

## File upload not initiated

For proper upload handling, a separate [Initialization Request](#initialize-file-upload) must be sent.

```js
{  
    "type": "notInitiatedResponse",
    "message": "Please initiate file upload first"
}
```

## Missing parameter

A required parameter is missing.

```js
{  
    "type": "missingParameterResponse",
    "message": "Parameter " + parameterName + " is not provided"
}
```

## File too large

Returned when a file exceeds 100MB. This is a technical limitation.

```js
{  
    "type": "fileTooLargeResponse",
    "message": "File size exceeds the 100MB limit."
}
```

## Incorrect offset sent

```js
{  
    "type": "incorrectOffsetResponse",
    "message": "Please send the offset received in the last response"
}
```

# Authentication

## Request

**Body**

```js
{
	"type": "authenticationRequest",
	"sessionToken": "your_session_token"
}
```

- `sessionToken` should be taken from the PHP_SESSID cookie. In browsers, this should be easily accessible through JavaScript API.

## Response

**Body**

```js
{  
    "type": "authenticationResponse",
    "success": this.success,
    "message": "Authentication " + (this.success ? "successful" : "failed")
}
```

# Initialize file upload

## Request

**Body**

```js
{
	"type": "initFileUploadRequest",
	"sessionToken": "your_session_token",
	"id": track.id
}
```

- `id` should be the track ID returned by the data endpoint after creating a new track.

## Response

**Body**

```js
{  
    "type": "initFileUploadResponse",
    "success": this.success,
    "message": "File upload " + (this.success ? "initialized" : "failed"),
    "offset": range(0, fileSize)
}
```

# File upload

Instead of sending the full file, it is also possible to send multiple requests with smaller chunks. Doing so adds more overhead on bandwidth, but might be more resilient to network outages or unstable networks because of the option to retry chunks with the `startAt` property returned by [Initialize file upload](#Initialize%20file%20upload).

## Request

```js
{
	"type": "fileUploadRequest",
	"sessionToken": "your_session_token",
	"id": track.id,
	"offset": lastResponse.offset
}
```

## Response

**Body**

```js
{  
    "type": "initFileUploadResponse",
    "success": this.success,
    "message": "Successfully uploaded file chunk"),
    "offset": range(0, fileSize)
}
```

# Finish file upload

