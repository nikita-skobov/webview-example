# How

This directory was created as follows:

```sh
cargo new webview-example
cd webview-example
cat >> Cargo.toml <<EOF
web-view = "0.7"

[profile.release]
lto = true
opt-level = 3
EOF
```

Then edit the src/main file to contain the following:

```rs
use web_view::*;

#[cfg(debug_assertions)]
const SHOULD_DEBUG: bool = true;
#[cfg(not(debug_assertions))]
const SHOULD_DEBUG: bool = false;


fn main() {
    // make sure there is an index.html file in the root
    // of the webview-example directory (ie the directory where Cargo.toml is).
    // The easiest way to do this is via a symlink of the dist/index.html file that
    // the frontend outputs. otherwise, you can copy it here each time before you compile.
    #[cfg(not(debug_assertions))]
    let content = {
        let data = include_str!("../index.html");
        Content::Html(data)
    };

    // this is the url where vite serves in dev mode:
    #[cfg(debug_assertions)]
    let content = Content::Url("http://localhost:3000/");

    web_view::builder()
        .title("my title")
        .content(content)
        .size(320, 480)
        .resizable(false)
        .debug(SHOULD_DEBUG)
        .user_data(())
        .invoke_handler(|_webview, _arg| {
            Ok(())
        })
        .run()
        .unwrap();
}

```

Then you can compile in debug mode with: `cargo build` and it will serve the freshest
frontend files because it loads it from localhost, but building for release with `cargo build --release` will inline the index.html file into the executable, and it will load it from the
static string it includes.
