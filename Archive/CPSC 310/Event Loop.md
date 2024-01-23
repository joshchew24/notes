# Event Loop

- code executed on call stack
- async calls to API (for browser, they're WebAPI, for node, they're C++ APIs, threading done in C++)
- API calls resolve and place callback task in task queue
- when call stack is empty, event loop places first dequeued callback onto stack