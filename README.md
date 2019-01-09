# Secure Digital Payments 2

Please read this entire document before attempting to interact with the Secure Digital Payments API. If this is your first time connecting to an external API, you may wish to begin with the [quick start guide](quickstart/tutorial.md).

# How It Works

The Secure Digital Payments API accepts queries using a two-phase approach: the authentication handshake and then the actual query itself. In either scenario, the system is queried with a version number, an endpoint (which defines the type of information requested) and a payload containing input parameters (sent by POST), and then returns a resultset.

The URL structure for querying the API is as follows:

```
https://api.smartpay.curexe.com/1-1/authenticate
```

"1-1" represents the version number, and "authenticate" represents the endpoint. All versions and endpoints will be documented here.

# Authentication Phase

The API is queried with a key which is obtained from the Integrate page in the Secure Digital Payments web application:

```
{
  api_key: ABC123
}
```

The returned data provides the input parameters which were queried, plus a resultset (in this case a session token), plus any warnings and/or errors:


```
{
  datetime: gmt_time,
  key: ABC123,
  format: json,
  result: [
    token: DEF456,
    expires_on: "2018-01-01 12:00:00"
  ]
  errors: [
    [789, "Incorrect key"]
  ]
  warnings:[
    [012, "Unspecified format"]
  ]
}
```

Note that this result set is solely illustrative; a token would never be provided if there were errors, only warnings.

Tokens will expire within one hour of issue.

# Query Phase

The token provided in the authentication phase is used as an input parameter to authenticate phase two: the actual data query. The result set will look something like this:


```
{
  datetime: gmt_time,
  token: DEF456,
  expires_on: "2018-01-01 12:00:00",
  format: json,
  page: 1,
  total_pages: 14,
  result: [
  ]
  errors: [
  ]
  warnings:[
  ]
}
```

Again, this simple result set is solely illustrative; there is no need for empty key-value pairs.

# Daily Rate Limiting

At this time, resultsets are not rate limited but are monitored for excessive use. It is strongly recommended that systems which connect with the API do so asynchronously wherever possible (i.e. not in user real-time). Users who hammer the server with redundant requests at greater than reasonable frequency will be disabled to prevent performance degredation for other users.

# Testing

To test your integration with the API, create a dummy consumer and attribute orders and invoices against that consumer. All orders and invoices created for a dummy consumer will be ignored by Secure Digital Payments but still remain visible through the Secure Digital Payments website (with a "D" or other indicator denoting that they're for testing only). Dummy entities are automatically deleted within 24-48 hours.

# Versions

- [v1.1](v1-1/overview.md) **Current**
- [v1.0](v1-0/overview.md) Deprecated

# Errors and Warnings

[https://smartpay.curexe.com/api_error_codes](https://smartpay.curexe.com/api_error_codes)
