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

## Languages of the wiki

The LÖVE wiki is already partially translated into a handful of languages, which are listed in http://www.love2d.org/wiki/Help:i18n. Most pages have the i18n in the bottom, which automatically dislays links to versions of that page in all available languages. If a language is not listed, then you must ask an administrator to add it.

English pages have simple names, such as *love.audio*, while translated versions are named, for example, *love.audio (LANGUAGE)*, where LANGUAGE is the name of the language as shown in the *Localized* column of the list referred to above. That variation in the internal page name is reflected in the page's address, for example, https://www.love2d.org/wiki/love.audio and https://www.love2d.org/wiki/love.audio_(Português) (notice how an underline replaces the space).

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

### The templates

The wiki has some templates that show some sort of warning in English, called oldin, newin and newobjectnotice. It's necessary to create an alternate version of those templates for each language. It's not strictly necessary to translate the documentation of the template, but it's nice.

The newobjectnotice template is pretty straightforward. You should got to the following address: https://www.love2d.org/wiki/Template:newobjectnotice_(LANGUAGE), but replacing LANGUAGE with the appropriate language name (see *Languages of the wiki*). The wiki will probably tell you that that page doesn't exist, so you need to create it. You should copy the content from https://www.love2d.org/wiki/Template:newobjectnotice, paste it into the new page and translate the part that says *This function can be slow if it is called repeatedly, such as from [[love.update]] or [[love.draw]]. If you need to use a specific resource often, create it once and store it somewhere it can be reused!*. Don't touch anything else. Notice that there are links. If you've read *Wiki links* then you'll know how to deal with them.

Now, the oldin and newin templates have complications. They show not one, but two texts. The first text is much like the one in newobjectnotice. You can go ahead and create a translated version of them like you did to newobjectnotice. The text to be translated from https://www.love2d.org/wiki/Template:oldin is *Removed in LÖVE {{{1}}}*, and in https://www.love2d.org/wiki/Template:newin it's *Available since LÖVE {{{1}}}*. When the template is used, *{{{1}}}* will be replaced by a LÖVE version number, such as 0.9.1.

It's the second part of newin and oldin that's complicated. When used, if a *text* parameter is defined, then that text is displayed. Otherwise, it generates text based on other parameters. The text generation is already complex for English, and it could be hell to recreate for other languages. This translation workflow takes the much simpler route of defining the *text* parameter in **every** occurrence of those templates. The translator won't have to worry about doing that manually, it'll all be automated.

If you wish to translate the doc of each template, just add */doc* to the template's address. But again, this is not necessary.
