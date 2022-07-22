---
title: "Hugo Conversion"
date: 2022-07-15T12:06:46-05:00
tags: ["hugo", "projects"]
---

migrating this site from [ssg](https://romanzolotarev.com/ssg.html) as a static site generator to [hugo](https://gohugo.io/). I want tags and more control over the pages, article lists, etc. i've had experience with (jekyll)[https://gavingreer.com] in the past with decent results, i hear great things about hugo. I toyed with the idea of building out ssg with more functionality, but doubt my abilities to create such a solution. 

i made a mild attempt at hugo years ago, but it required more understanding upfront than jekyll (or so i thought) mostly because there was no built in theme (and i'm lazy). recently [Luke Smith](https://lukesmith.xyz) put out a [video on hugo](https://videos.lukesmith.xyz/w/oz4VV8SrnTEACCndxMASZH), getting started with a [basic theme he created](https://github.com/LukeSmithxyz/lugo). i always appreciate practical approaches and i'm using this as a basis to convert this site. 

1. copy existing markdown articles from shibby to the content directory and add front matter.
2. figure out how to migrate static pages
3. add navbar
4. migrate static elements 
5. create index page
6. figure out list filtering
7. organize images and relink 
8. maintain absolute paths of existing media, images, files

## what I've learned so far

### creating lists 
when creating lists, there are two (that i've seen so far) methods to list the pages.  
- `range .Pages`
  - lists everything, section indexes, pages within sections, everything
- `range .RegularPages`
  - lists only the pages in the root content directory

### filtering posts in lists   
on the main index page, i wanted to only list regular pages and i wanted a way to filter some from the main list. i used built in hugo filters to do this.
```html
{{- range.RegularPages }}
	{{ if (not (isset .Params "liexclude")) | or (eq .Params.liexclude false) }}
		<li><time datetime="{{ .Date.Format "2006-01-02T15:04:05Z07:00" }}">{{ .Date.Format "2006 Jan 02" }}</time> | <a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
	{{ end }}
{{ end -}}
```
the if statement filters the regular pages list and looks for at least one condition. if the `liexclude` parameter is not set or if it equals false, then the page will be included in the list.

I also added a conditional to the footer to not include the nextprev.html partial where this param exists. Still to do is to exclude these pages from being included in when using the next/prev links.

### allpages layout
page layouts and types can be specified in frontmatter. i wanted to create a page to list all content but the theme default list and index list were limited to either regular pages or section pages. i created an `allpages/allpages.html` file in the layouts directory and specified the frontmatter of `allpages.md` as follows
```yaml
---
title: "All pages"
date: 2022-07-18T16:50:30-05:00
type: "allpages"
layout: "allpages"
liexclude: true
---
```
this works but... I'm still not clear on what a type is or why it is required here. I'm also not sure why it works when names `allpages/allpages.html` but not `allpages/list.html` regardless of the frontmatter.

### section specific archetypes 
new pages are created with `hugo new page.md`. you can create new pages in a specific section by adding the path to the filename. section specific archetypes or templates can be auto applied. I've created an archetype for the build-thread entries. 

## Deploy

with ssg, i used bash aliases to generate and deploy aptly named `generate && deploy`.

### ssg blog control
```bash
alias generate='rm -f dst/.files && ssg src dst "shibby.xyz" "https://shibby.xyz"'
alias deploy='rsync --rsync-path "sudo -u gavin rsync" -avP --delete /home/gavin/dev/shibby.xyz/dst/ mars:/var/www/shibby.xyz/'
```

hugo has its own generate function, just running `hugo` will build the site in the public folder. 

deploy is just a wrapper for rsync and can be converted to push the hugo site. 

```bash
alias deploy='rsync --rsync-path "sudo -u gavin rsync" -avP --delete /home/gavin/share/files/dev/shibbyxyz-hugo/public/ mars:/var/www/shibby.xyz/'
```

now, running `hugo && deploy` will push the site to the remote server. in the future, the plan is to make this more automated with a crude, elementary implementation of ci/cd.

seeing now that hugo has its own `deploy` command. I will revisit this in the future.