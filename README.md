# Simps
<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-1-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

[![Simps License](https://img.shields.io/packagist/l/simple-swoole/simps?color=blue)](https://github.com/simple-swoole/simps/blob/master/LICENSE)
[![Latest Version](https://img.shields.io/packagist/v/simple-swoole/simps.svg)](https://packagist.org/packages/simple-swoole/simps)
[![Simps Doc](https://img.shields.io/badge/docs-passing-blue.svg)](https://doc.simps.io)
[![Contact Simps Team](https://img.shields.io/badge/contact-@SimpsTeam-blue.svg?style=flat)](mailto:team@simps.io)
[![Php Version](https://img.shields.io/badge/php-%3E=7.1-brightgreen.svg)](https://www.php.net)
[![Swoole Version](https://img.shields.io/badge/swoole-%3E=4.4.0-brightgreen.svg)](https://github.com/swoole/swoole-src)

Simps 是基于`Swoole 4.4+`实现的极简PHP协程框架，相对于其他框架，本框架没有过多的封装，只有简单的几个文件。

同时也并未再次提供常用的组件，因为有`composer`的存在，`PHP`依赖管理的利器。各种现成的组件很多，我们都可以直接使用`composer`进行安装加载。

## 框架由来

项目不需要使用体量过重的框架，原生`Swoole`足够。但是直接上手使用`Swoole`时不知如何下手？这个框架就由此而生。

以`Swoole`的`HTTP`服务器为例，部分用户在看到文档中的示例代码不知道如何下手；`HTTP`服务器相对比较简单，只需要关注请求响应即可，所以只需要监听一个`onRequest`事件，当有新的`HTTP`请求进入就会触发此事件，但是依然有用户在使用时还不知道如何解析路由。

本框架提供了路由解析的功能，用户使用时只需要关注对应业务处理即可。

## 名称由来

`Simple + Swoole = Simps`，简单的`Swoole`框架。
## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="http://qq52o.me"><img src="https://avatars3.githubusercontent.com/u/33931153?v=4" width="100px;" alt=""/><br /><sub><b>沈唁</b></sub></a><br /><a href="https://github.com/simple-swoole/docs/commits?author=sy-records" title="Documentation">📖</a></td>
  </tr>
</table>

<!-- markdownlint-enable -->
<!-- prettier-ignore-end -->
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!