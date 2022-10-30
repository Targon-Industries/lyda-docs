Uploading audio is not available via an HTTP endpoint, only through websockets. This enables a more detailed and enhanced communication about certain states.

A typical flow sends the following message types:
- authenticationRequest (see #Authentication )
- initFileUploadRequest
- fileUploadRequest (multiple)
- finishFileUploadRequest

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

