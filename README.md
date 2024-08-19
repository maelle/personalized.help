
<!-- README.md is generated from README.Rmd. Please edit that file -->

# personalized.help

<!-- badges: start -->
<!-- badges: end -->

The goal of personalized.help is to demonstrate how you can create some
content of a manual page in an R package *on the fly*, inspired by a
conversation in the rOpenSci slack workspace.

## Installation

You can install the development version of personalized.help like so:

``` r
pak::pak("maelle/personalized.help")
```

## Example

Imagine I, Maëlle Salmon, load the help page for thing. Notice how the
help page says “hello” to *me*.

``` r
library(personalized.help)
?thing
```

``` html
 <h2>Function returning something</h2>

<h3>Description</h3>

<p>Hello Maëlle Salmon!
</p>


<h3>Usage</h3>

<pre><code class='language-R'>thing()
</code></pre>


<h3>Value</h3>

<p>Something!
</p>


<h3>Examples</h3>

<pre><code class='language-R'>thing()
</code></pre>

</main>

</div>
</body></html> 
```

Now what if you, Awesome Person, do that? To trick whoami to think I am
you I am setting the `FULLNAME` environment variable to your name.

The help page says hello to you using your name!

``` r
library(personalized.help)
withr::local_envvar(FULLNAME = "Awesome Person")
?thing
```

``` html
 <h2>Function returning something</h2>

<h3>Description</h3>

<p>Hello Awesome Person!
</p>


<h3>Usage</h3>

<pre><code class='language-R'>thing()
</code></pre>


<h3>Value</h3>

<p>Something!
</p>


<h3>Examples</h3>

<pre><code class='language-R'>thing()
</code></pre>

</main>

</div>
</body></html> 
```

## How it works

The source of the help of `thing()` has, in lieu of an actual
description, this:

``` r
#' \Sexpr[results=rd,stage=render]{personalized.help:::generate_help()}
```

where `generate_help()` is this internal function:

``` r
generate_help <- function() {
  sprintf("Hello %s!", whoami::fullname(fallback = "human"))
}
```

`generate_help()` gets executed every time the help page is loaded. It
gets the name of the user using the whoami package.

[Real example on
CRAN](https://github.com/cran/stevedore/blob/74b63bfcff2af200367fbd78febfd2d36855d500/R/help.R#L4).
[More
examples](https://github.com/search?q=\Sexpr%5Bresults%3Drd%2Cstage%3Drender%5D+user%3Acran&type=code&ref=advsearch).
