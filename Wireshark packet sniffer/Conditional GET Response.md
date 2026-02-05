# Conditional GET/Response

## 

Browsers use caching to save bandwidth by storing copies of web resources locally. When checking if a cached resource is still valid, they send a conditional GET request with freshness validators like If-Modified-Since (timestamp) or If-None-Match (ETag).

If the resource hasn't changed, the server responds with a 304 Not Modified status, sending only headers (no body), saving bandwidth.
If it has changed, the server responds with a 200 OK status and includes the updated resource in the body.
This mechanism allows browsers to efficiently manage cached content and reduce unnecessary data transfers.
