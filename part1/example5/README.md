# Example 5 - Adding More Plugins

Now that we have the infrastructure for styling our website we need an actual page to style. We'll be doing this through the [html-webpack-plugin](https://github.com/ampedandwired/html-webpack-plugin), which lets us generate an HTML page or use an existing one. We'll use an existing one `website.html`.

First we install the plugin:

    npm install --save-dev html-webpack-plugin@2

Then we can add it to our config

```javascript
// webpack.config.js
var path = require('path')
var webpack = require('webpack')
var HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: ['./src/index'], // .js after index is optional
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin({
      compressor: {
        warnings: false,
      },
    }),
    new webpack.optimize.OccurenceOrderPlugin(),
    new HtmlWebpackPlugin({
      filename: 'index.html',
      template: './src/website.html'
    })
  ],
  modules: {
    loaders: [{
      test: /\.css$/,
      loaders: ["style", "css"]
    }]
  }
}
```

Adding the plugin just means requiring it, and adding it to your plugins array. We also specify that we want it to use `website.html` as the template, but call it index.html in the dist folder.

We'll also have to actually put something in `website.html`

```html
<html>
<head>
  <title>Webpack Tutorial</title>
</head>
<body>
  <h1>Very Website</h1>
  <section id="color"></section>
  <button>Such Button</button>
  <script src="bundle.js"></script>
</body>
</html>
```

and while we're at it let's add some basic styling in `styles.css`

```css
h1 {
  color: rgb(114, 191, 190);
}

#color {
  width: 300px;
  height: 300px;
  margin: 0 auto;
}

button {
  cursor: pointer;
  display: inline-block;
  height: 20px;
  background-color: rgb(123, 109, 198);
  color: rgb(255, 255, 255);
  padding: 10px 5px;
  border-radius: 4px;
  border-bottom: 2px solid rgb(143, 132, 200);
}
```

#### Side note

I feel like I should mention that the `html-webpack-plugin` should
be used sparingly. To me, webpack should generate HTML files if you just have a really simple
one to bootstrap a SPA. So while it was useful for the learning experience, which required only
one HTML file, I wouldn't recommend it to generate 12 HTML files. This doesn't mean you can't use
html files with something like angular directives, which require HTML template files. In that case
you could do something like:

```javascript
// ...directive stuff
template: require('./templates/button.html') // using raw loader
```

Instead, it means that you should not be doing something like this:

```javascript
new HtmlWebpackPlugin
  template: './src/index.html'
}),
new HtmlWebpackPlugin({
  template: './src/button.html'
}),new HtmlWebpackPlugin({
  template: './src/page2.html'
})
```

Anyone with other experience feel free to correct me if I'm wrong.