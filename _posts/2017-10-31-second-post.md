---
layout: post
title:  "Using jekyll with docker"
author: Myself
date:   2017-10-31 04:19:01 +0100
categories: jekyll docker
---
To generate the site for the prepared jekyll project, change to the project root directory and use:
```
docker run --rm --volume="$PWD:/srv/jekyll" --volume="$PWD/vendor/bundle:/usr/local/bundle" -it jekyll/builder:$JEKYLL_VERSION jekyll build
```

To test it locally (this will initiate also a build):
```
docker run --rm --volume="$PWD:/srv/jekyll" --volume="$PWD/vendor/bundle:/usr/local/bundle" -p 4000:4000 -it jekyll/builder:$JEKYLL_VERSION jekyll serve --watch
```

`--volume="$PWD:/srv/jekyll"` binds the current directory (`./`) for the docker container  
`--volume="$PWD/vendor/bundle:/usr/local/bundle"` binds the `./vendor/bundle` folder for the docker container, where the downloaded gem-dependencies get cached for the site.

To do so, the life is easier when the jekyll site files are located in an own directory (app in this case).
Otherwise every file which should not be part of the web-page, must be excluded.

# github-pages
github-pages are a little bit more complicated because there are boundaries of
the framework. Currently I am not sure which restrictions and necessary
settings must be made to get a jekyll site working. I was able to get this
jekyll site working locally and on github-pages with the following changes:

1. All files and folders must be placed in the project root, next to the `.git` folder.
2. Add `gem "github-pages", group: :jekyll_plugins` to the Gemfile
3. Add the theme to the `_config.yml` (for instance `theme: jekyll-theme-midnight`)
4. In the `index.html` a variable builds the links to  the blog-posts `{{ post.url }}`
This is migth be okay when it is a personal github-page for your account (e.g. yourName.github.io).
If the site is for a github-project and will be accessible with an extension
e.g. yourName.github.io/yourProject/ then this will not work and the link must be build with
'{{ site.baseurl }}{{ post.url }}'. Otherwise the links will be build with yourName.github.io/yourPost instead of yourName.github.io/yourProject/yourPost.
5. After changing the Gemfile, it seems that I had to call `bundle install` (instead of `jekyll build`) with the docker container
6. To get it run locally, the `_config.yml` must be extended with these settings:
```
url: "https://yourName.github.io"
baseurl: "/yourProject"
repository: yourName/yourProject
```
7. Now it should be possible to start the jekyll-site with `jekyll serve` but
now it must be accessed with the baseurl e.g. http://localhost:4000/yourProject/index.html
