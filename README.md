# Step 5: Hot Reloading

[Back to step 4](https://github.com/mbrochh/django-reactjs-boilerplate/tree/step4_use_the_bundle)

Step 4 was nice and awesome, but not mind-blowing. Let's do mind-blowing now.
We don't really want to run that `webpack` command every time we change our
ReactJS app (and create thousands of local bundles in the process). We want to
see the changes in the browser immediately.

First, we need a `server.js` file that will start a webpack-dev-server for us:

```javascript
var webpack = require('webpack')
var WebpackDevServer = require('webpack-dev-server')
var config = require('./webpack.local.config')

new WebpackDevServer(webpack(config), {
  publicPath: config.output.publicPath,
  hot: true,
  inline: true,
  historyApiFallback: true,
}).listen(3000, config.ip, function (err, result) {
  if (err) {
    console.log(err)
  }

  console.log('Listening at ' + config.ip + ':3000')
})
```

Next, we need to add/replace the following in our `webpack.local.config.js`:

```javascript
var ip = 'localhost'

config.entry = {
  App1: [
    'webpack-dev-server/client?http://' + ip + ':3000',
    'webpack/hot/only-dev-server',
    './reactjs/App1',
  ],
}

config.output.publicPath = 'http://' + ip + ':3000' + '/assets/bundles/'

config.plugins = config.plugins.concat([
  new webpack.HotModuleReplacementPlugin(),
  new webpack.NoErrorsPlugin(),
  new BundleTracker({filename: './webpack-stats-local.json'}),
])
```

Ready? In one terminal window, start the webpack-dev-server with
`nodejs server.js` and in another terminal window, start the Django devserver
with `./manage.py runserver`.

Make sure that you can still see "Something New!".

And now change it to `Something Fancy!` in `containers/App1Container.jsx` and
switch back to your browser. If you are very fast, you can see how it updates
itself.

There is another cool thing: When you open the site in Google Chrome and open
the developer tools with `COMMAND+OPTION+i` and then open the `Sources` tab,
you can see `webpack://` in the sidebar. It has a folder called `.` where you
will find the original ReactJS sources. You can even put breakpoints here and
debug your app like a pro. No more `console.log()` in your JavaScript code.

[Step 6: Going to production](https://github.com/mbrochh/django-reactjs-boilerplate/tree/step6_going_to_production)
