# HTTP status codes

The status-code element is a 3-digit integer code giving the result of the attempt to understand and satisfy the request.

## Classes

The first digit of the status-code defines the class of response. The last two digits do not have any categorization role. There are 5 values for the first digit:

- `1xx` Information
- `2xx` Successful
- `3xx` Redirection
- `4xx` Client error
- `5xx` Server error

## Common

### 1xx

|  Code | Reason              | Description                                                                                                                                                                  |
| ----: | :------------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `1xx` | **Informational**   | Interim response for communicating connection status or request progress prior to completing the requested action and sending a final response.                              |
| `100` | Continue            | Initial part of a request has been received and has not yet been rejected by the server.                                                                                     |
| `101` | Switching Protocols | Server understands and is willing to comply with the client's request, via the Upgrade header field, for a change in the application protocol being used on this connection. |

### 2xx

|  Code | Reason                        | Description                                                                                                                                                                                                                      |
| ----: | :---------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `2xx` | **Successful**                | Client's request was successfully received, understood, and accepted.                                                                                                                                                            |
| `200` | OK                            | Request has succeeded.                                                                                                                                                                                                           |
| `201` | Created                       | Request has been fulfilled and has resulted in one or more new resources being created.                                                                                                                                          |
| `202` | Accepted                      | Request has been accepted for processing, but the processing has not been completed.                                                                                                                                             |
| `203` | Non-Authoritative Information | Request was successful but the enclosed payload has been modified from that of the origin server's 200 (OK) response by a transforming proxy.                                                                                    |
| `204` | No Content                    | Server has successfully fulfilled the request and that there is no additional content to send in the response payload body.                                                                                                      |
| `205` | Reset Content                 | Server has fulfilled the request and desires that the user agent reset the document view, which caused the request to be sent, to its original state as received from the origin server.                                         |
| `206` | Partial Content               | Server is successfully fulfilling a range request for the target resource by transferring one or more parts of the selected representation that correspond to the satisfiable ranges found in the requests's Range header field. |

### 3xx

|  Code | Reason             | Description                                                                                                                                                                                                                                                                                   |
| ----: | :----------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `3xx` | **Redirection**    | Further action needs to be taken by the user agent in order to fulfill the request.                                                                                                                                                                                                           |
| `300` | Multiple Choices   | Target resource has more than one representation, each with its own more specific identifier, and information about the alternatives is being provided so that the user (or user agent) can select a preferred representation by redirecting its request to one or more of those identifiers. |
| `301` | Moved Permanently  | Target resource has been assigned a new permanent URI and any future references to this resource ought to use one of the enclosed URIs                                                                                                                                                        |
| `302` | Found              | Target resource resides temporarily under a different URI.                                                                                                                                                                                                                                    |
| `303` | See Other          | Server is redirecting the user agent to a different resource, as indicated by a URI in the Location header field, that is intended to provide an indirect response to the original request.                                                                                                   |
| `304` | Not Modified       | Conditional GET request has been received and would have resulted in a 200 (OK) response if it were not for the fact that the condition has evaluated to false.                                                                                                                               |
| `305` | Use Proxy          | _deprecated_                                                                                                                                                                                                                                                                                  |
| `306` |                    | _unused_                                                                                                                                                                                                                                                                                      |
| `307` | Temporary Redirect | Target resource resides temporarily under a different URI and the user agent MUST NOT change the request method if it performs an automatic redirection to that URI.                                                                                                                          |

### 4xx

|  Code | Reason                        | Description                                                                                                                                                                                                                                      |
| ----: | :---------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `4xx` | **Client Error**              | Client seems to have erred.                                                                                                                                                                                                                      |
| `400` | Bad Request                   | Server cannot or will not process the request because the received syntax is invalid, nonsensical, or exceeds some limitation on what the server is willing to process.                                                                          |
| `401` | Unauthorized                  | Request has not been applied because it lacks valid authentication credentials for the target resource.                                                                                                                                          |
| `402` | Payment Required              | _reserved_                                                                                                                                                                                                                                       |
| `403` | Forbidden                     | Server understood the request but refuses to authorize it.                                                                                                                                                                                       |
| `404` | Not Found                     | Origin server did not find a current representation for the target resource or is not willing to disclose that one exists.                                                                                                                       |
| `405` | Method Not Allowed            | indicates that the method specified in the request-line is known by the origin server but not supported by the target resource.                                                                                                                  |
| `406` | Not Acceptable                | Target resource does not have a current representation that would be acceptable to the user agent, according to the proactive negotiation header fields received in the request, and the server is unwilling to supply a default representation. |
| `407` | Proxy Authentication Required | is similar to 401 (Unauthorized), but Client needs to authenticate itself in order to use a proxy.                                                                                                                                               |
| `408` | Request Timeout               | Server did not receive a complete request message within the time that it was prepared to wait.                                                                                                                                                  |
| `409` | Conflict                      | Request could not be completed due to a conflict with the current state of the resource.                                                                                                                                                         |
| `410` | Gone                          | indicates that access to the target resource is no longer available at the origin server and that this condition is likely to be permanent.                                                                                                      |
| `411` | Length Required               | Server refuses to accept the request without a defined Content-Length.                                                                                                                                                                           |
| `412` | Precondition Failed           | indicates that one or more preconditions given in the request header fields evaluated to false when tested on the server.                                                                                                                        |
| `413` | Payload Too Large             | Server is refusing to process a request because the request payload is larger than the server is willing or able to process.                                                                                                                     |
| `414` | URI Too Long                  | Server is refusing to service the request because the request-target is longer than the server is willing to interpret.                                                                                                                          |
| `415` | Unsupported Media Type        | Origin server is refusing to service the request because the payload is in a format not supported by the target resource for this method.                                                                                                        |
| `416` | Range Not Satisfiable         | None of the ranges in the request's Range header field overlap the current extent of the selected resource or that the set of ranges requested has been rejected due to invalid ranges or an excessive request of small or overlapping ranges.   |
| `417` | Expectation Failed            | Expectation given in the request's Expect header field could not be met by at least one of the inbound servers.                                                                                                                                  |
| `418` | I'm a teapot                  | Any attempt to brew coffee with a teapot should result in the error code 418 I'm a teapot.                                                                                                                                                       |
| `426` | Upgrade Required              | Server refuses to perform the request using the current protocol but might be willing to do so after the client upgrades to a different protocol.                                                                                                |

### 5xx

|  Code | Reason                     | Description                                                                                                                                                 |
| ----: | :------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `5xx` | **Server Error**           | Server is aware that it has erred or is incapable of performing the requested method.                                                                       |
| `500` | Internal Server Error      | Server encountered an unexpected condition that prevented it from fulfilling the request.                                                                   |
| `501` | Not Implemented            | Server does not support the functionality required to fulfill the request.                                                                                  |
| `502` | Bad Gateway                | Server, while acting as a gateway or proxy, received an invalid response from an inbound server it accessed while attempting to fulfill the request.        |
| `503` | Service Unavailable        | Server is currently unable to handle the request due to a temporary overload or scheduled maintenance, which will likely be alleviated after some delay.    |
| `504` | Gateway Time-out           | Server, while acting as a gateway or proxy, did not receive a timely response from an upstream server it needed to access in order to complete the request. |
| `505` | HTTP Version Not Supported | Server does not support, or refuses to support, the protocol version that was used in the request message.                                                  |

## Extensions

|  Code | Reason                          | Description                                                                                                                                                                                                                                                                                 |
| ----: | :------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `102` | Processing                      | Interim response used to inform the client that the server has accepted the complete request, but has not yet completed it.                                                                                                                                                                 |
| `207` | Multi-Status                    | provides status for multiple independent operations.                                                                                                                                                                                                                                        |
| `226` | IM Used                         | The server has fulfilled a GET request for the resource, and the response is a representation of the result of one or more instance-manipulations applied to the current instance.                                                                                                          |
| `308` | Permanent Redirect              | The target resource has been assigned a new permanent URI and any future references to this resource outght to use one of the enclosed URIs. [...] This status code is similar to 301 Moved Permanently                                                                                     |
| `422` | Unprocessable Entity            | Server understands the content type of the request entity (hence a 415(Unsupported Media Type) status code is inappropriate), and the syntax of the request entity is correct (thus a 400 (Bad Request) status code is inappropriate) but was unable to process the contained instructions. |
| `423` | Locked                          | Source or destination resource of a method is locked.                                                                                                                                                                                                                                       |
| `424` | Failed Dependency               | Method could not be performed on the resource because the requested action depended on another action and that action failed.                                                                                                                                                               |
| `428` | Precondition Required           | Origin server requires the request to be conditional.                                                                                                                                                                                                                                       |
| `429` | Too Many Requests               | User has sent too many requests in a given amount of time (rate limiting).                                                                                                                                                                                                                  |
| `431` | Request Header Fields Too Large | Server is unwilling to process the request because its header fields are too large.                                                                                                                                                                                                         |
| `451` | Unavailable For Legal Reasons   | This status code Server is denying access to the resource in response to a legal demand.                                                                                                                                                                                                    |
| `506` | Variant Also Negotiates         | Server has an internal configuration error: the chosen variant resource is configured to engage in transparent content negotiation itself, and is therefore not a proper end point in the negotiation process.                                                                              |
| `507` | Insufficient Storage            | Method could not be performed on the resource because the server is unable to store the representation needed to successfully complete the request.                                                                                                                                         |
| `511` | Network Authentication Required | Client needs to authenticate to gain network access.                                                                                                                                                                                                                                        |
| `7xx` | **Developer Error**             | Developer error.                                                                                                                                                                                                                                                                            |

For a full up-to-date list, continue reading on [HTTP Status Code Registry](https://www.iana.org/assignments/http-status-codes/http-status-codes.xml), [Wikipedia](http://en.wikipedia.org/wiki/List_of_HTTP_status_codes).
