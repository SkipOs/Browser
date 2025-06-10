# Networking
There some stuff that will be Adressed here. They will be divided in this order:
- 1. Resolve DNS names.
- 2. Establish TCP connections and perform the said "three‑way handshake".
- 3. Negotiate HTTPS via TLS/SSL.

First of all, I want to make sure I understand what every single one of those things mean. And then, I'll tackle how to build something that do everything here.

# Resolve DNS names

First of all, what even is a DNS?
Well [Cloudfare](https://www.cloudflare.com/learning/dns/what-is-dns/) and [Datadog](https://www.datadoghq.com/knowledge-center/dns-resolution/) helped me on this one, they should help you too.
First of all let's talk about how you can access a website.
Starting with the most simple thing ever, let's say you want to open duckduckgo.com. You go to the address bar, type it and voilá.

But, behind the scenes what is happening is something a bit more complex:

## First we make a Requisition

This can be done by typing the desired address in IPV4 or by it's hostname into the address bar.
If you type the hostname, then the browser checks if the machine have the IP address somewhere into it's cache. If it has, great, if not, crap here we go down the rabbit hole.

If this is the case, the web browser start a chain of processes by sending a "message", a DNS query to a bigger server. This guy is the DNS Resolver.

## Behind the scenes

Usually the DNS Resolver is provided by your Internet Service Provider, this Resolver (Let's call it Server A) is the one who will get your query to get the IP address of the "duckduckgo.com" domain and send it to something called a DNS root nameserver (the Server B).

Then, the Server B returns to Server A the address of a Top Level Domain DNS server, or a TLD (let's call it C).

This Server C, can be seen as a list with the name of all domains of some kind. It can be a .com .net . org and among many others. In the case of "duckduckgo.com" the Server C should be where .com domains are stored.

The Resolver (Server A) then make a requisition to the .com TLD (Server C) so he can get the IP of where is stored "duckduckgo.com".

The .com TLD (Server C) then answer with the said IP to this, wich points to an address into Server D, where "duckduckgo.com" is stored "phisically".

The Resolver (Server A) then sends a query with the address, in this case "duckduckgo.com" to the server where it is stored (Server D)

The Server D, then returns the IP address of the domain "duckduckgo.com" to the Resolver (Server A)
Lastly, the Resolver (Server A) send back the IP address of the domain to the Web Browser.

So what is hapenning here is:
1. Web Browser send a request to Server A
2. Server A send a request to Server B, wich tells it to search into Server C
3. Server A send a request to Server C, wich tells it to search into Server D
4. Server A send a request to Server D, wich tells it the domain's IP address
5. Server A tells the Web Browser the domain's IP address

After this, the domain can be stored in cache or somenthing alike so it can be easily accessed later on.

Then, after all of this, now knowing the IP that he should comunicate with, the browser can start the process to make the next two bigger steps.

# Establish TCP connections and perform the said "three‑way handshake".

Yeah, here I used [Stack Overflow](https://stackoverflow.com/questions/8156254/tcp-vs-udp-what-is-a-tcp-connection), [Datatracker](https://datatracker.ietf.org/doc/html/rfc7414) and a bunch of other sites to understand some of this stuff. (You should also give them a read or just do some googling around if you didn't understand something.)
- https://medium.com/@ajinkyawakhale3/how-does-tcp-connection-work-a29c1f6f5488
- https://www.coursera.org/articles/three-way-handshake
- https://www.sciencedirect.com/topics/computer-science/three-way-handshake
- https://developer.mozilla.org/en-US/docs/Glossary/TCP_handshake

I know how TCP works and what it is for. Right?

## TCP

Transmission Control Protocol or TCP for the homies, basically ensures that the data you send and receive, and its integrity, is OK.

This not only safeguards everyone that the packages are fully sent or received, but that they are unchanged by anyone who could be out there listening stuff. Another big thing about TCP is that it is connection-based, and that's why we need the IP address the DNS Resolver gave us.
That's because we need both an IP Address of our own, and a port, to receive the data we want, and an IP Address of the place where we want to retrieve the data from.

TCP connections can be identified by a pair of sockets, which are made of a host and a port. So a TCP connection could be identified by (host 1, port x) (host 2, port y)

But how does TCP work? What does it do? How?

Right, this is the part I kind of suffered to get while graduating.

First, this is done by a method called Three-Way Handshake.

In the Web Browser (Let's call it the Client), what should happen is the following: First, the Client will ask for a connection to be established with the desired IP Address (the one we got from our DNS Resolver).

This connection will be sent with a SYN flag, which asks for a connection.

This will be sent with an initial sequence number.

The Server in that IP Address will then receive this and then send back another message, with its own TCP segment, with both the flags SYN and ACK turned on, and another sequence number, the server will be then wait for an answer from the Client.

Upon receiving, the client will send back another TCP, one with only the ACK flag and the sequence number received from the Server, while also will be checking if the SYN is correct.

Then, after receiving the ACK from the client, if everything went well, both sides are synchronised and ready to excange data. This my friend is what we call the connection.

So, dum it up:
1. Client send a TCP with a SYN flag and a number to checksum to Server
2. Server send a TCP with a SYN flag, an ACK flag, and a number to checksum to Client
3. Client send a TCP with a ACK flag and a number to checksum to Server
4. Both are now in sync and ready to exchange data

Seems simple enought from a logical prespective. But until now we didn't recive any bit of the page we want, and we will need to convince (in the next step) the server to give us that data. THIS is the part I don't know how- yet.

# Negotiate HTTPS via TLS/SSL.

As always, my fonts (and complementary stuff you should read:

- [Cloudfare Article - What happens in a TLS handshake? | SSL handshake](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/)
- [Medium Article - Negotiation of TLS Parameters for HTTPS Encryption](https://medium.com/geekculture/negotiation-of-tls-parameters-for-https-encryption-1217833b3ffa)
- [Stack Exchange Forum - Is it possible to hand-negotiate an SSL/TLS session?](https://crypto.stackexchange.com/questions/19256/is-it-possible-to-hand-negotiate-an-ssl-tls-session)
- [SSL.com Article - SSL/TLS Handshake: Ensuring Secure Online Interactions](https://www.ssl.com/article/ssl-tls-handshake-ensuring-secure-online-interactions/)
- [What’s the Difference Between HTTP and HTTPS?](https://aws.amazon.com/pt/compare/the-difference-between-https-and-http/)

## What are we doing here?

Well HTTP (Hypertext Transfer Protocol) and HTTPS (Hypertext Transfer Protocol Secure) are both used for transferring data over the internet, but HTTPS is the secure version of HTTP.

HTTP works by a requesting logic. Usually a GET method, and the server then answers with something. HTTPS does the same thing, but add a new step by encrypting the data transmission to prevent eavesdropping and ensure data integrity. Without that "S" all that is transmitted is in plain text.

For example someone who intercepts an HTTP connection can see something like:
```json
{"user":"username","password":"password"}
```

While intercepting a HTTPS connection could give you something like:
```txt
Sajssdm1!@4kasdm!@$455as}{rasÇ3as4r{asvaserqwrqqwqrWQERasdf12312414
```
(Let's say that's how something that's encrypted look like)

Great. How?

By using ANOTHER "Handshake". TLS or Transport Layer Security, before referred to as SSL (Secure Sockets Layer), handles that extra step of making sure both sides (client and server) are talking securely.

The goal of this handshake is to:
agree on the encryption protocols
verify the server’s (and optionally client’s) identity
exchange keys that will be used to encrypt the session

This process happens before any actual HTTP request is sent. So, even the GET /whatever is protected under that encryption once it's all set.

The simplified flow looks like this:

## 1. Client Hello

The client says "Hey, I wanna talk securely" and sends a list of supported TLS versions, cipher suites (aka encryption algorithms), and some random values to help later in generating keys.

## 2. Server Hello

The server replies with the TLS version, chosen cipher suite, and its own random value. It also sends its digital certificate (usually signed by a CA - Certificate Authority) so the client can check if it’s talking to the right person.

## 3. Certificate Verification + Key Exchange

The client validates that certificate and starts key exchange using asymmetric crypto (usually with public keys). This is where the shared session key (used for symmetric encryption later) is generated.

## 4. Session Key Agreement

Both client and server now have enough info to derive the exact same session key, but no one watching from the outside can do it (unless they're a magician or a omniscient hacker lol).

## 5. Finished

A "Finished" message is sent (encrypted using that new session key), and then the other side answers back the same way. If both messages decrypt correctly, we're good to go.

From this point on, everything is encrypted using the symmetric session key. It’s fast and safe.

SO, Let’s say you type https://example.com/login in your browser.

Before even getting the login page:

1. Your browser sends a Client Hello
2. The server answers with its certificate
3. They negotiate stuff, create a session key
4. Then the login page is fetched via HTTPS with encryption in place

If someone intercepts the packets at that point, all they see is encrypted gibberish, not your login or session info.
