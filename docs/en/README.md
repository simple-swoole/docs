# Introduction

Simps is a minimalist PHP coroutine framework based on `Swoole 4.4 +`. Compared with other frameworks, this framework does not have too many packages, only a few simple files.

At the same time, it does not provide common components again, because of the existence of `composer`, a tool for` PHP` dependency management. There are many kinds of ready-made components, we can use `composer` to install and load directly.

## Original intention

The project does not need to use an overweight framework, native Swoole is enough. But how to start when using Swoole directly? This framework was born.

Taking the `Swoole` HTTP server as an example, some users do n’t know how to start when they see the sample code in the document; the` HTTP` server is relatively simple and only needs to pay attention to the request response, so it only needs to listen to an `onRequest` Event. This event is triggered when a new `HTTP` request comes in, but there are still users who do not know how to resolve the route when using it.

This framework provides the function of route resolution, users only need to pay attention to the corresponding business processing when using.

## Origin of the name

`Simple + Swoole = Simps`，Simple `Swoole` framework.