# How to build and test locally #

## Install Jekyll ##

Make sure Ruby (including RubyGems) are installed on your machine. You most like need the packages `rubi-dev` and `nodejs` as well.

Jekyll works fine under Windows using [Cygwin](http://cygwin.com). Install base Cygwin and the Ruby package.

To install Jekyll: `gem install jekyll`

## Test everything locally ##

To run in local development mode:
`jekyll serve`
And browse to http://localhost:4000/

Jekyll will detect changes and automatically regenerate the site. All you need to do is refresh your browser. The generated	site is in `_site`. This directory should not be pushed the repository. GitHub Pages takes care of that.

## Using additional Jekyll configuration ##

When you need some minor configuration changes to suite your personal testing needs you can use a secondary config.yml file.

For example, to adjust paths for local testing use a second config.yml something like this:
```bash
echo "baseurl: /~test/" > _my_config.yml
jekyll serve --config _config.yml,_my_config.yml
rm _my_config.yml
```

Don't push those personal testing files to the repository.
