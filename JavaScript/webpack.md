# Webpack Notes

## Setup

Install via `npm`

* `output.publicPath`

[An example](https://webpack.js.org/configuration/output/):

```javascript
module.exports = {
  //...
  output: {
    // One of the below
    publicPath: 'https://cdn.example.com/assets/', // CDN (always HTTPS)
    publicPath: '//cdn.example.com/assets/', // CDN (same protocol)
    publicPath: '/assets/', // server-relative
    publicPath: 'assets/', // relative to HTML page
    publicPath: '../assets/', // relative to HTML page
    publicPath: '', // relative to HTML page (same directory)
  }
};
```

## Reference

1. [Handling Images](https://medium.com/a-beginners-guide-for-webpack-2/handling-images-e1a2a2c28f8d)
2. [Loading images in React](https://stackoverflow.com/questions/34582405/react-wont-load-local-images)
3. [A tale of Webpack 4 and how to finally configure it in the right way](https://hackernoon.com/a-tale-of-webpack-4-and-how-to-finally-configure-it-in-the-right-way-4e94c8e7e5c1)