## DraftsPermalink

Drafts are posts without a date in the filename. They’re posts you’re still working on and don’t want to publish yet. To get up and running with drafts, create a _drafts folder in your site’s root and create your first draft:

```
.
├── _drafts
│   └── a-draft-post.md
...
```

To preview your site with drafts, run **jekyll serve** or **jekyll build** with the **--drafts** switch. Each will be assigned the value modification time of the draft file for its date, and thus you will see currently edited drafts as the latest posts.

```
jekyll serve --drafts 
or
jekyll build --drafts
```



### Список задач

1. Было бы круто добавить поиск на сайт.
2. [Category pages](https://www.amitmerchant.com/how-to-categorize-your-posts-in-jekyll/?__cf_chl_tk=k1jewWtV_eQk6xhPkseZ5TJTXDOVYcF3axRsmHjS3VE-1668194254-0-gaNycGzNCOU)

