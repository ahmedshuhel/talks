### Slide Link
https://docs.google.com/presentation/d/1HQQv-1mI9V3wXa319eIPZipcgd8yzB4VSrPP48eGZ7I/edit#slide=id.gc6f80d1ff_0_0


```javascript

function publishArticle(id, channelId) {
  // get article content
  // get channel from store.
  // validate article content as per cannel spec
  // ensure accesstoken
  // query channel for duplicates
  // publish to channel.
}

function publishArticle(id, channelId, callback) {
  let article, channel;
  getArticleContent(id, function (err, _article) {
    article = _article;
    if (err) {
      return callback(err);
    }
    if (!article) {
      return callback(new Error('article not found with id ' + id ));
    }
    getChannel(channelId, function (err, _channel) {
      channel = _channel;
      if (err) {
        return callback(err);
      }
      if (!channel) {
        return callback(new Error('no channel found with id ' + channelId));
      }
      let channelSpec = channel.articleSpec;
      validateArticle(article, channelSpec, function (err, res) {
        if (err) {
          return callback(err);
        }
        if (!res.isValid) {
          return callback(new Error(res.validationErrors));
        }
        ensureAccessToken(channel, function (err) {
          if (err) {
            return callback(err);
          }
          findDuplicate(channel, article, function (err, hasDup) {
            if (err) {
              return callback(err);
            }
            if (hasDup) {
              return callback(new Error('Duplicate found!'));
            }
            publishToChannel(channel, article, callback);
          });
        });
      });
    });
  });

}

function getArticle(id, cb) { ... }
function getChannel(id, cb) { ... }
function validateArticle(article, spec, cb) { ... }
function ensureAccessToken(channel, cb) { ... }
function findDuplicate(article, channel, cb) { ... }
function publishToChannel(article, channel, cb) { ... }


function publishArticle(articleId, channelId) {
  let channel, article;
  getChannel()
    .then(_channel => {
      channel = channel;
      return getArticle(articleId)
        .then(_article => {
          article = _article;
          return validateArticle(article, channel.articleSpec)
            .then(() => {
              return ensureAccessToken(channel)
                .then(_ => {
                  return hasDuplicate(channel, article)
                    .then(_ => {
                      return publishToChannel(channel, article)
                    });
                });
            });
        });
    });
}

function publishArticle(id, channelId) {
  let channel, article;
    return Promise.all([
      getChannel(),
      getArticle()
    ])
    .then(([_channel, _article]) => {
      channel = _channel;
      article = _article;
    })
    .then(_ => validateArticle(article, channle.articleSpec))
    .then(_ => ensureAccessToken(channel))
    .then(_ => hasDuplicate(channel, article))
    .then(_ => publishToChannel(channel, article))
}

```

