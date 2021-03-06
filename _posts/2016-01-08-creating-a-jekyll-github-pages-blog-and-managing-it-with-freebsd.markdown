---
layout: post
comments: true
title:  "Creating A Jekyll GitHub Pages Blog and Managing it With FreeBSD"
date:   2016-01-08 03:07:15 +0900
categories: jekyll github freebsd
---
I have wanted a low maintenance technical blogging solution for a while.  A Jekyll blog on GitHub Pages meets all of my criteria.  Write posts in markdown, commit and push.  The people at GitHub Pages will almost certainly keep the servers up.

These instructions are written for FreeBSD because that is what I use.  The same general approach should work for any operating system.

## Software Versions
{% highlight sh %}
$ date
January  8, 2016 at 03:07:15 AM JST
$ uname -a
FreeBSD mirage.sennue.com 11.0-CURRENT FreeBSD 11.0-CURRENT #0 r287598: Thu Sep 10 14:45:48 JST 2015     root@:/usr/obj/usr/src/sys/MIRAGE_KERNEL  amd64
$ ruby --version
ruby 2.1.8p440 (2015-12-16 revision 53160) [amd64-freebsd11]
$ jekyll --version
jekyll 3.0.1
{% endhighlight %}

## Instructions
First, create a new GitHub repository named *my_username*.github.io, where *my_username* is your GitHub username.  The repository needs to have ".github.io" at the end.

Next, install Jekyll, create a new blog and push it to github.
{% highlight sh %}
# install jekyll
su
portmaster devel/ruby-gems
gem install jekyll
gem install github-pages # optional, for local preview
exit

# create a new blog
# replace my_username with your username
# replace my_blog directory with your preferred project organization
cd # if you need to get $HOME
mkdir my_blog
cd my_blog
jekyll new my_username.github.io
cd my_username.github.io

# add ssh key to session using the sh shell
# assumes you are set up to use ssh
# use the following link to set up ssh
# https://help.github.com/articles/generating-ssh-keys/
sh
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa

# add project to git and push to github
# replace my_username with your username
# use the https github url if ssh is not set up
# https://github.com/my_username/my_username.github.io.git
git init
git add .
git commit -m "Initial commit."
git remote add origin git@github.com:my_username/my_username.github.io.git
git remote -v
git push origin master

# configure blog
vim _config.yml

# save welcome post in drafts for future reference
# replace YYYY-MM-DD with the post date
# the archived welcome post can be deleted when it is no longer needed
mkdir _drafts
mv _posts/YYYY-MM-DD-welcome-to-jekyll.markdown _drafts/

# save default about in drafts for future reference
# the archived about file can be deleted when it is no longer needed
# customize about page
cp about.md _drafts/about.markdown
vim _drafts/about.markdown # remove permalink line, change layout to post
vim about.md

# create post
# replace YYYY-MM-DD the post date
# replace name-of-post with the title of the post
vim _posts/YYYY-MM-DD-title-of-post.markdown

# local build / test for errors
jekyll build --incremental --drafts

# local preview
jekyll serve --host 0.0.0.0 --port 4000 --drafts --watch

# sync changes
git add .
git commit -m "Change message."
git push
{% endhighlight %}

## References:
- [GitHub Pages Setup Instructions](https://pages.github.com)
- [GitHub Markdown Basics](https://help.github.com/articles/markdown-basics/)
- [GitHub Generating SSH Keys](https://help.github.com/articles/generating-ssh-keys/)
- [GitHub Add Existing Project](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/)
- [Jekyll Quick-Start Guide](http://jekyllrb.com/docs/quickstart/)
- [Jekyll Configuration](http://jekyllrb.com/docs/configuration/)
- [Jekyll Writing posts](http://jekyllrb.com/docs/posts/)
- [Jekyll Working with drafts](http://jekyllrb.com/docs/drafts/)
- [Jekyll, Build A Blog With Jekyll And GitHub Pages](https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/)
- [Jekyll bug: Tag was never closed](http://blog.slaks.net/2013-08-09/jekyll-tag-was-never-closed/)
- [FreeBSD Handbook Using the Ports Collection](https://www.freebsd.org/doc/handbook/ports-using.html)
