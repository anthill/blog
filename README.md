ANTS blog
========

### Install

- Clone this repo and `cd` to it
- Install bundler `gem install bundler`
- Install local dependencies `bundle install`
- To see the blog locally: `bundle exec jekyll serve --baseurl ''`

You should have a server up and running locally at <http://localhost:4000>.

### Writing a blog

#### Drafts

Initially write your post in the `_drafts` and the name should be of the form:

```title-of-your-post.markdown```

Use ```bundle exec jekyll serve --baseurl '' --drafts``` to compile them (the date will be added automatically).

#### Posts

Once you're ready to publish, move your file to `_posts` and add the date:

```year-month-day-title-of-your-post.markdown```

respecting the header guidlines and place the images needed for your blog in ```assets/article_images/title-of-your-post/```.



#### Thanks

This blog is greatly inspired by [@dirkfabisch](https://twitter.com/dirkfabisch)'s awesome Mediator template.
