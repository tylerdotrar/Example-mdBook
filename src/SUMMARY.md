# Welcome to the ShittyTest!

This site simplty a test for me to figure out how to get a simple MdBook / Github Pages workflow running.

![Pixel-Skeletor](./attachments/Pasted%20image%20123456789.png)


# Requirements:
1. Create a repository that is set to ``Public``
    - (TBD) Repo name might need to be all lower-case.

2. Markdown files (aka site contents) should be inside of the ``src`` directory.

3. ``src`` directory also needs to include a root ``SUMMARY.md`` file.

4.  Include a simple ``book.toml``

```toml
# Example
[book]
authors = ["Tyler McCann (@tylerdotrar)"]
language = "en"
multilingual = false
src = "src"
title = "Example Site"

[build]
build-dir = "public"

[output.html]
cname="example.hotbox.zip"
```

5. (TBD) Enable Page support in ``Repository --> Settings --> Pages --> Branch``
    - Note sure if this has to be ``/(root)`` or ``/docs``.
    
6. Create mdBook workflow via ``Repository --> Actions --> Pages --> View All --> mdBook --> Configure``

7. (TBD) Default deployment might work, or might need a custom one.
