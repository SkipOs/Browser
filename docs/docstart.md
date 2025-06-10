# How did it start

Video from Augusto Galego
> [Video from Augusto Galego](https://www.youtube.com/watch?v=fv_B3FTXwxo&t)

Also a great font of info, no mean dudes
> https://stackoverflow.com/questions/598841/how-to-get-started-building-a-web-browser

# Goals 

So, to be clear: 
The main goal of the project is learn how a browser work, what a rendering engine is, and get better at understanding protocols used in the process, and get acquainted with RUST.
Is a long shot, and just learning all of this stuff should be nice. If I manage to write something that can be used for something, the final product will be REALLY simple. So I'll consider this project will be complete when:
- It can access websites (duh) Both by typing the link on the URL space or IP;
- The browser is able to SHOW SOMETHING on the screen. 
- It can read Javascript and understand CSS and HTML.
- Have a history? Being able to click arrows in the UI to go back to pages OR have a dedicated page for a history;
- Being able to resize the window;

# Initial Toughts
There's a lot of stuff involved in understanding how a browser work and then trying to implement one. A lot of research to do, and even more stuff to learn.

After initial research I've broken it down into steps.

# Steps

A Web Browser could be divided into 7 smaller "components" if they could be called that. Is really simple for just click buttons and type addresses, but now, looking a bit deeper, this project seem a bit scary; I don't really was planning on using any libs, so it seem even scarier. I might open some exceptions. Down here we have an overal view of what each part of the browser should, hopefully, do.

The plan is developing each step one-by-one, incrementing the browser like when bulding a puzzle.

## [1. Networking](1-network.md)
- Resolve DNS names.
- Establish TCP connections and perform the said "three‑way handshake".
- Negotiate HTTPS via TLS/SSL (stack overflow told me to study the TLS handshake process and consider using an existing library for cryptography to avoid reinventing the wheel).

## 2. HTTP Client

- Write a simple client that can make HTTP(S) GET requests.
- Download Oor cache the plain HTML recieved.
- This should be a nice entry point for future parsing and sorting, and eventually, rendering the browsers.

## 3. HTML Parser

- Create a parser that can read HTML. Proabbly will start with a small subset of HTML (head, body, then h1, and p and br...)

## 4. DOM Construction

- Build a Document Object Model (DOM) tree for it.
- Make sure that the Browser can diferenciate attributes and they priorities.

## 5. CSS Parsing and Styling

- Implement a basic CSS parser to convert style rules into computed styles.
- Merge these with my DOM tree.
- Build a “render tree” that couples HTML structure with style information.

## 6. Rendering Engine

- Here, I will be screwed. Maybe I start this before the step 4 so I can visualize what I am getting, but who knows.
- Start with simple block and inline layout models.
- Paint the content onto the screen.
- Pray so it works

# 7. Address Javascript Execution
- This should be a project itself.
If I get to the point of making a browser work with css 
AND it don't runs like a rusty machine 
AND is actualy rendering; 
IF I FEEL LIKE IT, I won't be importing a existing library, and will TRY to START doing my own Javascript Interpreter. That's because I've seen other projects being dropped because of this and if I can't, I will limit the browser for static content. 
BUT in the case I do this (or if you is thinking into giving it a go):
- Start understandig how the process of interpreting JavaScript work
- Create a simple lexer and parser to process a subset of JavaScript.
- Nowadays the process is: parsing -> compilation to bytecode -> JIT execution
