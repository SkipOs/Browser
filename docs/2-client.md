Now I aparenttly understand how the network part of the browser work; So I'm now ready to see how I could do a http client so I can SEE this happening.

1. Understand the HTTP Protocol in Practice
- I already got the conceptual side, now it’s time to see it in action. I've used Postman before so I will probabbly stick with it.
- Learning about HTTP methods (GET, POST, PUT, DELETE...)
- I should explore headers, status codes, and response formats
- Try out real requests using Postman (to test without coding yet)

2. Requests in a High-Level Language
- I will look up Go and Rust, see if I should use a built-in or a documented HTTP library (now that I knwo how TSL works I'm not that excited to do one myself), and then try it.
- Then I'll try to make a small CLI that fetches and print a web page's tilte or JSON content of an API

3. Dive into Headers & Cookies
- Then work into the client so it can manage cookies, custom headers, and maybe even tokens for APIs.
- Learn how to send headers (e.g., Authorization, User-Agent)
- Handle redirects and cookie sessions (sounds like a headache)
- Read and parse response headers (So JSON don't look like sh*t)

4. Also Try HTTPS Manually
- Do a HTTPS request using a raw socket (e.g., in Python, Go, or Node)
- Understanding the certificate part (validate or skip it on purpose)

5. Do the actual prototype of a HTTP Client From Scratch
- Open a TCP connection (socket) manually
- Send a raw HTTP request as a string
- Parse the response headers + body
- Add HTTPS support with TLS socket wrapping
- And package it so it is reusable:
> Wraping it as a class or function set
> Adding config options (headers, method, etc)
> Pray so it works

# HTTP Protocol 
(work still in progress)
