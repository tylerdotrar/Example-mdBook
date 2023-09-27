# Example-mdBook
Simple repostory made to teach myself (and anyone else) how to use the mdBook Github Pages.

### Requirements:
---

#####  1. Create a repository that is set to ``Public``

#####  2. Markdown files (aka the site contents) should be inside of the ``src`` directory.

##### 3. ``src`` directory also needs to include a root ``SUMMARY.md`` file.
- The ``SUMMARY.md`` file format can be found [here](https://rust-lang.github.io/mdBook/format/summary.html).
- Example:

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

##### 4. Include a simple ``book.toml``
- Example:

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

##### 5. Enable Page support in ``Repository --> Settings --> Pages --> Branch``
- The folder ``/(root)`` works for me.
- The folder ``/docs`` yells at me for not having any ``.css`` files.
    
##### 6. Create mdBook workflow via ``Repository --> Actions --> Pages --> View All --> mdBook --> Configure``
- Default deployment yelled at me, so I'm using a custom ``mdbook.yml``
- Example:

```yml
name: Deploy Github Pages

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v2

      - name: Setup mdBook
        uses: peaceiris/actions-mdbook@v1
        with:
          mdbook-version: '0.4.21'
          # mdbook-version: 'latest'

      - run: mdbook build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```
