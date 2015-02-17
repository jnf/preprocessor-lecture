# Preprocessors!
###### _Hard to spell, but fun to use._
Hi! I want to tell you about some of my favorite kinds of tools.

## What are preprocessors?

Preprocessor is a broad term that covers several kinds of technologies. In
general, it refers to a program or methodology that converts, conforms, or
otherwise manipulates one set of data to be the input for another set of data.

You've already used at least one kind of preprocessor--ERB--which is a special
DSL/amalgamation of HTML and Ruby that is _processed_ (by Rails) to _produce_
HTML that is rendered by a browser. This two-step process is the hallmark of a
preprocessor; one thing is _processed_ and used to _produce_ another.

So a preprocessor is just a thing--a program, function, or tool--which produces
an output that is the input for something else.

## Why talk about them now?

With internships starting soon, there's a good chance that you'll be seeing some
preprocessors in the wild, if you haven't already. The goal today is to
understand what preprocessors do and why we use them.

### What kind of problems do they solve?

Here’s four things that preprocessors are really good at. Given that the term is
so general, these don’t apply to all preprocessors, but, with our focus on web
dev, this is a pretty good list.

- Provide better visual structure
- Encourage better practices
- Reduce boilerplate
- Reduce cognitive load

We are going to look at code for each of these, but first let me introduce a
preprocessor that's, like, my best friend.

### Haml - HTML Abstraction Markup Language
###### How Awesome Markup Looks

Let's talk about Haml. It’s a ruby gem that replaces erb as the preprocessor for
producing html. My chief complaint of erb is that it does not do enough to
eliminate the pointless repetition inherent to HTML. You can find lots of docs
and examples at [haml.info](http://haml.info). I love markup. I think it can be beautiful.
I’ve heard folks say that Haml goes too far, but I believe Haml doesn’t go too
far enough. Let’s look at some code. . .

#### best_names_for_cats.html

```html
<!-- Please don't do this -->
<div id="cats"><h2 class="cats-title">The Best Names For Cats</h2>
<ol class="cats-list"><li class="cats-name">Pickles</li><li class="cats-name">Raquel Welch's Grape Jam</li>
<li class="cats-name">Grand Lord Snugglewumps</li><li class="cats-name">Thunderpaw the Destroyer</li><li class="cats-name">Katy Purry</li></ol>
</div>


<!-- Good, but very repetitive and difficult to maintain -->
<div id="cats">
  <h2 class="cats-title">The Best Names For Cats</h2>
  <ol class="cats-list">
    <li class="cats-name">Pickles</li>
    <li class="cats-name">Raquel Welch's Grape Jam</li>
    <li class="cats-name">Grand Lord Snugglewumps</li>
    <li class="cats-name">Thunderpaw the Destroyer</li>
    <li class="cats-name">Kitty Purry</li>
  </ol>
</div>
```

This is just HTML. top example - very poorly formatted. I see this all the time
with HTML. The spec is written to allow this kind of terrible smushing
(structure from tag nesting, not file structure), and it is a nightmare to
maintain. Syntax highlighting helps, but not enough.

bottom example - better. About the best formatting you can get with plain HTML.
Lots of redundant information. Updating it is error prone and slow; for example,
changing `cats-name` to `cats-title` has to happen five times. Why is this
example better? Well. . .

##### We can do better

Let’s look at what preprocessors are supposed to do again: better visual
structure, encourage good practices, reduce boilerplate, and reduce cognitive
load.

The second html examples gives us everything except reducing boilerplate, just
with good use of whitespace and indentation, but there’s so much boilerplate
here. Surely ERB is the answer here, right? I mean, how often are we writing
static html anyway? Look at those cat names! That’s an array and a loop! Right?
ERB will fix this!

#### best_names_for_cats.html.erb

```erb
<!-- Pretend we got this from a controller or something -->
<% names = ["Pickles", "Raquel Welch's Grape Jam", "Grand Lord Snugglewumps",
            "Thunderpaw the Destroyer", "Kitty Purry"] %>

<!-- Technically correct, but really hard to read -->
<div id="cats">
<h2 class="cats-title">The Best Names for Cats</h2>
<ol class="cats-list"><% names.each do |name| %><li class="cats-name"><%= name %></li><% end %></ol></div>

<!-- The best of a bad bunch; using whitespace to increase legibility -->
<div id="cats">
  <h2 class="cats-title">The Best Names for Cats</h2>
  <ol class="cats-list">
    <% names.each do |name| %>
      <li class="cats-name"><%= name %></li>
    <% end %>
  </ol>
</div>
```

Look at that first example. Technically works, but what a mess. ERB is
totally ok with us making ugly markup. There’s no penalty, besides sadness, for
making a mess.

So let’s do the same cleanup with whitespace and indentation. Easier to read
now, and we actually reduce a little bit of boiler plate, right? We use an array
and a loop to make our list items. Now it’s easy to update those class names.

But really. Look at all the unnecessary stuff still in there. What if we really
wanted to reduce boilerplate. Here’s the same code written in Haml.

#### best_names_for_cats.html.haml

```haml
- names = ["Pickles", "Raquel Welch's Grape Jam", "Grand Lord Snugglewumps", "Thunderpaw the Destroyer", "Kitty Purry"]

#cats
  %h2.cats-title The Best Names for Cats
  %ol.cats-list
    - names.each do |name|
      %li.cats-name= name

/ No:
  / - wrapping ruby tags (use - for code and = for output instead of <% %>)
  / - closing html tags (</h2>,</li>,</ol>)
  / - unnecessary tags; haml assumes `div` unless you say otherwise
  / - redundant attributes (use # for id and . for class)
  / - and look at these minimal requirements for comments!
/ After doing all that, we're left with only the most important bits
```

##### It's so pretty!

- Provide better visual structure: Haml uses indentation to establish nesting and structure
- Encourage better practices: indentation and whitespace are good practice; poorly formatted html won’t compile (which I maintain is a feature).
- Reduce boilerplate: we are barebone here; only thing left are things that actually change or matter
- Reduce cognitive load: all of this works together to make the code small and easy to understand, but just as powerful.


### What kind of problems can preprocessors introduce?

Ok, so we’ve talked about some problems that preprocessors can solve, and you
watched me profess my unending affection for Haml. But nothing is perfect and
introducing a preprocessor into a project can come with some pitfalls.

__note__ from this point forward, most of the content is in my keynote presentation.

- Obfuscate errors:  we’ll look at some code to demonstrate, but every step between you and the problems is noise
- Yes another set of patterns and quirks - potential source of confusion, adds some cognitive load

###### first two are tech oriented; next two are people oriented, and I want to talk about people stuff first.

- Raises bar of entry: collaborators have to know or be comfortable learning your tool chain
- Arguing - people love to argue on the internet; those arguments detract from the work


