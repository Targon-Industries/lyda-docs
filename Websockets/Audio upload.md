Uploading audio is not available via an HTTP endpoint, only through websockets. This enables a more detailed and enhanced communication about certain states.

A typical flow sends the following message types:
- [Authentication](#Authentication)
- [Initialize file upload](#Initialize%20file%20upload)
- [File upload](#File%20upload)
- finishFileUploadRequest

# Generic error responses

## Invalid session token

This might occur when a user does not have access to the track they're trying to access.

```js
{  
    type: request.type.replace("Request", "Response"),
    success: false,
    message: 'Session token is not valid'
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
    "message": 'Authentication ' + (this.success ? 'successful' : 'failed')
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
    "message": 'File upload ' + (this.success ? 'initialized' : 'failed')
}
```

# File upload

Instead of sending the full file, it is also possible to send multiple requests with smaller chunks. Doing so adds more overhead on bandwidth, but might be more resilient to network outages or unstable networks because of the option to retry chunks.

