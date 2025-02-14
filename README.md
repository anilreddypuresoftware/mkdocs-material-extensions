[![Donate via PayPal][donate-image]][donate-link]
[![Build][github-ci-image]][github-ci-link]
[![Coverage Status][codecov-image]][codecov-link]
[![PyPI Version][pypi-image]][pypi-link]
[![PyPI - Python Version][python-image]][pypi-link]
![License][license-image-mit]
# MkDocs Material Extensions

Markdown extension resources for [MkDocs for Material][mkdocs-material]

## Install

Generally, just installing MkDocs Material will automatically install `mkdocs-material-extensions`. But if you had a
need to manually install it, you can use pip.

```
pip install mkdocs-material-extensions
```

But make sure you've also installed MkDocs Material as well as this won't work without it. 

```
pip install mkdocs-material
```

## Inline SVG Icons

MkDocs Material provides numerous icons from Material, FontAwesome, and Octicons, but it does so by inlining the SVG
icons into the source. Currently there is no easy way access these icons and arbitrarily insert them into Markdown
content. Users must include the icon fonts themselves and do it with HTML.

This module allows you to use PyMdown Extensions' [Emoji][emoji] extension to enable easy insertion of MkDocs Material's
SVG assets using simple `:emoji-syntax:`.  This is done by creating our own [emoji index][emoji-index] and
[emoji generator][emoji-generator]. The custom index provides a modified version of the Emoji extensions Twemoji
index.

In addition to the custom index, you must also specify the associated custom generator. This will will find the
appropriate icon and insert it into your Markdown content as an inlined SVG.

Example:

```yaml
markdown_extensions:
  - pymdownx.emoji:
      emoji_index: !!python/name:materialx.emoji.twemoji
      emoji_generator: !!python/name:materialx.emoji.to_svg
```

Then, using the folder structure of Material's `.icons` folder, you can specify icons:

```
We can use Material Icons :material-airplane:.

We can also use Fontawesome Icons :fontawesome-solid-ambulance:.

That's not all, we can also use Octicons :octicons-octoface:.
```

## Using Local Custom Icons

In MkDocs, you can override theme assets locally, and even add assets to the theme. Unfortunately, the Markdown parsing
process isn't aware of the MkDocs environment. Luckily, if you are using PyMdown Extensions 7.1, you can pass in custom
icon paths that will be used when constructing the emoji index and include your custom SVG assets. If a folder path of
`theme/my_icons` was given to the index builder, all icons under `my_project/my_icons`, even in sub-folders, would
become part of the index.

```yaml
markdown_extensions:
  - pymdownx.emoji:
      emoji_index: !!python/name:materialx.emoji.twemoji
      emoji_generator: !!python/name:materialx.emoji.to_svg
      options:
        custom_icons:
          - theme/my_icons
```

If given an icon at `my_project/my_icons/animals/bird.svg`, the icon would be available using the emoji syntax as
`:animals-bird:`. Notice that the base folder that is provided doesn't contribute to the icon's name. Also, folders
are separated with `-`. Folder names and icon names should be compatible with the emoji syntax, so special characters
should be avoided -- `-` and `_` are okay.

You can provide as many paths as you would like, and they will be evaluated in the order that they are specified. The
Material theme's own icons will be evaluated after all custom paths. This allows a user to override Material's icons if
desired.

If an icon name is already in the index, the icon will not be added. It is recommended to always have your icons in
sub-folders to help namespace them to avoid name collisions. In the example above, `bird` was under `animals` which
created the name `:animals-bird:` and helped create a more unique name with less of a chance of creating a duplicate
name with existing emoji and Material icons.

[emoji]: https://facelessuser.github.io/pymdown-extensions/extensions/emoji/
[emoji-index]: https://facelessuser.github.io/pymdown-extensions/extensions/emoji/#custom-emoji-indexes
[emoji-generator]: https://facelessuser.github.io/pymdown-extensions/extensions/emoji/#custom-emoji-generators
[mkdocs-material]: https://github.com/squidfunk/mkdocs-material

[donate-image]: https://img.shields.io/badge/Donate-PayPal-3fabd1?logo=paypal
[donate-link]: https://www.paypal.me/facelessuser
[github-ci-image]: https://github.com/facelessuser/mkdocs-material-extensions/workflows/build/badge.svg?branch=master&event=push
[github-ci-link]: https://github.com/facelessuser/mkdocs-material-extensions/actions?query=workflow%3Abuild+branch%3Amaster
[discord-image]: https://img.shields.io/discord/678289859768745989?logo=discord&logoColor=aaaaaa&color=mediumpurple&labelColor=333333
[discord-link]: https://discord.gg/TWs8Tgr
[codecov-image]: https://img.shields.io/codecov/c/github/facelessuser/mkdocs-material-extensions/master.svg?logo=codecov&logoColor=aaaaaa&labelColor=333333
[codecov-link]: https://codecov.io/github/facelessuser/mkdocs-material-extensions
[pypi-image]: https://img.shields.io/pypi/v/mkdocs-material-extensions.svg?logo=pypi&logoColor=aaaaaa&labelColor=333333
[pypi-link]: https://pypi.python.org/pypi/mkdocs-material-extensions
[python-image]: https://img.shields.io/pypi/pyversions/mkdocs-material-extensions?logo=python&logoColor=aaaaaa&labelColor=333333
[license-image-mit]: https://img.shields.io/badge/license-MIT-blue.svg?labelColor=333333
