# browserforge

html files are inplace-editable documents like [dokieli](https://dokie.li/)
which can be shared over peer-to-peer networking (wifi, bluetooth, sneakernet)

everything runs in the browser

## status

concept

## specs

### inplace-editing

document is a html page
by default, it is read-only (not editable)
when i click "edit" then load the editor
= make the document editable \*in place\* = no page reload

### offline first

aka: local-first, dweb, nocloud

https://github.com/topics/offline-first
https://github.com/topics/local-first
https://github.com/topics/dweb
https://github.com/topics/dapp

because ...

[There is no cloud, it’s just someone else’s computer](https://www.chriswatterston.com/article/success-of-my-there-is-no-cloud-sticker)

![There is no cloud, it’s just someone else’s computer](there-is-no-cloud.jpg)

#### data flow

on the first page load, the full git repo is fetched and stored in the browser (indexeddb or folder api)

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

### TiddlyWiki

https://github.com/Jermolene/TiddlyWiki5 - 7K stars

A self-contained JavaScript wiki for the browser, Node.js, AWS Lambda etc.

https://tiddlywiki.com/

https://en.wikipedia.org/wiki/TiddlyWiki

> TiddlyWiki is a personal wiki and a non-linear notebook for organising and sharing complex information. It is an open-source single page application wiki in the form of a **single HTML file** that includes CSS, JavaScript, embedded files such as images, and the text content. It is designed to be easy to customize and re-shape depending on application. It facilitates re-use of content by dividing it into small pieces called Tiddlers.
>
> TiddlyWiki is an unusual example of a practical quine. This idea of producing a copy of its own source code that lies at the heart of TiddlyWiki's ability to independently save changes to itself.

storage: single HTML file

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

#### stackedit.io

https://stackedit.io/

https://github.com/benweet/stackedit - 20K stars

markdown editor

browser app

backends: github, gitlab, google drive, dropbox

<details>

#### dillinger.io

https://dillinger.io/

https://github.com/joemccann/dillinger - 8K stars

markdown editor

backends: github, google drive, dropbox, one drive, medium

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

#### code-server

https://github.com/coder/code-server - 60K stars

VS Code in the browser

#### Eclipse Theia

https://theia-ide.org/

https://github.com/eclipse-theia/theia - 18K stars

Cloud & Desktop IDE

compatible with vscode extensions

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

#### payload

https://github.com/payloadcms/payload - 9K stars

#### tinacms

https://github.com/tinacms/tinacms - 8K stars

https://tina.io/

A headless CMS for Markdown

using isomorphic-git

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
