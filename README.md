# jQCloud: beautiful word clouds with jQuery

jQCloud is a jQuery plugin that builds neat and pure HTML + CSS word clouds and tag clouds that are actually shaped like a cloud (otherwise, why should we call them 'word clouds'?).

You can see a demo here: http://www.lucaongaro.eu/demos/jqcloud/

## Installation

Installing jQCloud is extremely simple:

1. Make sure to import the jQuery library in your project.
2. Download the jQCloud files. Place [jqcloud-0.2.10.js](https://raw.github.com/DukeLeNoir/jQCloud/master/jqcloud/jqcloud-0.2.10.js) (or the minified version [jqcloud-0.2.10.min.js](https://raw.github.com/DukeLeNoir/jQCloud/master/jqcloud/jqcloud-0.2.10.min.js)) and [jqcloud.css](https://raw.github.com/DukeLeNoir/jQCloud/master/jqcloud/jqcloud.css) somewhere in your project and import both of them in your HTML code.

You can easily substitute jqcloud.css with a custom CSS stylesheet following the guidelines explained later.

## Usage

Once you imported the .js and .css files, drawing a cloud is as simple as this:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>jQCloud Example</title>
    <link rel="stylesheet" type="text/css" href="jqcloud.css" />
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.4/jquery.min.js"></script>
    <script type="text/javascript" src="jqcloud-0.2.10.js"></script>
    <script type="text/javascript">
      /*!
       * Create an array of objects to be passed to jQCloud, each representing a word in the cloud
       */
      var word_list = [
          {text: "Lorem", weight: 15},
          {text: "Ipsum", weight: 9, url: "http://jquery.com/", title: "I can haz URL"},
          {text: "Dolor", weight: 6},
          {text: "Sit", weight: 7},
          {text: "Amet", weight: 5}
          // ...other words
      ];
      $(document).ready(function() {
        // Call jQCloud on a jQuery object passing the word list as the first argument. Chainability of methods is maintained.
        $("#example").jQCloud(word_list);
      });
    </script>
  </head>
  <body>
    <!-- You must explicitly specify the dimensions of the container element -->
    <div id="example" style="width: 550px; height: 350px; position: relative;"></div>
  </body>
</html>
```

### Word Attributes

For each word, you need to specify the following mandatory attributes:
 
 * **text** (string): the word(s) text
 * **weight** (number): a number (integer or float) defining the relative importance of the word (such as the number of occurrencies, etc.). The range of values is arbitrary, as they will be linearly mapped to a discrete scale from 1 to 10.

You can also specify the following optional attributes for each word:

 * **url** (string): if specifyed, the word will be wrapped in a HTML link (`<a>` tag) and the URL used as its `href` attribute.
 * **title** (string): an HTML title for the `<span>` that will contain the word(s)
 * **callback** (callable): a function to be called after this word is rendered. Within the function, `this` refers to the jQuery object for the `<span>` containing the word.
 * **customClass** (string): an additional HTML class for the word
 * **dataAttributes** (object): keys and values to be used as HTML5 data attributes for the word (e.g. {confirm: "Are you sure?", remote: true})
 * **handlers** (object): an object specifying event handlers that will bind to the word (e.g.: {click: function() { alert("it works!"); } })

### Options:

Since version 0.2.0, jQCloud accepts an object containing configuration options as the second argument:

```javascript
$("#example").jQCloud(word_list, {
  width: 300,
  height: 200
});
```

The full list of available options is the following:

* **width** (number): The width of the word cloud. Defaults to the width of the container element.
* **height** (number): The height of the word cloud. Defaults to the height of the container element.
* **center** (object): The x and y coordinates of the center of the word cloud (e.g.: {x: 300, y: 150}). Defaults to the center of the container element.
* **callback** (function): A callback function to be called after the cloud is fully rendered. This is different from the callbacks defined for each word in which this one is called after the whole cloud is rendered.
* **delayedMode** (boolean): If true, words are rendered one after another with a tiny delay between each one. This prevents freezing of the browser when there are many words to be rendered. If false, the cloud will be rendered in one single shot. By default, delayedMode is true when there are more than 50 words.
* **randomClasses** (number or array): If it's a number and higher than 0, each word will be assigned randomly to a class of kind `r1`, `r2`, ..., `rN` with N = randomClasses. If it is an array of classes, each word will be assigned randomly to one of the classes in the array. This random class can be used for random CSS styling of words, for example to set a random color or random orientation (see `examples/vertical_words.html`).
* **nofollow** (boolean): if true, all words linked to a URL will have the `rel="nofollow"` attribute.
* **shape** (string): the shape of the cloud. By default it is elliptic, but it can be set to `"rectangular"` to draw a rectangular-shaped cloud.

### Note:

Since drawing the cloud is rather computationally intensive, cloud rendering isn't instantaneous. If you want to make sure that some code executes after the cloud is rendered, you can specify in the options a callback function:

```javascript
$("#example").jQCloud(word_list, {
  callback: function() {
    // This code executes after the cloud is fully rendered
  }
});
```

#### Deprecation warning:

Before version 0.2.0 jQCloud used to accept a callback function as the second argument. This way of specifying the callback function is deprecated and, although version 0.2.0 maintains backward compatibility, it will be removed in newer versions. If you need a callback function, use the 'callback' configuration option instead.

## Custom CSS guidelines

The word cloud produced by jQCloud is made of pure HTML, so you can style it using CSS. When you call `$("#example").jQCloud(...)`, the containing element is given a CSS class of "jqcloud", allowing for easy CSS targeting. The included CSS file `jqcloud.css` is intended as an example and as a base on which to develop your own custom style, defining dimensions and appearance of words in the cloud. When writing your custom CSS, just follow these guidelines:

* Always specify the dimensions of the container element (div.jqcloud in jqcloud.css).
* The CSS attribute 'position' of the container element must be explicitly declared and different from 'static'.
* Specifying the style of the words (color, font, dimension, etc.) is super easy: words are wrapped in `<span>` tags with ten levels of importance corresponding to the following classes (in descending order of importance): w10, w9, w8, w7, w6, w5, w4, w3, w2, w1. 


## Examples

Just have a look at the examples directory provided in the project or see a [demo here](http://www.lucaongaro.eu/demos/jqcloud/).


## Gallery

Some creative examples of jQCloud use are:

* http://www.politickerusa.com/trends/ uses jQCloud to show trends in US politicians' tweets.
* http://www.turtledome.com/noisy/ shows you which of the people you follow on Twitter tweets the most.

If you happen to use jQCloud in your projects, you can make me know (just contact me on [my website](http://www.lucaongaro.eu)): I'd be happy to add a link in the 'gallery', so that other people can take inspiration from it.


## Contribute

Contributes are welcome! To setup your build environment, make sure you have Ruby installed, as well as the `rake` and `erb` gems. Then, to build jQCloud, run:

```
rake build
```

The newly-built distribution files will be put in the `jqcloud` subdirectory.

If you make changes to the JavaScript source, to the README, to examples or to tests, make them to .erb files in the `src` subdirectory: changes will be reflected in the distribution files as soon as you build jQCloud. Also, if you send me a pull request, please don't change the version.txt file.

## Changelog

0.2.10 Fix bug occurring when the container element has no id

0.2.9 Add dataAttributes option (thanks again to [cham](https://github.com/cham)) and fix bug when weights are all equal (thanks to [Grepsy](https://github.com/Grepsy))

0.2.8 Add possibility to specify custom classes for words with the custom_class attribute (thanks to [cham](https://github.com/cham))

0.2.7 Add possibility to draw rectangular-shaped clouds (an idea by [nithin2e](https://github.com/nithin2e))

0.2.6 Fix bug with handlers, add nofollow option (thanks to [strobotta](https://github.com/strobotta)) and word callbacks.

0.2.5 Add possibility to bind event handlers to words (thanks to [astintzing](https://github.com/astintzing))

0.2.4 Option randomClasses can be an array of classes among which a random class is selected for each word

0.2.3 Add option randomClasses, allowing for random CSS styling (inspired by issue about vertical words opened by [tttp](https://github.com/tttp))

0.2.2 CSS improvements (as suggested by [NomikOS](https://github.com/NomikOS))

0.2.1 Optimization and performance improvements (making 0.2.1 around 25% faster than 0.2.0)

0.2.0 Add configuration options, passed as the second argument of jQCloud (include ideas proposed by [mindscratch](https://github.com/mindscratch) and [aaaa0441](https://github.com/aaaa0441))

0.1.8 Fix bug in the algorithm causing sometimes unbalanced layouts (thanks to [isamochernov](https://github.com/isamochernov))

0.1.7 Remove duplicated `</span>` when word has an URL (thanks to [rbrancher](https://github.com/rbrancher))

0.1.6 JavaScript-friendly URL encode 'url' option; Typecast 'weight' option to float (thanks to [nudesign](https://github.com/nudesign))

0.1.5 Apply CSS style to a "jqcloud" class, automatically added (previously an id was used. Again, thanks to [seanahrens](https://github.com/seanahrens))

0.1.4 Fix bug with multiple clouds on the same page (kudos to [seanahrens](https://github.com/seanahrens))

0.1.3 Added possibility to pass a callback function and to specify a custom HTML title attribute for each word in the cloud

0.1.0 -> 0.1.2 I just started this tiny project, only minor improvements and optimization happened
