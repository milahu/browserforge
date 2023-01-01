# browserforge

html files are inplace-editable documents like [dokieli](https://github.com/linkeddata/dokieli)
which can be shared over peer-to-peer networking (wifi, bluetooth, sneakernet)

everything runs in the browser

## status

concept

### implementations

* https://github.com/tinacms/tinacms - content management system with visual editing (inplace editing)
* https://github.com/linkeddata/dokieli - client side editor, but i want "dokieli with git backend"
* https://github.com/solid/specification - federated web hosting, but i want "solid pods with git backend"
* https://github.com/isomorphic-git/isomorphic-git#who-is-using-isomorphic-git
  * https://github.com/mizchi/next-editor - https://next-editor.app/ - 300 stars - last update 2020
  * https://github.com/wmhilton/nde - https://wmhilton.com/nde/ - https://nde.now.sh/ - broken - 80 stars - last update 2018
  * https://github.com/wmhilton/git-apps - https://git-app-manager.now.sh/ - broken - 0 stars - last update 2017

## specs

### inplace-editing

document is a html page
by default, it is read-only (not editable)
when i click "edit" then load the editor
= make the document editable \*in place\* = no page reload

### offline first

aka: local-first, dweb, nocloud, nobackend

https://github.com/topics/offline-first
https://github.com/topics/local-first
https://github.com/topics/dweb
https://github.com/topics/dapp

because ...

[There is no cloud, it’s just someone else’s computer](https://www.chriswatterston.com/article/success-of-my-there-is-no-cloud-sticker)

![There is no cloud, it’s just someone else’s computer](there-is-no-cloud.jpg)

#### workflow

like on github: fork, branch, edit, commit, merge

the user's "first impression" is the rendered html file,
because documents are "read often" or "mostly read"

when the user wants to make a change,
the "editing experience" should be instant,
so the user does not forget his idea.

to edit the page, click the "edit this page" button, and edit.
the details of the edit process (fork, branch, edit, commit, merge) come later.
but the "first edit" is persistent across page reloads, so dont worry about data loss.

the user can fork before editing, but its not a hard requirement,
to make editing as "instant" and "inplace" as possible.

when the first edit is done, its time to fork, branch, commit.

fork: download the full repo to the browser (filesystem api or indexeddb).
this must be independent of the original URL, so we must install as browser app.

> window.showOpenFilePicker()

<details>
<summary>
filesystem api
</summary>

https://web.dev/file-system-access/

The entry point to the File System Access API is window.showOpenFilePicker().
When called, it shows a file picker dialog box, and prompts the user to select a file.

We have developed a library called [browser-fs-access](https://github.com/GoogleChromeLabs/browser-fs-access)
that uses the File System Access API wherever possible
and that falls back to these next best options in all other cases.

Opening a directory and enumerating its contents

To enumerate all files in a directory, call showDirectoryPicker(). The user selects a directory in a picker, after which a FileSystemDirectoryHandle is returned, which lets you enumerate and access the directory's files. By default, you will have read access to the files in the directory, but **if you need write access, you can pass { mode: 'readwrite' } to the method**.

</details>

now the app can work offline: browser other pages, edit pages, branch/commit/rebase

on future page loads, the app will ask "do you want to fetch updates?"

updates are merged with local changes.
use either the simple conflict-resolution of git,
or a the complex conflict-resolution of CRDT (yjs, automerge, ...).
the problem with CRDT is, it needs lots of memory to store ALL the changes (character-based diff)

also handle schema migrations

##### self-modifying code

code and data - both can be modified with this system,
so it can be called "self-modifying code" (in a good sense, a "good virus")
more precise a "self-modifying browser-app"

https://www.i-programmer.info/programming/javascript/989-javascript-jems-self-modifying-code.html

#### pazguille/offline-first

https://github.com/pazguille/offline-first
Everything you need to know to create offline-first web apps

### html first

javascript is optional
no build steps, no SSR, no markdown

"html is the mother language" -- rich harris, svelte

markdown can be derived from html

browser first, native second https://jasonette.com/

#### qwik framework

https://github.com/BuilderIO/qwik - 14K stars

The HTML-first framework. Instant apps of any size with ~ 1kb JS

https://qwik.builder.io/playground/

qwik playground source: https://github.com/BuilderIO/qwik/tree/main/packages/docs/src/repl

ui framework should be "SSR by default" like qwik
to produce static html pages, which run without javascript.
optional javascript dependencies are lazy-loaded (on demand) to reduce memory footprint of the app

##### builder.io

https://github.com/BuilderIO/builder - 4K stars

https://builder.io/demo

Drag and drop Visual CMS for React, Vue, Angular, and more

Integrate with any site or app. Drag and drop with the components already in your codebase.

see also

- https://github.com/primodotso/primo - https://primo.so/ - 500 stars - javascript - visual CMS, based on: svelte, prosemirror. Primo is a component-based CMS that makes it easy to build visually-editable static sites

#### htmlgoddess

https://github.com/jonascript/htmlgoddess

A static site generator to code like its 1999

no javascript, only html + css

#### html as data exchange format

html is useful as a data exchange format
between mobile devices

data flow:

- click on "export app" -> this will download all data to a single html file
- this html file is sent to the other device
- on the other device, open the html file
- this will install the app to the other device

html template:

```html
<html>
<body>

<script>
console.time("parse html");
</script>

<div>hello ...</div>

<pre style="display:none" id="packfile">
UEFDSwAAAAIAABQFkw14nJXLQQrDIBBA0b2nmH2hjHU0DpTSZTc9hI5jE4hJCOb+TY/Q5fvw+64K
EaUKsi9RBsbgfYxUQrmJkji1pNlFSTWbLe26dFCnjvKtMglqKox+COjQWqrM2eZK7BAxm3T0cd3h
Pc1pgddPcG8nxuP5aWmar7K2B9gQ2CIjE1zQIpqztql3/f80x2a+AzI+TZ4NeJyNy0EKwjAQheF9
TpG9VGaStM6AiEs3HmKaTGzBtKWk9zeCB3D5P95Xd1XrUxzJkyoTSgJOpOQ1955i1qgQcuaRM5tN
...
20 MByte of base64 data
...
</pre>

<div>... world</div>

<script>
console.timeEnd("parse html");

console.time("get innerHTML");
var packfile = document.querySelector("#packfile").innerHTML
console.timeEnd("get innerHTML");

console.time("decode base64");
packfile = atob(packfile);
console.timeEnd("decode base64");
</script>
</body>
</html>
```

benchmark in chrome on a cheap laptop:

```
parse html: 769.47216796875 ms
get innerHTML: 865.382080078125 ms
decode base64: 1356.555908203125 ms
```

the base64 data is generated by

```sh
git gc
# now there is only 1 pack file
cat .git/objects/pack/*.pack | base64 >>pack.html 
```

## based on

this is a fusion of multiple tools

### editable html files

- https://github.com/linkeddata/dokieli
  - backend: https://solidproject.org/
- https://github.com/LivelyKernel/LivelyKernel - browser-based runtime and development environment that makes creation of (Web) applications much more immediate and direct. All development happens "live", i.e. you change your application and the system while it is running
- IPFS-based
  - This single-page web app can edit itself
    - https://medium.com/textileio/this-single-page-web-app-can-edit-itself-62734dac2700
      - https://laptrinhx.com/this-single-page-web-app-can-edit-itself-667783868/
    - https://github.com/ipfs/js-ipfs
    - https://github.com/carsonfarmer/profile-app
    - https://github.com/textileio/dapp-template
- https://www.pagecloud.com/blog/how-to-edit-your-website
  - `document.body.contentEditable = 'true'; document.designMode='on';`

### web desktop

aka: web based operating system

https://github.com/syxanash/awesome-web-desktops

#### svelte

https://github.com/puruvj/macos-web - 2K stars - macOS Web written in Svelte

#### react

https://github.com/blueedgetechno/win11React - 7K stars - Windows 11 in React

https://github.com/DustinBrett/daedalOS - 6K stars - Desktop environment in the browser. 

https://github.com/Renovamen/playground-macos - 3K stars - My portfolio website simulating macOS's GUI, developed with React and UnoCSS.

https://github.com/vivek9patel/vivek9patel.github.io - 3K stars - Personal portfolio website of theme Ubuntu 20.04, made using NEXT.js & tailwind CSS

https://github.com/piyushsuthar/windows-11-web - 500 stars

#### vue

https://github.com/GoodManWEN/GoodManWEN.github.io - 1K stars - A website simulating linux system's GUI, using theme of Deepin distro.

https://github.com/DonChiaQE/win95 - 100 stars - Windows 95 Portfolio built with Vue.js

#### other

https://github.com/os-js/OS.js - 6K stars - OS.js - JavaScript Web Desktop Platform. based on Hyperapp, an "ultra-lightweight **Virtual DOM**, highly-optimized diff algorithm, and state management library" (Virtual DOM is a bug ...)

https://github.com/dahliaOS/pangolin_desktop - 2K stars - Pangolin Desktop UI shell, designed for dahliaOS, written in Flutter.

https://github.com/KilledByAPixel/OS13k - 500 stars - A Tiny OS and Mini Game Engine - 13 KByte

https://github.com/Manthee1/linuxWeb - 100 stars - Linux... but simulated on your web browser.

### web unix

- https://github.com/plasma-umass/browsix - https://browsix.org/ - 3K stars - Browsix is a Unix-like operating system for the browser.
- https://news.ycombinator.com/item?id=13151566
  - https://github.com/runtimejs/runtime - https://github.com/runtimejs-comm/runtime - operating system (unikernel) for the cloud
  - https://bellard.org/jslinux/ - Run Linux or other Operating Systems in your browser!

https://news.ycombinator.com/item?id=13153258

> Browsix is not an interpreter. Browsix provides a shared kernel and primitives like system calls to _existing_ interpreters and runtimes that target JavaScript.
>
> For example, we extended both the GopherJS and Emscripten compilers and runtimes to talk to our shared kernel, so that processes written in C and Go can run in parallel (on separate Web Workers) and communicate over pipes, sockets and the filesystem in the browser (much like they can in a standard Unix environment).

https://news.ycombinator.com/item?id=13159090

> JSLinux is a tremendous feat of engineering.
>
> If you want to run realistic programs in the browser (and not just technology demos), WebAssembly, asm.js, and compilation to JavaScript in general is the way to go. WebAssembly can easily execute within a factor of 2 of optimized GCC binaries, whereas JSLinux is ~80x slower.
>
> Paired with compilers like Emscripten and GopherJS Browsix has the potential to be a relatively fast and lightweight solution to running legacy code in the browser in a way that integrates with existing tools for web UIs.

#### wasm

- https://rreverser.com/webassembly-shell-with-a-real-filesystem-access-in-a-browser/
- https://github.com/jprendes/emception - Run Emscripten in the browser

#### terminal emulators

- https://github.com/xtermjs/xterm.js - 14K stars
  - https://github.com/wavesoft/local-echo - 100 stars - A local-echo controller for xterm.js
- https://github.com/sdllc/cmjs-shell - 5 stars (underrated) - Command shell based on CodeMirror. javascript repl. syntax highlighting. all text (input + output) is rendered in CodeMirror

<details>

- https://github.com/Gottox/terminal.js - 500 stars
- https://github.com/os-js/osjs-xterm-application - xterm app for os.js, client side: xterm.js. server side: powershell.exe or bash
- https://www.npmjs.com/package/xterm-browser
- https://www.npmjs.com/package/seeelaye
- https://www.npmjs.com/package/ttty
- https://github.com/chjj/term.js - 1K stars
- https://github.com/finseth/bash - terminal emulator. no commands

</details>

#### bash interpreters

- https://github.com/nfischer/shelljs-transpiler - 70 stars - transpile Bash to ShellJS
  - https://github.com/shelljs/shelljs - 14K stars - target is node.js not browser. implements some coreutils, but not bash
- https://github.com/trybash/bash-emulator - lightweight, only plain text, no ansi escapes

##### parallel processing

aka: task scheduling, threading, multiprocess, coroutines

- https://github.com/miketalbot/js-coroutines - 500 stars (underrated) - Javascript Coroutines for idle processing and animation.  based on requestIdleCallback
- https://github.com/raksoras/koroutine - 40 stars - coroutine scheduler for node.js based on ES6 generators
- https://github.com/bmeck/generator-runner - 20 stars

<details>

- https://github.com/plasma-umass/browsix - https://browsix.org/ - 3K stars - Browsix is a Unix-like operating system for the browser.
- https://github.com/bashojs/basho - 120 stars - Lazy shell pipelines in plain JavaScript
- https://github.com/dthree/cash - 8K stars - Cross-platform Linux commands in ES6. only for node.js
  - https://news.ycombinator.com/item?id=11334597
- https://github.com/UltiRequiem/coreutils - 30 stars - deno - Cross-platform Deno rewrite of the GNU Coreutils
- https://github.com/rethab/deno-bash - A silly little bash interpreter written in TypeScript for Deno.
- https://github.com/taokann/WeBash
- https://github.com/lpan/rebash - react
- https://github.com/mullaney/bash - minimal, some commands, fs async api
- https://github.com/austinoboyle/bash - react
- https://github.com/hamaxx/jsbash - Basic bash interpreter in javascript. pipes
- https://github.com/stencila/basha - Bash interpreter for executable documents. deprecated for https://github.com/takluyver/bash_kernel in python and https://github.com/stencila/stencila
- https://github.com/krithravi/bash-prompt-interpreter - Interprets the values of Bash prompts. Submitted to Sunhacks 2020!

not

- https://github.com/davidecaruso/shell.js - 80 stars - read only. not interactive
- https://github.com/importpw/vercel-bash - Bash Runtime for Vercel serverless functions. remote exec of bash scripts

#### bash parsers

- https://github.com/codemirror/legacy-modes/blob/main/mode/shell.js

</details>

### web framework

https://krausest.github.io/js-framework-benchmark/

### web package

https://github.com/WICG/webpackage

specifications aimed at packaging websites

allow people to bundle together the resources that make up a website, so they can be shared offline, either with or without a proof that they came from the original website

### component manager

https://github.com/teambit/bit

toolchain for component-driven software. Forget monolithic apps and distribute development to components

motivation: package management https://bit.dev/blog/painless-monorepo-dependency-management-with-bit-l4f9fzyw/

### interface designer

https://www.builder.io/

Drag and drop on your tech stack

### TiddlyWiki

https://github.com/Jermolene/TiddlyWiki5 - 7K stars

A self-contained JavaScript wiki for the browser, Node.js, AWS Lambda etc.

https://tiddlywiki.com/

https://en.wikipedia.org/wiki/TiddlyWiki

> TiddlyWiki is a personal wiki and a non-linear notebook for organising and sharing complex information. It is an open-source single page application wiki in the form of a **single HTML file** that includes CSS, JavaScript, embedded files such as images, and the text content. It is designed to be easy to customize and re-shape depending on application. It facilitates re-use of content by dividing it into small pieces called Tiddlers.
>
> TiddlyWiki is an unusual example of a practical quine. This idea of producing a copy of its own source code that lies at the heart of TiddlyWiki's ability to independently save changes to itself.

storage: single HTML file

#### tiddly-gittly/TidGi-Desktop

https://github.com/tiddly-gittly/TidGi-Desktop - 800 stars

TidGi is an privatcy-in-mind, automated, auto-git-backup, freely-deployed Tiddlywiki knowledge management Desktop note app, with local REST API

##### git-sync-js

https://github.com/tiddly-gittly/git-sync-js

JS implementation for Git-Sync

### dokieli

https://github.com/linkeddata/dokieli

inplace-editable html pages

git backend is missing

### html editor

aka: rich text editor, wysiwyg editor

ideally, the user can install different editors

#### pell

https://github.com/jaredreich/pell - 12K stars

html editor with only 4 KByte

to compare:

- https://bundlephobia.com/package/prosemirror-view@1.29.1 has 140 KByte
- https://bundlephobia.com/package/draft-js@0.11.7 has 220 KByte
- https://bundlephobia.com/package/monaco-editor@0.34.1 has 80 KByte

#### prosemirror

https://github.com/ProseMirror/prosemirror-view - 1K stars (underrated)
rich-text editor

see also:
https://tiptap.dev/guide/collaborative-editing
https://tiptap.dev/hocuspocus/
https://github.com/pubpub/pubpub-editor - based on react, prosemirror - 100 stars
https://github.com/chanzuckerberg/czi-prosemirror - based on react, prosemirror - 300 stars
https://github.com/primodotso/primo - https://primo.so/ - 500 stars - javascript - visual CMS, based on: svelte, prosemirror. Primo is a component-based CMS that makes it easy to build visually-editable static sites

#### grapesjs

https://github.com/artf/grapesjs - 17K stars

Web Builder Framework. Build templates without coding

drag-and-drop, no-code, gui, visual-programming

#### stackedit.io

https://stackedit.io/

https://github.com/benweet/stackedit - 20K stars

markdown editor

browser app

backends: github, gitlab, google drive, dropbox

alternatives

- https://github.com/nhn/tui.editor - 15K stars
- https://github.com/joemccann/dillinger - 8K stars

#### trix

https://github.com/basecamp/trix - 17K stars

#### nota

https://github.com/nota-lang/nota - 500 stars - A document language for the browser. looks like Markdown and Latex

#### inplace editors

https://github.com/xreader/inplaceeditor - 40 stars - Edit static HTML pages with simple InPlaceEditor

https://github.com/ZenCocoon/inplacericheditor - 20 stars - AJAX In Place Rich Editor: In place AJAX-powered WYSIWYG editor

https://github.com/doomhz/jQuery-Inplace-Edit - 10 stars - A jQuery plugin for editing text in place.

https://github.com/wbotelhos/inplace - 10 stars - An inplace editor

https://github.com/ovesco/flyter - 5 stars - A library for inline editing. inspired by x-editable (create editable elements), but doesn't rely on jquery, offers tons of customization options and can be easily extended to fit your needs.

https://github.com/vitalets/x-editable - 7K stars - In-place editing. create editable elements. includes both popup and inline modes. [demo](https://vitalets.github.io/x-editable/demo-bs3.html)

<blockquote>

Different By Design

Most WYSIWYG editors are wrappers around HTML’s contenteditable and execCommand APIs, designed by Microsoft to support live editing of web pages in Internet Explorer 5.5, and eventually reverse-engineered and copied by other browsers.

Because these APIs were never fully specified or documented, and because WYSIWYG HTML editors are enormous in scope, each browser’s implementation has its own set of bugs and quirks, and JavaScript developers are left to resolve the inconsistencies.

Trix sidesteps these inconsistencies by treating contenteditable as an I/O device: when input makes its way to the editor, Trix converts that input into an editing operation on its internal document model, then re-renders that document back into the editor. This gives Trix complete control over what happens after every keystroke, and avoids the need to use execCommand at all.

</blockquote>

todo: what would prosemirror do?

<details>

#### dillinger.io

https://dillinger.io/

https://github.com/joemccann/dillinger - 8K stars

markdown editor

backends: github, google drive, dropbox, one drive, medium

#### sofish/pen

https://github.com/sofish/pen - 5K stars

html editor, inplace editing, live editing

#### woofmark

https://github.com/bevacqua/woofmark - 2K stars

markdown editor

#### litewrite

https://litewrite.net/

https://github.com/litewrite/litewrite - 400 stars

html editor

> Offline: Once loaded, it’s essentially a desktop app. Thanks to AppCache and localStorage, both app and data are fully cached offline and synced whenever online.

> This is an [unhosted web app](http://unhosted.org/), meaning its code is fully client-side, without any server backend you need to trust! It also supports the open [remotestorage](http://remotestorage.io/) protocol so you can sync your data across devices & browsers.

via http://unhosted.org/apps/

#### hyperdraft

https://hyperdraft.rosano.ca/

html editor

### static web hosting

github.com pages, gitlab.com pages, codeberg.org pages ...

</details>

### git forges

#### gitea

https://github.com/go-gitea/gitea - 30K stars

web frontend for git repos, alternative to github, gitlab

works with isomorphic-git

selfhosted, localfirst, nocloud, anyone can start a new instance

### cloud IDE

aka: browser IDE, browser playground

https://alternativeto.net/category/developer-tools/ide/?license=opensource

https://www.sitepoint.com/code-playgrounds/

#### CodeSandbox

An online IDE for rapid web development

https://codesandbox.io/

https://github.com/codesandbox/codesandbox-client - 12K stars

<blockquote>

CodeSandbox is less of a playground and more of an online development environment.

Like standard web projects, you can add any number of files and edit them using a multi-tab, VS Code-like integrated development environment (aka IDE). It’s free to sign up using a GitHub or Google account, but you can then collaborate with others in real time, export projects to a Git repository, and deploy to static site hosts such as Netlify and Vercel.

CodeSandbox could be a practical option if you’re working remotely or using a non-typical development device such as a Chromebook.

</blockquote>

##### bundler

https://github.com/codesandbox/sandpack - 3K stars

webpack for the browser

https://sandpack.codesandbox.io/docs/advanced-usage/client

alternative: https://github.com/divriots/browser-vite - 500 stars

##### github backend

https://github.com/codesandbox/codesandbox-importers/blob/master/packages/git-extractor/src/routes/github/pull/download.ts

##### Full Offline Support

https://codesandbox.io/post/creating-a-parallel-offline-extensible-browser-based-bundler-for-codesandbox

<blockquote>
  
Full Offline Support

Everything works offline already, but for full offline support we need to allow you to save sandboxes offline. This allows you to work offline on your projects forever, and upload to CodeSandbox whenever you’d like. The only feature that requires an internet connection are npm dependencies, but we already cache all npm results. We’ll give you the option to precache combinations for when you’re planning a trip or flight.

</blockquote>

##### alternatives

https://github.com/styfle/awesome-online-ide

- https://github.com/jsbin/jsbin - 4K stars
- https://github.com/chinchang/web-maker - 2K stars
- https://github.com/porsager/flems - 400 stars
- https://github.com/popcodeorg/popcode - 200 stars

##### online playgrounds

aka: repl (read eval print loop)

- https://github.com/pngwn/REPLicant
- https://github.com/novacbn/svelte-pipeline
- https://playground.solidjs.com/

#### code-server

https://github.com/coder/code-server - 60K stars

VS Code in the browser

https://github.com/gitpod-io/openvscode-server

https://github.com/conwnet/github1s - 20K stars - One second to read GitHub code with VS Code.

#### Eclipse Theia

https://theia-ide.org/

https://github.com/eclipse-theia/theia - 18K stars

Cloud & Desktop IDE

compatible with vscode extensions

#### Eclipse Che

https://www.eclipse.org/che/

middleware for a remote IDE, browser based, similar to vscode server

with support for multiple frontends: vscode, jetbrains, Eclipse Theia

#### web-maker

https://github.com/chinchang/web-maker - 2K stars

> A blazing fast & offline frontend playground
>
> Web-Maker is an offline playground for your web experiments. Something like CodePen or JSFiddle, but much more faster and works offline because it runs completely on your system.

export to single html file

<details>

#### Atheos

https://github.com/Atheos/Atheos - 300 stars

https://github.com/Codiad/Codiad - 3K stars

PHP backend

> A self-hosted browser-based cloud IDE, updated from Codiad IDE
>
> Atheos is an updated and currently maintained fork of Codiad, a web-based IDE framework with a small footprint and minimal requirements.

#### gitpod

https://github.com/gitpod-io/gitpod - 10K stars

> Gitpod automates the provisioning of ready-to-code development environments.
>
> Gitpod is an open-source Kubernetes application for ready-to-code cloud development environments that spins up fresh, automated dev environments for each task, in the cloud, in seconds. It enables you to describe your dev environment as code and start instant, remote and cloud development environments directly from your browser or your Desktop IDE.

#### codeanywhere

https://codeanywhere.com/

closed source

#### koding

https://github.com/koding/koding

archived in year 2019

#### htmlhouse

https://github.com/writeas/htmlhouse

html server + editor

</details>

### cloud compiler

<details>

#### ideone

https://ideone.com/

online compilers

#### tryitonline

https://github.com/TryItOnline/tryitonline

online interpreters for an evergrowing list of practical and recreational programming languages

#### godbolt

https://godbolt.org/

online compilers

</details>

### browser editors

#### codimd

CodiMD - Realtime collaborative markdown notes on all platforms.

https://github.com/hackmdio/codimd - 8K stars

markdown editor with github backend

<details>

#### flems

https://github.com/porsager/flems

html editor

</details>

### git storage backend

#### isomorphic-git

https://github.com/isomorphic-git/isomorphic-git - 7K stars
https://www.npmjs.com/package/isomorphic-git - 200K downloads
A pure JavaScript implementation of git for node and browsers

does not work with github, because github refuses to add CORS headers (fuck them)
works with gitea &rarr; example: https://try.gitea.io/milahu/alchi

overkill?
we just need to fetch like 10 versions of \*one file\*
&rarr; github rest api should work

##### @isomorphic-git/lightning-fs

https://www.npmjs.com/package/@isomorphic-git/lightning-fs

https://github.com/isomorphic-git/lightning-fs - 400 stars

filesystem based on IndexedDB

alternative to

- https://github.com/jvilk/BrowserFS - 3K stars
- https://github.com/filerjs/filer - 500 stars

##### github-activity-writer

https://github.com/orta/github-activity-writer

##### gatsby-source-git

https://github.com/stevetweeddale/gatsby-source-git - 70 stars

git client for gatsby

##### gridsome-source-git

https://github.com/noxify/gridsome-source-git - 10 stars

git client for gridsome

https://github.com/gridsome/gridsome - 9K stars - The Jamstack framework for Vue.js

##### WeGit

https://github.com/welldan97/WeGit - 20 stars

Distributed P2P Git Hosting Provider Network

#### js-git

https://github.com/creationix/js-git - 4K stars
https://www.npmjs.com/package/js-git - 1M downloads
A JavaScript implementation of Git

> I've been asking Github to enable CORS headers to their HTTPS git servers, but they've refused to do it. This means that a browser can never clone from github because the browser will disallow XHR requests to the domain.

##### js-github

https://github.com/creationix/js-github - 160 stars

A js-git mixin that uses github as the data storage backend.

##### git-indexeddb

https://github.com/aaronpowell/git-indexeddb - 10 stars

js-git db on top of IndexedDB

example with indexeddb caching

##### git-browser

https://github.com/creationix/git-browser

based on js-git, git-indexeddb, ...

#### git-master

https://github.com/ineo6/git-master - 400 stars

Git Master Extension for git file tree, support GitHub GitLab Gitee Gitea Gogs

browser extension

#### git-documentdb

https://github.com/sosuisen/git-documentdb - 40 stars

Offline-first Database that Syncs with Git

#### workbox

https://github.com/GoogleChrome/workbox
JavaScript libraries for Progressive Web Apps
service workers

#### berty

https://github.com/berty/berty
peer-to-peer messaging app that works with or without internet access

#### UpUp

https://github.com/TalAter/UpUp
create sites that work offline as well as online

#### vuejs-templates/pwa

https://github.com/vuejs-templates/pwa
PWA template for vue-cli based on the webpack template

#### client-side-databases

https://github.com/pubkey/client-side-databases
An implementation of the exact same app in Firestore, AWS Datastore, PouchDB, RxDB and WatermelonDB

#### pouchdb

https://github.com/pouchdb/pouchdb - 15K stars

> PouchDB is an open-source JavaScript database inspired by [Apache CouchDB](http://couchdb.apache.org/) that is designed to run well within the browser.
> 
> PouchDB was created to help web developers build applications that work as well offline as they do online.
> https://pouchdb.com/

#### couchdb

https://couchdb.apache.org/

> [The Couch Replication Protocol](https://docs.couchdb.org/en/stable/replication/protocol.html) lets your data flow seamlessly between server clusters to mobile phones and web browsers, enabling a compelling [offline-first](https://offlinefirst.org/) user-experience while maintaining high performance and strong reliability.

#### kinto

https://github.com/Kinto/kinto.js

https://docs.kinto-storage.org/en/stable/faq.html#comparison

Kinto | Parse Server | Firebase | CouchDB | Kuzzle | Remote-Storage | Hoodie | BrowserFS
-- | -- | -- | -- | -- | -- | -- | --

#### wora

https://github.com/morrys/wora
Write Once, Render Anywhere.
documentation for typescript libraries: cache-persist, apollo-offline, relay-offline, offline-first, apollo-cache, relay-store, netinfo, detect-network

#### git-documentdb

https://github.com/sosuisen/git-documentdb
Offline-first Database that Syncs with Git

demo app
https://github.com/sosuisen/inventory-manager

#### tool-db

https://github.com/Manwe-777/tool-db
A peer-to-peer decentralized database

<details>

#### cube.js

https://github.com/cube-js/cube.js

> Headless Business Intelligence for Building Data Applications
>
> Cube is the headless business intelligence platform. It helps data engineers and application developers access data from modern data stores, organize it into consistent definitions, and deliver it to every application.

#### backbone

#### tanstack/query

</details>

### cms

https://github.com/topics/content-management-system?l=typescript

https://github.com/topics/cms?l=typescript

#### bodiless-js

https://www.bodiless-js.org/

https://github.com/johnsonandjohnson/Bodiless-JS - 130 stars

Framework for building editable websites on the JAMStack

using isomorphic-git

"edit this page" feature = (almost) inplace editing

edit content and presentation (style, menus, ...)

#### payload

https://github.com/payloadcms/payload - 9K stars

#### tinacms

https://github.com/tinacms/tinacms - 8K stars

https://tina.io/

A headless CMS for Markdown

using isomorphic-git

has inplace-editing

https://tina.io/blog/introducing-visual-open-authoring/

> We're focused on improving the independent web as a whole. We want to craft tools that help people build better sites and create better content.
>
> In line with this mission, we sought to create something we call **Visual Open Authoring**. The [open authoring concept](https://css-tricks.com/netlify-cms-open-authoring/#article-header-id-0) was originally pioneered by Netlify CMS this past summer and involves opening up your website's CMS to accept content-related contributions from anyone. Using Tina, we were able to take this a step further, establishing the editing context on the page itself.
>
> Add an "Edit Mode" to Your Site
>
> Picture this: on your website, there's an "Edit this page on GitHub" link — familiar enough. But imagine that when you click it, instead of kicking you over to GitHub, the site itself becomes editable, like a Google Doc.
>
> The experience should feel familiar for anyone that has used a word processor or site builder. You navigate to the page you want to change, click “Edit”, make updates in a WYSIWYG, and then submit your changes. That’s it.

can be used with capri https://capri.build/docs/integrations/tinacms/ to replace next.js with react/svelte/solid/vue/...

> TinaCMS is an open source, Git-backed CMS that adds a custom **visual editing** experience to a site.
>
> While Tina is originally made for Next.js sites, it can also be used as content source for Capri.
>
> When used with Capri, you get a completely static site that does not need a Node server and can be deployed to any static hosting service, including S3, GitHub Pages, surge or Firebase Hosting.
>
> The cool thing is that the resulting website will ship zero KB of JavaScript to the browser. Still, under /admin you get the full Tina editing experience as single page app!

- https://github.com/capri-js/tinacms-cloud-capri
- https://github.com/capri-js/tinacms-barebones-capri

#### netlify-cms

https://github.com/netlify/netlify-cms - 16K stars

> A Git-based CMS for Static Site Generators
>
> Give users a simple way to edit and add content to any site built with a static site generator.

https://github.com/vencax/netlify-cms-github-oauth-provider

#### alinea

https://github.com/alineacms/alinea - 600 stars

Content management for the modern web

Content is stored in flat files and committed to your repository

#### remix-cms

https://github.com/ShafSpecs/remix-cms

+ github api

#### github-pages-cms

https://github.com/jansmolders86/github-pages-cms - 50 stars

### writing tools

#### obsidian.md

https://obsidian.md/

https://github.com/obsidianmd/obsidian-releases - 4K stars

Obsidian is not open source software and this repo DOES NOT contain the source code of Obsidian

markdown editor

closed source

alternatives: codimd

##### obsidian-git

https://github.com/denolehov/obsidian-git - 3K stars

git backend for obsidian

### book printing

render a book-layout for printing

at the end of the day, digital is just a medium,
and the actual target is always print

todo: latex in javascript? (vanilla javascript, no wasm)

a good layout engine can distribute content across pages
to meet 2 constraints:

1. pretty
2. compact

so, this tool should also be usable as a book-authoring software,
where readers can modify and reprint a book ("editable book")

#### bindery

https://github.com/evnbr/bindery

i tried other tools, but for now im using bindery

its not perfect, sometimes the layout is too compact and ugly,
so i must insert manual pagebreaks ... but it works

## too much

could be useful, but too complex

### daedalOS

https://github.com/DustinBrett/daedalOS - 6K stars

Desktop environment in the browser.

## crdt

aka: live collaboration, realtime collaboration

- https://github.com/yjs/yjs
  - https://github.com/y-crdt/y-crdt/tree/main/ywasm
- https://github.com/automerge/automerge
- https://github.com/inkandswitch/peritext - 500 stars - A CRDT for asynchronous rich-text collaboration, where authors can work independently and then merge their changes.
- https://github.com/jupyterlab/rtc - 200 stars - Real Time Collaboration for Jupyter and beyond

## annotations and comments

- https://github.com/hypothesis
- https://github.com/LicomJS/Licom
- https://github.com/gab-ai-inc/gab-dissenter-extension

## self replicating

- Samizdat is a self-replicating LiveCD @ https://github.com/redecentralize/alternative-internet

## git based

- https://github.com/redecentralize/alternative-internet
- https://github.com/hbons/SparkleShare - 5K stars - self-hosted file sync service, similar to Dropbox and based on Git. Linux, macOS, and Windows.
- https://github.com/raybejjani/gitsync - git repository synchronisation and discovery tool. Its goal is to allow developers to coordinate without a central master repository.
- https://github.com/cjb/GitTorrent - peer-to-peer network of Git repositories being shared over BitTorrent.
- https://github.com/MichaelMure/git-bug - distributed, offline-first bug tracker embedded in git.

<details>

- https://github.com/neithernut/git-dit - distributed issue tracker for git.
- archived: https://github.com/cryptosphere/cryptosphere - http://cryptosphere.org/ - global peer-to-peer cryptosystem for publishing and securely distributing both data and HTML5/JS applications pseudonymously with no central point of failure. It's built on top of the next-generation Networking and Cryptography (NaCl) library and the Git data model. nacl: http://nacl.cr.yp.to/

</details>


## web desktop

aka: browser os

- http://blog.grimwire.com/#2013-04-04-grimwire.md - browser OS which uses Web Workers for process isolation, and WebRTC for peer-to-peer communication.





## publishing

- http://firecloud.co/ - P2P web publishing platform in your using Persona and WebRTC to work its magic.




