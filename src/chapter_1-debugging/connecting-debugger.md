# Connecting the Chrome Developer Tools to a Node.js process

One of the most useful tool to debug a Node.js process is bundled with Google
Chrome: The Chrome Developer Tools (or CDT).

Connecting the CDT to a Node.js instance can be pretty straightforward in the
case of a locally running process. It might require a bit more of tuning to
connect them to a remote instance of Node.js.

## Placing a Node.js instance in debug mode

### If the instance is not running yet

The `--inspect` flag has been introduced to Node.js 6.3.0. Starting Node.js with
this flag will start a websocket server for the Chrome Developer Tools to
connect to:

```bash
$ node --inspect file.js
Debugger listening on ws://127.0.0.1:9229/f9257636-7dc0-4b78-a308-8f8b5e8f66da
For help, see: https://nodejs.org/en/docs/inspector
```

By default, the websocket server will be opened on `127.0.0.1:9229`.
If you want to connect to expose it on another interface or another port,
you will need to use the following syntax:

```bash
node --inspect=<host>:<port> index.js
```

### If the instance is already running

If it is not possible to restart the Node.js instance with the wanted flag,
it is still possible to enable the debug mode on it.

This is done by sending a signal to the Node.js process.
`SIGUSR1` is reserved by Node.js. Sending this signal to the process will result
in enabling the inspect mode on the given Node.js process.

On Mac OS and Linux, sending this signal can be done with the following command:

```bash
kill -usr1 <PID>
```

Where `PID` is the PID of the Node.js process.
Identifying the PID of a running process can be done using the `ps` command.
The following command usually works pretty well:

```bash
ps aux | grep node
```

It will list the current process started with the `node` executable.

```text
vdeturckheim 33666 0.1 0.1 4595640 16016 s005 S+ 1:44PM 0:00.37 node --inspect server.js
```

In this example, the PID of the process is `33666`.

When the `SIGUSR1` signal is received by the Node.js instance, the following
will be added to the console of the process:

```text
Debugger listening on ws://127.0.0.1:9229/f9257636-7dc0-4b78-a308-8f8b5e8f66da
For help, see: https://nodejs.org/en/docs/inspector
```

## Connecting the Chrome Developer Tools to the instance

Opening Google Chrome to the URL chrome://inspect should display the following
page:
![chrome://inspect whith one Node.js instance available](
./assets/images/local_debug.png)
The list of currently debuggable Node.js instances is available under the
"Target" title. Clicking on the "inspect" link will open the debug tools
for an instance.  

### In case of a remote instance, use port forwarding

In the case you are trying to connect to a remote instance of Node.js, it is
possible that the port used by Node.js to listen to debugging instructions is
not available to the outside world. Rather than opening this port on the server,
I recommend using port forwarding. A quick online search should tell you the
way to do this in your situation.

## Exercise 1: Start a Node.js process in debug mode and connect the DevTools

Install the [sample application](https://github.com/vdeturckheim/debuggable-app)
locally and connect to the Chrome DevTools by starting the process with the
`--inspect` flag.

## Exercise 2: Connect the DevTools to a remote Heroku instance

Deploy the [sample application](https://github.com/vdeturckheim/debuggable-app)
on Heroku and connect to the Chrome DevTools by enabling the debug mode on the
already running Node.js process. You will need to forward the port 9229 from the
Heroku server to your computer.
