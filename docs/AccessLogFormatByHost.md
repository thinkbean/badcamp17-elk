# Access Log Format By Host

Access log formats are generally similar, but not identical. There
may be small differences between web servers (Apache vs NGINX) or
different hosting providers may inject additional non-standard but helpful data.

Below is the data provided, in order, for access files generated on
Platform.sh, Pantheon, and Acquia.

## Platform.sh Access Log Format

- IP: The IP address detected by the server
- User: Basic user auth
- Timestamp
- Verb: HTTP verb. i.e. GET, POST, etc.
- Request: The part of the url after the domain
- HTTP version
- Response: HTTP response code. i.e. 200, 404, etc.
- Bytes
- Referrer
- User Agent

You can read more about Platform.sh logs [here](https://docs.platform.sh/development/logs.html).

## Pantheon Access Log Format

- IP: The IP address detected by the server
- User: Basic user auth
- Timestamp
- Verb: HTTP verb. i.e. GET, POST, etc.
- Request: The part of the url after the domain
- HTTP version
- Response: HTTP response code. i.e. 200, 404, etc.
- Bytes
- Referrer
- User Agent
- Request time: The time it took the request to process
- Forwarded for: The IP address of the end user in case request was forwarded

You can read more about Pantheon logs [here](https://pantheon.io/docs/logs/#available-logs).


## Acquia Access Log Format

- IP: The IP address detected by the server
- User: Basic user auth
- Timestamp
- Verb: HTTP verb. i.e. GET, POST, etc.
- Request: The part of the url after the domain
- HTTP version
- Response: HTTP response code. i.e. 200, 404, etc.
- Bytes
- Referrer
- User Agent
- Virtual Host: Internal server hostname
- Host: The domain name
- Hosting Site:
- pid
- Request time:  The time it took the request to process
- Forwarded for: The IP address of the end user in case request was forwarded
- Request id: An ID of the request which can be used to relate entries in other logs (php, php error)
- SSL Protocol
- SSL Cypher

You can read more about Acquia access logs [here](https://docs.acquia.com/acquia-cloud/monitor/logs/balancer-access).