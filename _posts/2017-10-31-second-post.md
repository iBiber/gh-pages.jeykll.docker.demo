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
