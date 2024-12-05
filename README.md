# CVE-2022-22965 (Spring4Shell) Blocker

## Firewall Server Handler - 
This project is a proof-of-concept (POC) firewall server designed to detect and block attacks exploiting the CVE-2022-22965 vulnerability, commonly known as Spring4Shell. The server inspects incoming HTTP requests and blocks any that match known malicious patterns associated with this vulnerability.

## Overview

The Firewall Server Handler is a simple Python-based HTTP server, built using the http.server library. It monitors incoming HTTP POST requests, looking for specific headers and payload patterns associated with the Spring4Shell vulnerability. If a request matches these patterns, it is blocked, and the server responds with a 403 Forbidden status.

## Requirements

- Python 3.x

## Setup and Usage

1. Clone the Repository
   ```bash
   git clone [https://github.com/BlackBird63030/Block-Spring4Shell]
   cd Block-Spring4Shell
   ```

2. Run the Server
   ```bash
   python frs.py
   ```
   By default, the server will run on localhost at port 8000.

3. Test the Firewall Rule
   - To test, you can send use the tnt.py script that simulates the attack by doig 5 connections.

## Blocking Rules

This POC uses two main rules to detect and block CVE-2022-22965 exploit attempts:

1. Rule 1: Blocking Payload Pattern
   - Detects requests containing the payload pattern class.module.classLoader.resources.context.parent.pipeline.first, which is used in Spring4Shell exploits to inject malicious Java code.

2. Rule 2: Blocking Suspicious Headers
   - Blocks requests with specific headers characteristic of Spring4Shell attack payloads:
     - suffix: %>//
     - C1: Runtime
     - C2: <%
     - DNT: 1
     - Content-Type: application/x-www-form-urlencoded

If either rule matches, the server responds with a 403 Forbidden status and returns a JSON message:
{"error": "Forbidden Access"}

If no conditions are met, the server responds with 200 OK and:
{"message": "Request received"}

## Code Structure

- ServerHandler: The main class handling HTTP requests.
  - block_request(): Sends a 403 response when a request is blocked.
  - rule_1(): Checks the payload for patterns associated with CVE-2022-22965.
  - rule_2(): Checks for specific headers known to be part of Spring4Shell exploits.
  - do_GET() and do_POST(): Process incoming GET and POST requests, applying the firewall rules.

## Example

To test the firewall, use the tnt.py script. It gonna send 5 POST requests to the firewall script
```bash
python tnt.py
```

The server will respond with:
{"error": "Forbidden Access"}

## Purpose

This POC is intended for educational and testing purposes to demonstrate a basic firewall rule that blocks specific attack vectors targeting the Spring4Shell vulnerability. It is not a substitute for a comprehensive firewall solution in production environments.

## License

MIT License
