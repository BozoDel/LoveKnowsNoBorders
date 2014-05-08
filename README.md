LoveKnowsNoBorders
==================

A translation workflow and a set of filters for the translation of the LÖVE wiki.

# Things a translator must be aware of beforehand

MediaWiki and Lua have its peculiarities. While this project tries to get them all out of the way of the translator, sometimes it's not possible. The cases described below require special attention.

## Wiki links

The most common type of link in the LÖVE wiki is a page name encapsulated by double brackets:

```
[[love.audio]]
```

And what the reader gets is [love.audio](https://www.love2d.org/wiki/love.audio). It's not too different with translated pages, the following example...

```
[[love.audio (Português)]]
```

...is shown as [love.audio (Português)](https://www.love2d.org/wiki/love.audio_(Português)). Notice that spaces in the wiki markup are converted to underscores in the page address.

However, you'll most likely want to change the way links are displayed, and you can do so by adding a pipe bar and after it the name of the link. This...

```
[[love.audio (Português)|love.audio]].
```

...will be seen as [love.audio](https://www.love2d.org/wiki/love.audio_(Português))

Simple links to external websites work by simply pasting the url. However, if you want to display that link with a different name, you use the url, a space, and the name, all between single brackets...

```
[http://en.wikipedia.org/wiki/OpenAL OpenAL]
```

...and you'll get [Openal](http://en.wikipedia.org/wiki/OpenAL).

## Variable an funcion names in Lua

The use of non-ASCII characters in Lua code is sometimes possible, but it may or may not work depending on the user's system, so it's not recommended, and we must keep that in mind when translating variable and function names. Our concern is only with user defined functions, not the ones native to LÖVE/Lua (those should not be changed)

An example: a variable named *distance* would be translated into Portuguese as *distância*, but it contains one non-ASCII character. Replacing it, we get *distancia*.

More on this in the what-to-translate-and-what-not-to-tranlate part (**TO DO**).

## On the translatability of LÖVE and Lua specific terms

Changes in some wiki templates (see wiki templates **TO DO**) have enabled us to choose display names for links in all situations, so that terms in page names can be translated more consistently. For example, before those changes, the term *Source* would be translatable in some situations, but not all - but fortunately that's not the case anymore. We can say that limitations to the translation imposed by the structure of the wiki have been almost completely overcome.

However, there are other limitations imposed by LÖVE and Lua (and are common to programming languages as libraries in general). It is possible to make a clear distinction between terms affected by those limitations and those that aren't:

* Names of objects/types and enums are not really "part of the programming language" and can be translated

* Names of functions and LÖVE modules are "part of the programming language" and can't be translated

* Some page names are composed of an object name and a function name, for example, *Source:getPitch*. *Source* here can be replaced by any variable of the source type, so it can be translated as an isolated object/type would. But *getPitch* is a function and can't be translated.

* Constants are more complicated. The Lua function *type* returns the type of a given variable. If that variable is a number, the function will return "number" as a string of text. In most parts of the documentation, we would translate *number* as *número* (in Portuguese), but it's not possible if it's a constant. The function *type* will never return "número", and assuming otherwise can induce errors. It may not seem such a huge problem for functions that *output* constants, but it definitely is for functions that take constants as *input*. Therefore words used as constants can't be translated when used as such (a single word in a string of text), but can be translated in descriptions and explanations.

## CAT tool recommendation

This explanation will assume that you're using [OmegaT](http://omegat.org/), which is free and open source (and very good and growing in popularity). But you should be able to use other CAT tools, making a few or no changes.

(**EXPAND THIS**)

# How to

A few things are necessary in order to implement this workflow for a given language. First, one has to setup some sort of repository (it can be a GitHub page like this one!) which will contain a translation memory, a glossary and a list of parts of the wiki with their status.

Sections of the wiki will go through a first translation that probably won't need to be updated in a long time. However, when the time comes, "touching up" the translation will be easier. There isn't much difference from the first translation of a section and following ones, but distinctions will be made when necessary.

## Setup


