# Welcome to the Example mdBook!

- This site is an example for how to configure a really simple mdBook Github Pages workflow (utilizing custom domain names).

- The following notes are a manual walkthrough of setting up the site **WITHOUT** using a local instance of ``mdbook``.
	- Steps 2 through 4 are normally done automatically by running ``mdbook init``.
	- Using a custom domain name is **NOT** a requirement for setting up a successful Github Page, it is simply included in this repository for the sake of completion and verbosity.

- Link to the root Github repository [here](https://github.com/tylerdotrar/Example-mdBook).


![Pixel-Skeletor](../attachments/Pasted%20image%20123456789.png)
- **Note:** attachments don't need to be completely URL encoded, however spaces do need to be replaced with ``%20`` within the file path.

## Creating your Site
---

#### 1. Create a repository that is set to ``Public``


#### 2. Create a "``src``" directory.


#### 3. Place all Markdown files (aka the site contents) into the ``src`` directory.
- The root ``.md`` file should be ``SUMMARY.md``.
  - Formatting documentation can be found [here](https://rust-lang.github.io/mdBook/format/summary.html).
- Example ``SUMMARY.md``:

```markdown
# Summary

# Primary Section
- [mdBook Github Page Creation](Primary%20Directory/mdBook_GithubPages_Creation.md)

# Secondary Section
- [Obsidian Markdown Comparison](Secondary%20Directory/Obsidian_Markdown_Comparison.md)

# Tertiary Section
- [Export-Obsidian.ps1](Tertiary%20Directory/Export-Obsidian.md)
```


#### 4. Include a simple ``book.toml``
- Your custom domain name should be included.
- Simply remove the ``[output.html]`` section to avoid custom domain configuration.
  - If no domain name is specified, Github Pages will opt for: ``https://<username>.github.io/<repository>``
- Example ``book.toml``:

```toml
[book]
authors = ["Tyler McCann (@tylerdotrar)"]
language = "en"
multilingual = false
src = "src"
title = "Example mdBook Site"

[build]
build-dir = "public"

[output.html]
cname="example.hotbox.zip"
```


#### 5. Enable 'Read & Write Permissions' for Workflows using the GITHUB_TOKEN.
- ``Repository --> Settings --> Actions --> General --> Workflow Permissions``

![Allow GITHUB_TOKEN](../attachments/Setting_Workflow_Perms.png)


#### 6. Create mdBook Workflow (``mdbook.yml``)
- ``Repository --> Actions --> Pages --> View All --> mdBook --> Configure``
- The default deployment yelled at me, so I opted for a simpler, custom ``mdbook.yml``.
  - You should be able to copy and paste this example file verbatim.
- Example ``mdbook.yml``:

```yml
name: Deploy mdBook Github Pages

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

#### 7. Set your Github Page deployment to the 'gh-pages' branch.
- The 'gh-pages' branch will be created by the ``mdbook.yml`` workflow (assuming no errors occur).
- Once it is created, you can set that branch as your deployment branch.

![Github Page Deployment](../attachments/Setting_Page_Deployment.png)

#### 8. Create a CNAME record to point your custom domain to the Github Pages site.
- Documentation on configuring subdomains with Github Pages can be found [here](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain-and-the-www-subdomain-variant).
- This step will vary for everyone, so below is my experience with Cloudflare.

![Cloudflare CNAME](../attachments/Cloudflare_CNAME.png)


#### 9. Add your target Domain to your Repository settings.
- ``Repository --> Settings --> Pages --> Custom Domain``
- Once your CNAME finishes propegating, your mdBook should now be accessible.

![Custom Domain](../attachments/Setting_Custom_Domain.png)


#### 10. Flex on your peers.

