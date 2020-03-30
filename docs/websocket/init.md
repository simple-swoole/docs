# WebSocket

## 启动

```bash
php bin/simps.php ws:start
```

## 对应事件

本框架默认存在`onStart`和`onWorkerStart`事件，如需增加其他事件请参考[事件处理-未定义事件](/listens?id=未定义事件)，如需修改已存在事件请参考[事件处理-已存在事件](/listens?id=已存在事件)

!> WebSocket可用事件参考[Swoole官方文档](https://wiki.swoole.com/#/websocket_server?id=%e4%ba%8b%e4%bb%b6)