# How to install bootstrap on Rails 8 using esbuild

Do not use Importmaps — It will not bring Popper files.

Check out https://mixandgo.com/learn/ruby-on-rails/how-to-install-bootstrap — Cezar Halmagean wrote this in 2022 for Rails 7, and it was the reference I used to build this step-by-step guide.

## If you want to start a new project
run:
```
rails new project_name -j esbuild --css bootstrap
```
There will still be missing Tooltips and Popovers.
Go to `/app/javascript/application.js` and add:
```js
var tooltipTriggerList = [].slice.call(document.querySelectorAll('[data-bs-toggle="tooltip"]'))
var tooltipList = tooltipTriggerList.map(function (tooltipTriggerEl) {
  return new bootstrap.Tooltip(tooltipTriggerEl)
})
var popoverTriggerList = [].slice.call(document.querySelectorAll('[data-bs-toggle="popover"]'))
var popoverList = popoverTriggerList.map(function (popoverTriggerEl) {
  return new bootstrap.Popover(popoverTriggerEl)
})
```
Then run:
```
bin/dev
```
It will use the foreman gem to execute the commands inside the Procfile.dev.
So if it doesn't work the first time it's probably missing the gem (don't put it on your Gemfile).

To fix it:
```
gem install foreman
```

## You already have a project
Add cssbulding-rails gem to your project running:
```
bundle add cssbundling-rails
```
Run the css builder installing bootstrap:
```
./bin/rails css:install:bootstrap
```
Add jsbundling-rails gem to your project running:
```
bundle add jsbundling-rails
```
Run the javascript builder installing esbuild:
```
./bin/rails javascript:install:esbuild
```
Fix turbo-rails since it is not being imported via importmaps anymore:
```
yarn add @hotwired/turbo-rails
```
Same with stimulus:
```
yarn add @hotwired/stimulus
```
<br>
Fixing js files.

`app/javascript/application.js`
```diff
-import "controllers"
+import "./controllers";
```

`app/javascript/controllers/index.js`
```diff
-import { application } from "controllers/application"
-import { eagerLoadControllersFrom } from "@hotwired/stimulus-loading"
-eagerLoadControllersFrom("controllers", application)
+import { application } from "./application";
```

`app/views/layouts/application.html.erb`
```diff
-<%= javascript_importmap_tags %>
```
<br>

Install bootstrap javascript:
```
yarn add bootstrap
```
There will still be missing Tooltips and Popovers.
Go to `/app/javascript/application.js` and add:
```js
var tooltipTriggerList = [].slice.call(document.querySelectorAll('[data-bs-toggle="tooltip"]'))
var tooltipList = tooltipTriggerList.map(function (tooltipTriggerEl) {
  return new bootstrap.Tooltip(tooltipTriggerEl)
})
var popoverTriggerList = [].slice.call(document.querySelectorAll('[data-bs-toggle="popover"]'))
var popoverList = popoverTriggerList.map(function (popoverTriggerEl) {
  return new bootstrap.Popover(popoverTriggerEl)
})
```
Then run:
```
bin/dev
```
It will use the foreman gem to execute the commands inside the Procfile.dev.
So if it doesn't work the first time it's probably missing the gem (don't put it on your Gemfile).

To fix it:
```
gem install foreman
```
