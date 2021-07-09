# webview-example
> a template for making rust webview apps using vite as frontend and convenient dev mode vs release mode settings.

the `webview-example/` directory contains the rust code which creates a webview.
The webview content will either be served from localhost (if compiled in debug mode), or from a static inlined file index.html (if compiled in release mode).

If compiled in release mode, the `webview-example/` directory assumes that there is a file: `webview-example/index.html` present, and it will
try to inline this file. This file does not exist in this repository because it must first be generated from the frontend code:

The frontend code is in `singlepage-frontend-example/`. It can be served in debug mode (hot reload, quick compile times) via `npm run dev`, in which case the
rust code can load it via `http://localhost:3000/`, or otherwise it can be compiled into a single html file via `npm run build-single`. This html file
is now in `singlepage-frontend-example/dist/index.html` and if you want it to be statically included in the rust executable, it needs to be copied (or symlinked)
to `webview-example/index.html`.
