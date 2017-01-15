---
layout: post
title: Atom plugins and configs
date: 2016-07-08 16:58:51.000000000 -04:00
type: post
published: true
status: publish
categories: []
tags: []
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '24596769897'
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

Plugins
- Minimap
Show a minimap.
- file-icons
Show nice file-icons in the file-tree.
- [merge-conflicts](https://atom.io/packages/merge-conflicts)
Show git merge-conflicts in a more intuitive way.
- keyboard-localization
If you have a non-english keyboard.
- react
Show react-syntax.
- pigments
- [multi-cursor-plus](https://atom.io/packages/multi-cursor-plus)
This is a great plugin. For the times you want to use multiple cursors but now exactly below each other. It requires a copy-paste configuration.
- [color-picker](https://atom.io/packages/color-picker)
Makes it a bit easier to select colors for your css
- linter
To automatically check the code.
- linter-jshint
For linter to work on JS
Here is a list of other languages [languages](https://atomlinter.github.io/)
- linter-markdown
- atom-terminal
this plugin let's you run ctr-alt-t in any file, and a terminal will open up in that directory
Themes
atom-material-ui
Syntax
atom-material-syntax
This can be done fast with the atom package manager

```
apm install minimap file-icons merge-conflicts keyboard-localization react pigments multi-cursor-plus color-picker linter linter-jshint atom-material-syntax atom-material-ui atom-terminal

```
Paste in this in your keymap.json file to get the comment function to work correctly.

```
'atom-workspace atom-text-editor:not([mini])':
  'ctrl-7': 'editor:toggle-line-comments'
```
