# Welcome to the Example mdBook!

This site is a test for me to figure out how to get a simple mdBook Github Pages workflow running.


![Pixel-Skeletor](../attachments/Pasted%20image%20123456789.png)
- **Note:** attachments should be URL encoded.


# Requirements:
1. Create a repository that is set to ``Public``

2. Markdown files (aka site contents) should be inside of the ``src`` directory.

3. ``src`` directory also needs to include a root ``SUMMARY.md`` file.
    - The ``SUMMARY.md`` file format can be found [here](https://rust-lang.github.io/mdBook/format/summary.html).
```markdown
# Example mdBook

- [Table of Contents](./SUMMARY.md)
- [Primary Directory](Primary%20Directory/README.md)
  - [mdBook Github Pages Checklist](Primary%20Directory/mdBook_GithubPages_Checklist.md)
- [Secondary Directory](Secondary%20Directory/README.md)
  - [Obsidian Markdown Test](Secondary%20Directory/Obsidian_Markdown_Test.md)
- [Tertiary Directory](Tertiary%20Directory/README.md)
  - [Export-Obsidian.ps1](Tertiary%20Directory/Export-Obsidian.md)
```

4.  Include a simple ``book.toml``

```toml
[book]
authors = ["Tyler McCann (@tylerdotrar)"]
language = "en"
multilingual = false
src = "src"
title = "Example mdBook"

[build]
build-dir = "public"

[output.html]
cname="example.hotbox.zip"
```

5. Enable Page support in ``Repository --> Settings --> Pages --> Branch``
    - The folder ``/(root)`` works for me.
    - The folder ``/docs`` yells at me for not having any ``.css`` files.
    
6. Create mdBook workflow via ``Repository --> Actions --> Pages --> View All --> mdBook --> Configure``

7. Default deployment should work, or might need a custom one.
