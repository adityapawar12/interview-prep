#### 1.Timeout Functions

- In JavaScript, the setTimeout() function is vey good for adding delays or scheduling the execution of a specific function after a certain period.
- It’s a key feature of both browser environments and Node.js, enabling asynchronous behavior in code execution.

```
const myFunction = (firstParam, secondParam) => {};
const id = setTimeout(myFunction, 2000, firstParam, secondParam);
```

- setTimeout returns the timer id. This is generally not used, but you can store this id, and clear it if you want to delete this scheduled function execution.

```
clearTimeout(id);
```

- The setImmediate function is used to execute a function right after the current event loop finishes.
- In simple terms, the given function is called after all the statements in the script are executed.
- It is the same as calling the setTimeout function with zero delays.
- This is especially useful to avoid blocking the CPU on intensive tasks and
- let other functions be executed while performing a heavy calculation, by queuing functions in the scheduler.

- setInterval is a function similar to setTimeout, with a difference: instead of running the callback function once,
- it will run it forever, at the specific time interval you specify
- unless you tell it to stop, using clearInterval, passing it the interval id that setInterval returned

```
const id = setInterval(() => {
  // runs every 2 seconds
}, 2000);
clearInterval(id);
```

- The setTimeout, setImmediate and setInterval functions can be found in the Timers module of Node.js.\

- Understanding process.nextTick() method: Whenever a new queue of operations is initialized we can think of it as a new tick.
- The process.nextTick() method adds the callback function to the start of the next event queue.
- It is to be noted that, at the start of the program process.nextTick() method is called for the first time before the event loop is processed.
- process.nextTick(callback);

- Understanding setImmdeiate() method: Whenever we call setImmediate() method, it’s callback function is placed in the check phase of the next event queue.
- There is slight detail to be noted here that setImmediate() method is called in the poll phase and it’s callback functions are invoked in the check phase.

```
setImmediate(function A() {
    console.log("1st immediate");
});

setImmediate(function B() {
    console.log("2nd immediate");
});

process.nextTick(function C() {
    console.log("1st process");
});

process.nextTick(function D() {
    console.log("2nd process");
});

// First event queue ends here
console.log("program started");
```

- In the first event queue only ‘program started is printed’.
- Then second event queue is started and function C i.e. callback of process.nextTick() method is placed at the start of the event queue.
- C is executed and the queue ends.
- Then previous event queue ends and third event queue is initialized with callback D.
- Then callback function A of setImmdeiate() method is placed in the followed by B. Now, the third event queue looks like this,

#### 2.Express Streams

- Streams are one of the fundamental concepts of Node.js. Streams are a type of data-handling methods and are used to read or write input into output sequentially.
- Streams are used to handle reading/writing files or exchanging information in an efficient way.
- Streams are objects in Node.js that lets the user read data from a source or write data to a destination in a continuous manner.
- Time Efficient: We don’t have to wait until entire file has been transmitted. We can start processing data as soon as we have it.
- Memory Efficient: We don’t have to load huge amount of data in memory before we start processing.

- types of streams
  a. Writable: We can write data to these streams. Example: fs.createWriteStream().
  b. Readable: We can read data from these streams. Example: fs.createReadStream().
  c. Duplex: Streams that are both, Writable as well as Readable. Example: net.socket.
  d. Transform: Streams that can modify or transform the data as it is written and read. Example: zlib.createDeflate.

##### A. readable stream :

```javascript
// Sample JavaScript Code for creating
// a Readable Stream
// Accessing streams
const { Readable } = require("stream");

// Reading the data
const inStream = new Readable({
  read() {},
});

// Pushing the data to the stream
inStream.push("GeeksForGeeks : ");
inStream.push("A Computer Science portal for Geeks");

// Indicates that no more data is
// left in the stream
inStream.push(null);

// Echoing data to the standard output
inStream.pipe(process.stdout);
```

##### B. Writable stream :

```javascript
// Sample JavaScript Code for
// Writable Stream
// Accessing Streams
const { Writable } = require("stream");

// Whatever is passed in standard
// input is out streamed here.
const outStream = new Writable({
  // The Write function takes three
  // arguments
  // Chunk is for Buffer
  // Encoding is used in case we want
  // to configure the stream differently
  // In this sample code, Encoding is ignored
  // callback is used to indicate
  // successful execution
  write(chunk, encoding, callback) {
    console.log(chunk.toString());
    callback();
  },
});

// Echo the data to the standard output
process.stdin.pipe(outStream);
```

### 15. Is Node.js Single-Threaded or Multi-Threaded?

- Node.js employs a single-threaded model. This means it uses a single thread to handle multiple tasks.
- Traditional server-side applications often use a multi-threaded approach, where multiple threads handle multiple tasks concurrently.

### 16. If Single-Threaded, How Does Node.js Handle Multiple Requests/Concurrency?

- To achieve this, Node.js leverages an event-driven, non-blocking I/O model.

### 17. Event Loop/How It Works

- The event loop is a process that continuously monitors both the call stack and the event queue and checks whether the call stack is empty.
- If the call stack is empty and there are pending events in the event queue, the event loop dequeues the event from the event queue and pushes it to the call stack.
- The call stack executes the event, and any additional events generated during the execution are added to the end of the event queue.

**Note:** The event loop allows Node.js to perform non-blocking I/O operations, even though JavaScript is single-threaded, by offloading operations to the system kernel whenever possible. Since most modern kernels are multi-threaded, they can handle multiple operations executing in the background.
