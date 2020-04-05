# Chapter 1: Debugging

Debugging is a critical part in software engineering. Ideally, debugging is done
on local environment. But any software engineers know that sentences starting
with "Ideally," are often labeled as "famous last words".

In this chapter, we will cover the principles of Node.js debugging. We will
consider a "running application" that can be either running locally or remotely
on a server.

After learning how to attach the debug tools to this application, we will use
these tools to troubleshoot memory or CPU related issues. During this chapter,
we will also highlight several tips in term of coding practices that can be
used to eventually make the debug sessions easier (or at least, less painful).

In the first version of this chapter, we will not cover the details of the
Chrome Debug protocol and the use of the `inspector` core module. Introducing
these is kept for the next iterations.
