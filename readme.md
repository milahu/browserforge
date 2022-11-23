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

code and data - both can be modified with this system,
so it can be called "self-modifying code" (in a good sense, a "good virus")
more precise a "self-modifying browser-app"

#### pazguille/offline-first

https://github.com/pazguille/offline-first
Everything you need to know to create offline-first web apps

### html first

javascript is optional
no build steps, no SSR, no markdown

"html is the mother language" -- rich harris, svelte

markdown can be derived from html

browser first, native second https://jasonette.com/

ui framework should be "SSR by default" like https://github.com/BuilderIO/qwik
to produce static html pages, which run without javascript.
optional javascript dependencies are lazy-loaded (on demand) to reduce memory footprint of the app

## based on

this is a fusion of multiple tools

### html editor

aka: rich text editor, wysiwyg editor

#### prosemirror

https://github.com/ProseMirror/prosemirror-view - 1K stars (underrated)
rich-text editor

see also:
https://tiptap.dev/guide/collaborative-editing
https://tiptap.dev/hocuspocus/
https://github.com/pubpub/pubpub-editor - based on react, prosemirror - 100 stars
https://github.com/chanzuckerberg/czi-prosemirror - based on react, prosemirror - 300 stars

### static web hosting

github.com pages, gitlab.com pages, codeberg.org pages ...

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

github backend: https://github.com/codesandbox/codesandbox-importers/blob/master/packages/git-extractor/src/routes/github/pull/download.ts

#### code-server

https://github.com/coder/code-server - 60K stars

VS Code in the browser

#### Eclipse Theia

https://theia-ide.org/

https://github.com/eclipse-theia/theia - 18K stars

Cloud & Desktop IDE

compatible with vscode extensions

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

#### js-git

https://github.com/creationix/js-git - 4K stars
https://www.npmjs.com/package/js-git - 1M downloads
A JavaScript implementation of Git

> I've been asking Github to enable CORS headers to their HTTPS git servers, but they've refused to do it. This means that a browser can never clone from github because the browser will disallow XHR requests to the domain.

https://github.com/creationix/js-github - 160 stars
A js-git mixin that uses github as the data storage backend.

example with indexeddb caching

#### git-documentdb

https://github.com/sosuisen/git-documentdb

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
