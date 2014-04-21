# Reddit.js
Reddit.js is a browser based wrapper on most of the read-only [Reddit API](http://www.reddit.com/dev/api/oauth#scope_read). It makes [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) requests to the [Reddit API](http://www.reddit.com/r/changelog/comments/1r0u3v/reddit_change_third_party_websites_can_now_make/). Thus only unauthenicated (logged-out) requests are allowed. Latest versions of Internet Explorer, Chrome, FireFox are tested via automated tests running on [Testling](https://ci.testling.com/sahilm/reddit.js). Other browsers such as Internet Explorer 9 and the Android browser are expected to work but are tested manually.

[![browser support](https://ci.testling.com/sahilm/reddit.js.png)
](https://ci.testling.com/sahilm/reddit.js)

# Usage

Reddit.js is a lightweight dependency-free library which comes with minified source. Include the minified source with a script tag.
```html
  <script src="reddit.min.js"></script>
```

The `reddit` variable is exported which can be used to talk to the Reddit API.

```javascript
  // Fetch the 5 hottest posts on /r/awww
  reddit.hot('aww').limit(5).fetch(function(res) {
    // res contains JSON parsed response from Reddit
    console.log(res);
  });
```

```javascript
  // Errors (if any) will be passed to the 2nd argument of fetch()
  reddit.hot('aww').limit(5).fetch(function(res) {}, function(err) {
    // err contains the error from Reddit
  });
```

```javascript
  // All Reddit API filters are supported and chainable
  reddit.hot('aww').limit(1).after("t3_23jf8n").fetch(function(res) {
    // res contains JSON parsed response from Reddit
    console.log(res);
  });
```

```javascript
  // Subreddits may be omitted
  // Fetch the hottest posts from Reddit
  reddit.hot().fetch(function(res) {
    console.log(res);
  });
```

# Supported API Endpoints

* [Hot](http://www.reddit.com/dev/api/oauth#GET_hot)

Fetch the hottest posts on Reddit or on a subreddit. All filters such as `after`, `before`, `limit` are supported.

```javascript
  reddit.hot('aww').limit(1).after("t3_23jf8n").fetch(function(res) {
    // res contains JSON parsed response from Reddit
    console.log(res);
  });
```

* [New](http://www.reddit.com/dev/api/oauth#GET_new)

Fetch the newest posts on Reddit or on a subreddit. Again all filters are supported.

```javascript
  reddit.new('programming').before("t3_23jf8n").fetch(function(res) {
    // res contains JSON parsed response from Reddit
    console.log(res);
  });
```

* [Top](http://www.reddit.com/dev/api/oauth#GET_top)

Fetch the top posts on Reddit or on a subreddit. As usual all filters are supported. The `t` filter allows filtering by top posts of the day, week, month, year or all-time.

```javascript
  // Fetch the top 5 funniest posts of all time from /r/funny
  reddit.top('funny').t('all').limit(5).fetch(function(res) {
    console.log(res);
  });
```

* [Controversial](http://www.reddit.com/dev/api/oauth#GET_controversial)

Fetch the top posts on Reddit or on a subreddit As usual all filters are supported.. The `t` filter allows filtering by top posts of the day, week, month, year or all-time.

```javascript
  // Fetch the 25 most controversial posts from this month
  reddit.controversial().t('month').limit(25).fetch(function(res) {
    console.log(res);
  });
```

* [Comments](http://www.reddit.com/dev/api/oauth#GET_comments_{article})

Fetch comments on a link. As usual all filters are supported. Context and depth are useful filters to narrow or highlight comments.

```javascript
  // Fetch the hottest comment on article 23ha0a from /r/pics
  reddit.comments("23ha0a", "pics").limit(1).sort("hot").fetch(function(res) {
    console.log(res);
  });
```

* [Search](http://www.reddit.com/dev/api/oauth#GET_search)

Search everything. All filters including `syntax` and `t` are supported.

```javascript
  reddit.search("programming").t('month').limit(1).sort("hot").fetch();
```

* [Search subreddits](http://www.reddit.com/dev/api/oauth#GET_subreddits_search)

Search subreddits by title and description. Filter away.

```javascript
  // Fetch 5 subreddits related to gardening
  reddit.searchSubreddits("gardening").limit(5).fetch();
```

* [About](http://www.reddit.com/dev/api/oauth#GET_r_{subreddit}_about)

Fetch information about Reddit or a subreddit.

```javascript
  // Fetch information about /r/pics
  reddit.about("pics").fetch(function(res) {
    console.log(res);
  });
```

* [Random](http://www.reddit.com/dev/api/oauth#GET_random)

The Serendipity button.

```javascript
  // Fetch random articles from /r/funny
  reddit.random("funny").fetch(function(res) {
    console.log(res);
  });
```

* [Info](http://www.reddit.com/dev/api/oauth#GET_api_info)

Fetch a link by fullname or a list of links by URL. Can be filtered by a subreddit.

```javascript
  // Fetch info about reddit itself
  reddit.info().url("http://www.reddit.com/").fetch(function(res) {
    console.log(res);
  });
```

* [Recommended subreddits](http://www.reddit.com/dev/api/oauth#GET_api_recommend_sr_{srnames})

Fetch a list of recommended subreddits based on the comma separated list of subreddits supplied. Can be filtered by the `omit` param.

```javascript
 reddit.recommendedSubreddits("programming").omit("leapmotion,CFD").fetch(function(res) {
    console.log(res);
  });
```

* [Subreddits by topic](http://www.reddit.com/dev/api/oauth#GET_api_subreddits_by_topic)

Fetch list of subreddits that are relevant to a search query.

```javascript
 reddit.subredditsByTopic("programming").fetch();
```

* [Popular subreddits](http://www.reddit.com/dev/api/oauth#GET_subreddits_{where})

Fetch subreddits ordered by activity.

```javascript
  // Fetch the 25 most popular subreddits
  reddit.popularSubreddits().limit(25).fetch();
```

* [New subreddits](http://www.reddit.com/dev/api/oauth#GET_subreddits_{where})

Fetch newest subreddits.

```javascript
  // Fetch the 25 newest subreddits.
  reddit.newSubreddits().limit(25).fetch();
```

* [About user](http://www.reddit.com/dev/api/oauth#GET_user_{username}_about.json)

Fetch information about the user, including karma and gold status.

```javascript
  reddit.aboutUser("chromakode").fetch();
```
