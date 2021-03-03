# The Stackability Website

* Built with [markdown] and [hugo].
* Uses <https://github.com/StaticMania/portio-hugo> portio theme.

## Ermahgerd On-boarding

### Hugo

<img src="https://i.kym-cdn.com/entries/icons/original/000/009/479/Ermahgerd.jpg"
     alt="Ermahgerd, I love docs on how to write the docs!"
     width="200">

It took forever to get `hugo` to just ... show a site.

The "[quick start]" on gohugo.io walks through a golden path that isn't realistic
for other builds.  Because it glosses over what files need to be copied from the
theme to your site.

Here's what I did in one folder above website (the name of this repo), so start
in the same place.

```shell
$ echo $PWD
/Users/tbird/code/stackability
```

Then I did all these [shenanigans] to turn what was in the repo into something
that **GitHub Pages** would build and show for our site.

```shell
cp website/config.toml website/config.toml.bak
rm -rf website/content/ website/docs/ website/resources/ website/themes/ website/archetypes/ website/config.toml
hugo new site website --force # which force didn't work and why I had to rm -rf the above
cd website/
git submodule add git@github.com:StaticMania/portio-hugo.git themes/portio
git submodule init
git submodule update

rm -rf content/ docs/ resources/ archetypes/
cp -rp themes/portio/exampleSite/. . # the "here" of . being in the website folder
hugo server -D # -D includes drafts, `hugo' builds to `publishDir`, 
               # and `hugo server` builds and hosts a localhost site at
               # http://localhost:1313/
               # use `hugo server --help` for all the helps
```

### GitHub Pages

That's the Hugo malarky... Now let's trash talk our free host, GitHub Pages.

These files are needed, from the root path of this repo:

```shell
config.toml
docs/CNAME
index.html
.nojekyll
```

#### `config.toml`

* Change theme to `theme = "portio"`
* Copy the theme specific configs from [`themes/portio/config.toml`] to `config.toml`
* Add the `publishDir = "docs"` in our `config.toml` to match GitHub Pages convention

#### `docs/CNAME`

Maps the custom domain to this repo, looks like this:

```
www.stackability.com
```

#### `index.html`

Can be an empty file, otherwise opening a custom domain shows a GitHub 404 page.

The site is served statically from the `docs` folder.

#### `.nojekyll`

Turns off the default action steps to build and display a Jekyll site.

<img src="https://thumbs.gfycat.com/AnyEnragedJunebug-size_restricted.gif"
     alt="I don't want no Jekyll, a Jekyll is a default site generator for GitHub Pages.">

### DNS

[Custom domains] require the following to be setup with DNS.

* An `A` record with 4 IP values
    * 185.199.108.153
    * 185.199.109.153
    * 185.199.110.153
    * 185.199.111.153
* A `CNAME` record pointing to `<organization>.github.io`, as in: (`stackability.github.io`)

<img src="static/images/dns-records.png"
     alt="A records and CNAME records">

[markdown]:                    https://daringfireball.net/projects/markdown/
[hugo]:                        https://gohugo.io/
[shenanigans]:                 https://www.youtube.com/watch?v=xdXo8uJ9NSk
[quick start]:                 https://gohugo.io/getting-started/quick-start/
[Custom domains]:              https://docs.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site
[`themes/portio/config.toml`]: themes/portio/exampleSite/config.toml