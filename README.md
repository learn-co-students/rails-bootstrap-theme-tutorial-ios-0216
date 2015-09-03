# Using Bootstrap Themes to Style Your Rails App

## Introduction

_"Style is a way to say who you are without having to speak."_
– Rachel Zoe

As a developer, I sometimes find myself wanting to integrate a preexisting themes into my web applications. But the Rails asset pipeline can make the setup of these themes tricky. So, when I utilize Bootstrap themes, I tend to follow a specific progression in order to get my fancy themes up and running.

![Fancy Dog Gif](https://media.giphy.com/media/3bKZlDZjU0hR6/giphy.gif)

Let's take a look at this process, using the Rival theme from the WrapBootstrap marketplace.

## Requiring Dependencies

In order to use components from a theme, we'll need to successfully require all of the themes assets in the `vendor/assets/stylesheets` and `vendor/assets/javascripts` folders. Let's take a look at our theme in the browser:

![Rival Theme](/images/Screen Shot 2015-08-28 at 11.55.18 AM.png)

For this post, we'll use an unstyled Cat App, and I'd love to grab this sortable gallery for my kittens. If we right-click on the theme page, we can view the page source.

![View Page Source](/images/Screen Shot 2015-08-28 at 11.55.59 AM.png)

### CSS Dependencies

We're going to want to grab the CSS and Javascript files from the theme and require them __in the correct order__ in the Rails application. Let's start with the CSS files – we'll want to copy and paste the `link` tags into a new document in our text editor.

![CSS Files](/images/Screen Shot 2015-08-28 at 11.56.37 AM.png)

From there, let's modify the link tags paths to fit the CSS [Sprockets](https://github.com/sstephenson/sprockets) format, as demonstrated below. Not that the CSS Sprockets `require` does _not_ include the `.css` extension.

![Require CSS](/images/Screen Shot 2015-08-28 at 11.57.25 AM.png)

Now, we'll copy and paste the Sprockets into our `application.css` file.

![CSS Sprockets](/images/Screen Shot 2015-08-28 at 11.58.20 AM.png)

The last step is to ensure that all of the theme's CSS files are included locally in the `vendor/assets/stylesheets` folder. In Rival, we'll copy every file from `Site/assets/css`, as well as `Site/assets/boostrap/css/bootstrap.min.css`, into our `vendor/assets/stylesheets` folder. This will be different for each Bootstrap theme.

### Javascript Dependencies

We'll want to follow the same process for our Javascript `script` tags. Let's copy them into a new document.

![JS Files](/images/Screen Shot 2015-08-28 at 1.29.50 PM.png)

One thing that I immediately notice is that one of the `script` tags links to an external resource, not a local file. I don't know what to do with it yet, so we'll deal with it later. Note that JS Sprockets __do__ include the `.js` extension.

![JS Sprockets](/images/Screen Shot 2015-08-28 at 1.32.44 PM.png)

Like last time, we'll copy and paste the Sprockets into our `application.js` file.

![JS Sprockets 2](/images/Screen Shot 2015-08-28 at 1.52.27 PM.png)

This time, we'll copy every file from `Site/assets/js`, as well as `Site/assets/boostrap/js/bootstrap.min.js`, into our `vendor/assets/javascripts` folder. Now, if we look at the Javascript console, we get an error regarding the Google Maps API script that we skipped over before.

You've got to be kitten me!

![JS Google Error](/images/Screen Shot 2015-08-28 at 1.54.25 PM.png)

There's no way to require external resources with Sprockets, so let's add a `javascript_include_tag` to our `application.html.erb` file.

![JS Include Tag](/images/Screen Shot 2015-08-28 at 1.55.10 PM.png)

Now our kittens have no errors in the Javascript console! What a happy meow-ment!

![Kittens Required](https://media.giphy.com/media/SRO0ZwmImic0/giphy.gif)

## Integrating Components

### Portfolio

Now that all of our dependencies have been required, we can start grabbing components from the Bootstrap theme. If we Inspect Element for the portfolio, we can find the start of the portfolio code. We'll copy the line `<section class="module">`, and our clipboard will contain all of the code between the start and end of the `section` element.

![Grab Portfolio](/images/Screen Shot 2015-08-28 at 1.55.35 PM.png)

Let's paste that into a new document. Now, we have to decide which bits of that code we want to integrate into our views, and, more speficially, what needs to be dynamic. We'll want a portfolio item for each of our cat items, so let's copy the list items code.

![Portfolio Item](/images/Screen Shot 2015-08-28 at 1.55.54 PM.png)

Now, let's paste that into an ERB `each` loop:

![ERB](/images/Screen Shot 2015-08-28 at 1.56.10 PM.png)

We know that whenever we have `li` elements, we'll want a `ul` element to wrap the list items, so we'll grab that `ul` element, as well as the `section` and container `div`, from the theme code, and paste them before our ERB `@cats.each` loop.

![ERB Header](/images/Screen Shot 2015-08-28 at 1.56.24 PM.png)

And we'll close the `ul`, `div`, and `section` tags after our ERB `end`:

![ERB Footer](/images/Screen Shot 2015-08-28 at 1.56.53 PM.png)

If we refresh the page now, we'll get a gallery full of broken images. In order to see our adorable cats, we replace the `img` element with our ERB `image_tag`.

![ERB Images](/images/Screen Shot 2015-08-28 at 1.57.08 PM.png)

Refresh the page! __Kittens!__

![Cat Gallery](/images/Screen Shot 2015-08-28 at 1.57.26 PM.png)

BUT. Our kittens are still named "Corporate Identity". That's not a great name for one kitten, let alone many. So let's replace "Corporate Identity" with the identifier `Cat <%= cat.id %>`.

![ERB Names](/images/Screen Shot 2015-08-28 at 1.57.38 PM.png)

Now, we can hover over our kittens in the gallery to get their ID's!

![Cat 1](/images/Screen Shot 2015-08-28 at 1.58.05 PM.png)

### Filters

Now that the gallery is functioning properly, we can add filters. Let's copy the `<div class="row">` element, and paste it inside `<div class="container">`, before the `each` loop.

![Filters](/images/Screen Shot 2015-08-28 at 1.59.14 PM.png)

The theme's filters now show up in the browser. Let's modify them for our cats!

![Demo Filters](/images/Screen Shot 2015-08-28 at 1.59.31 PM.png)

The `data-filter` in each filter corresponds with the class name in our portfolio items. So, adding a filter with `data-filter=".animated"` means that clicking on it will keep all elements with the class `animated`.

![Animated Filters](/images/Screen Shot 2015-08-28 at 1.59.53 PM.png)

So, let's use ERB to add a class name of `animated` to each `cat` list item where `cat.animated` is true.

![Animated ERB](/images/Screen Shot 2015-08-28 at 2.00.30 PM.png)

Now, if we click on the "Animated" filter, we'll get some cat GIFs!

![Animated Gallery](/images/Screen Shot 2015-08-28 at 2.00.47 PM.png)

### Hooray!

![Happy Cat](https://media.giphy.com/media/ToCRja2miF3Xi/giphy.gif)
