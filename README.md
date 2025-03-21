# web-components-toolbox
The web component toolbox for any CMS but particularly used for [Web Components + Umbraco === Mutobo](http://mutobo.ch/)

## [documentation](https://github.com/mits-gossau/web-components-toolbox-2.0/tree/master/docs/README.md)
## [organize components](https://wiki.migros.net/display/OCC/Web+Components+CMS+Template)

Install:
- `git submodule update --init --recursive --force`\
VS Code: don't use the `--remote` flag, https://git-scm.com/book/en/v2/Git-Tools-Submodules \
VS Studio: `--remote` is required to load latest Frontend Solution into Backend Solution
- `npm install`

Previews:
- https://mits-gossau.github.io/web-components-toolbox-2.0/

Component generation:   
- To generate a new plain component use: `npm run make`

Component Rules:
- Components should share their breakpoint with children.
- A component directly requiring web components as children has to customElement define those, if not already defined. To do this, use fetchModules. See more at Shadow.js.
- A component triggers the rendering at the connected callback and must check, if it should render before doing so, to avoid multiple rendering.
- Add event listeners at the connected callback and remove them at the disconnected callback.

Event Driven Rules:
- A controller must not query-select nor output any css or html. The communication must be strictly through events.
- The only data, which can be passed between components except of events, is in case of a parent with child web components, through the constructor or attributes at creation time.

JS Rules:
- use as little JS as possible. First think, if your problem could be solved with CSS before using JS.
- run the linter from time to time.
- use as much jsdocs annotations as possible.
- for nice appearance use this.hidden boolean. See more at Shadow.js.
- added event listeners must be timely removed. This means calling addEventListener should not be async especially if the event target is outside of the web component.
- when extending Shadow.js, use this.root instead of this.shadowRoot (this.root is a getter on Shadow.js and exposes the right target according to the shadow mode)

CSS Rules:
- no double dashes in css selectors or within variables NO:(tile--stuff) or (--var-hi--stuff) [Note: Shadow.js cssNamespaceToVar function does select on double dashes] 
- no inline style tags / styles with css properties outside of a web component or a template which is loaded by a web component (allowed are only inline style tags with css variable settings)
- mobile font-size should not be smaller than 1rem
- no absolute CSS values like px except for borders. Everything has to be relative eg. em, vw, vh, etc.
- variablesCustom.css has to be kept as tiny as possible
- use templates instead of namespace-fallback
- each template must have a local preview
- ONE breakpoint strategy!
- use _\_max-width_\_ for "@media only screen and (max-width:" selector
- use _\_import-meta-url_\_ for all urls (!important, pass option "importMetaUrl: import.meta.url" to Shadow.js in the constructor to have the viewpoint of the Class inheriting Shadow.js) / Example [NewsPreview.js](https://github.com/mits-gossau/web-components-toolbox-2.0/blob/master/src/es/components/contentful/newsPreview/NewsPreview.js#L7)
- sort css properties and variables alphabetically
- variables naming rule: --{selector aka component aka namespace}-{css property}-{pseudo class or media query name} eg. --p-background-color-hover
- within the component don't use any name spacing eg. component header don't use --header-default-color just use --color the namespace can be added by the Shadow as an html attribute
- avoid overly use of reassigning / overwrite variables
- the default transition is: 0.3s ease-out

HTML Rules:
- Components which move at initialization (connectedCallback, or any live cycle event) nodes. Expl. ```Array.from(this.root.children).forEach(node => this.section.appendChild(node))``` must be avoided! Or the html must be written as ```<o-grid><section>...</section></o-grid>``` and the component must accept already prefilled children. o-header[header], o-footer[footer], o-navigation[nav], o-body[main], o-grid[section], o-wrapper[section]

HTML/CSS Tooling:
- vscode extensions: es6-string-html & es6-string-css

## Master Branch locked 🙌

Checklist for each component:
- [ ] Note date of cleaning/overhaul, fix comments, fix all type/file errors of red highlighted files, + ("return Promise.resolve" to async function)
- [x] Transform mouseEventElement (mouseover|mouseout) hover on parent to prototype Hover.js class
- [x] Remove fetchCSS.then replace and use replaces property
- [x] Split the Template Switch/Case into a separate function, that it can be overwritten by components which extend it
- [x] Change the loadChildComponents function to use the Shadow.js fetchModules
- [x] transform all import.meta.url.replace(/(.*\/)(.*)$/, '$1') to this.importMetaUrl

TODO:
- [ ] mdx icons pull dynamically from main repo https://git.intern.migros.net/mdx/mdx/-/tree/main/packages/icons/dist/svg
- [x] mdx compatibility use scss from here: https://git.intern.migros.net/mdx/mdx/-/blob/main/packages/web-components/src/components/button/button.scss , etc. to transcend the styles here: https://github.com/mits-gossau/web-components-toolbox-2.0-mdx and extend the component in this toolbox if anything is missing.
- [ ] playwright single component tests
- [ ] documenter.js / automatic story book documentation by comments
- [ ] Template.html api call to fetch page content for previews (playwright or https://github.com/Rob--W/cors-anywhere)
- [ ] redo header and navigation /\drem/, then eliminate all rem values (tbd @miduca)
- [ ] incorporate container query, grouping styles in layers, user preferences, dvh... https://www.smashingmagazine.com/2023/07/writing-css-2023/?utm_source=changelog-news
- [ ] rework the reset.css with the new learnings from https://medium.com/appwrite-io/css-layers-for-css-resets-f60f270aa1cd
- [ ] print.css and some ideas from here https://alvaromontoro.hashnode.dev/css-tip-style-your-radio-buttons-and-checkboxes-for-printing?utm_source=newsletter&utm_medium=email&utm_campaign=wdrl-309
- [ ] scroll timeout solutions replaced by scrollend https://developer.chrome.com/blog/scrollend-a-new-javascript-event/?utm_source=newsletter&utm_medium=email&utm_campaign=wdrl-310
