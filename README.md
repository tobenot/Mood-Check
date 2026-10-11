# Mood-Check

异常情绪自检 SOP。当前 checkout 是静态 HTML / JavaScript PWA，不是含小程序构建链的微信工程。

## 本地入口

没有 npm 依赖清单或构建步骤。在仓库根目录运行一个仅绑定 loopback 的静态服务：

```sh
python3 -m http.server 8000 --bind 127.0.0.1
```

打开 `http://127.0.0.1:8000/`，使用后停止该服务。`index.html`、`sw.js`、`manifest.webmanifest` 与两个 PNG 图标均已跟踪。

## 运行输入与路径

入口另外加载 animate.css 4.1.1 与 bootstrap-icons 1.11.1 的 CDN 样式。这些网络输入不在 service worker 的五个预缓存路径内，源码可恢复不等于首次离线时样式与图标字体完整可用。

The manifest, worker registration and precache paths assume an origin-root deployment. Moving this checkout under a subdirectory requires a coordinated path review, not only copying the HTML.

- manifest 的 `start_url`、`scope` 均为 `/`。
- 页面注册 `/sw.js`，预缓存使用根绝对路径。
- 当前 worker 在激活时删除所有名称不同于 `mood-check-v5` 的同源缓存，不只清退 Mood-Check 自己的旧版本。真实 worker 的内存模拟执行已复现删除其他应用缓存，这是源码层风险，尚未修改。

本次仅验证跟踪输入并以模拟 Cache API 执行 worker 的 install / activate，没有启动浏览器、验证离线安装、触碰真实缓存或访问线上站点。PWA 配置存在不代表安装与离线体验已通过。`CNAME` 是已有站点配置，巡视不会据此授权发布。
