LoveKnowsNoBorders
==================

A translation workflow and a set of filters for the translation of the LÖVE wiki.

# Things a translator must be aware of beforehand

MediaWiki and Lua have its peculiarities. While this project tries to get them all out of the way of the translator, sometimes it's not possible. The cases described below require special attention.

## System requirements

**TO DO**

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

## Sections of the wiki

**WARNING: this part is still a draft**

One should not try to translate the whole wiki all at once. Take into consideration that it's the kind of work done by a handful volunteer translators, in their spare time, with the possibility of the original English wiki being updated at any time. All these factors can create confusion, so it's best to divide the wiki in small sections to keep things organized. There are of course other measures to counter those problems, but they'll be mentioned further on.

Only one translator may be working on a single section at once. Here is a draft of how the wiki may be divided.

* Each module with its functions

* Each LÖVE object type with its functions

* All Lua object types

* All enums

* Main/general pages

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

### Search & replace language setting

This projects contains a filter (okf_regex@lovewiki.rfpm), a set of search & replace steps (searchandreplace.utf8, which we'll refer to as S&R) and a Lua script (stripper.lua). Open the S&R in a text editor, you'll see that there are several occurrences of *LANGUAGE*. You must replace those occurrences with the name of the languague you're translating to (see *Languages of the wiki*). 

### The repository

**I have to come back to this when the Portuguese repo is done, okay? okay!**

It should have a status page, containing a list of the sections of the wiki, and their status, either not translated, being translated (plus translator's name and when the translation started) or translated (plus the translator's name and the date of when the translation was finished).

For our purposes, a section or page is considered not translated if it hasn't been translated through this workflow, that is, if there's a previous translation, it's still considered not translated. Setting the section as being translated is the first thing a translator does, even before downloading the pages. It's only considered translated when the translator has finished uploading all pages and cheked for updates that might have been made in the meantime.

### The templates

The wiki has some templates that show some sort of warning in English, called oldin, newin and newobjectnotice. It's necessary to create an alternate version of those templates for each language. It's not strictly necessary to translate the documentation of the template, but it's nice.

The newobjectnotice template is pretty straightforward. You should got to the following address: https://www.love2d.org/wiki/Template:newobjectnotice_(LANGUAGE), but replacing LANGUAGE with the appropriate language name (see *Languages of the wiki*). The wiki will probably tell you that that page doesn't exist, so you need to create it. You should copy the content from https://www.love2d.org/wiki/Template:newobjectnotice, paste it into the new page and translate the part that says *This function can be slow if it is called repeatedly, such as from [[love.update]] or [[love.draw]]. If you need to use a specific resource often, create it once and store it somewhere it can be reused!*. Don't touch anything else. Notice that there are links. If you've read *Wiki links* then you'll know how to deal with them.

Now, the oldin and newin templates have complications. They show not one, but two texts. The first text is much like the one in newobjectnotice. You can go ahead and create a translated version of them like you did to newobjectnotice. The text to be translated from https://www.love2d.org/wiki/Template:oldin is *Removed in LÖVE {{{1}}}*, and in https://www.love2d.org/wiki/Template:newin it's *Available since LÖVE {{{1}}}*. When the template is used, *{{{1}}}* will be replaced by a LÖVE version number, such as 0.9.1.

It's the second part of newin and oldin that's complicated. When used, if a *text* parameter is defined, then that text is displayed. Otherwise, it generates text based on other parameters. The text generation is already complex for English, and it could be hell to recreate for other languages. This translation workflow takes the much simpler route of defining the *text* parameter in **every** occurrence of those templates. The translator won't have to worry about doing that manually, it'll all be automated.

If you wish to translate the doc of each template, just add */doc* to the template's address. But again, this is not necessary.

## Translating

You should pick a section of the wiki to translate (see *Sections of the wiki* and *The repository*), go to the repository status page and mark that section as being translated by you.

### Exporting

Make a list of the pages in the the section you've chosen (use their internal wiki names, not their addresses). Go to the wiki's [Special:Export](http://www.love2d.org/wiki/Special:Export) page, paste the list in the big box and click *Export*. You should receive an XML file containing all the listed pages.

### Stripping

**WARNING: only tested on Linux. On Windows, it might be necessary to wrap the filepath in double quotes.**

If the selected section of the wiki hadn't been translated before (using this workflow), then you can skip to the next step. Otherwise, you'll only want to translate pages that have been updated since the last translation of that section, and that's what the stripper.lua script does. When used on the XML exported by the wiki, it cuts off all unnecessary pages, leaving only those that have been updated since the last translation. You have to have Lua (not just LÖVE) installed in your system to be able to use it.

To use the script you must use the command line or terminal and go to the page where the script is, then type:

```
lua stripper.lua filepath date
```

...where filepath is the location of the XML (for example *~/Downloads/LOVE-20140509191706.xml* or *C:\Downloads\LOVE-20140509191706.xml*) and date is when the last translation of that section was finished, formatted as yyyymmdd (year, month, day). You should get a message telling how many pages were kept and how many were removed. It also tells you the name of the new XML file generated in the same folder as the original one.

If it finds that no page has been updated (0 pages kept), then there's nothing to translate. You can change that section's status to translated and change the date of the last translation as you would do if you had translated it.

### Search & Replace

Using Okapi's Search & Replace we can strip away all the XML from the pages (making them simple text) and do a lot of automatic replacements (if you wish to know exactly what it does, all the explanations can be read by opening searchandreplace.utf8 in a text editor).

* Start Okapi Rainbow

* Click *Add Documents* (the green circle with a plus sign)

* Select the XML file (the original one if this is a first translation of the section, or otherwise the stripped one) and click *OK*, the file will show up in the Input List

* Right-click the file in the Input List and click Edit Document Properties

* In both encoding fields, type *utf8* and click *OK*

* In the top panel, click *Utilities* and then *Search and Replace **without** Filter*

* Click *Import*, find the searchandreplace.utf8 file and click *OK*

* Click *Execute*

A new file will be generated, its name will be something like nameoftheoriginalfile.out.xml.

### Creating the translation kit

Still using Okapi Rainbow:

* Clear the Input List

* Add the file created in the previous step to the List

* Edit its properties - again, set encodings to utf8, but now you must also specify the filter. Click *More*, a new window will open up. Under *Custom Configurations*, specify the folder where okf_regex@lovewiki.fprm filter is. It will now show up in the bottom of the big filter list. Click on it an then click *Select* and *OK*.

* Click *Utilities* and *Translation Kit Creation*. Under *Package Format*, choose the appropriate one (OmegaT project, if you're using that CAT tool). Under *Output Location*, choose a folder and a name for the package. Click *Execute* and wait a few seconds.

This creates a translation kit that can be opened with CAT tools.

### Translating

If this workflow has been used at least once for a given language, then a translation memory and a glossary must have been created. You should add them to your project/translation kit before starting the translation.

If you're using OmegaT, the glossary should be put in the glossary folder. The compatibility of the glossary with other CAT tools hasn't been tested, but conversion shouldn't be too hard to do.

There are three translation memory files. One is compatible only with OmegaT, while the others are level 1 and level 2 tmx standard. If using OmegaT, copy the appropriate one into the project's tm folder (there's no need to copy all of them). You may even want to put it into tm/auto instead. The difference is that OmegaT will always ask you to check translations from the tm folder, while translations from tm/auto will be approved automatically.

Now you can open and translate the project as you normally would. If you've never used OmegaT, it shows a short tutorial when you start it. Ctrl+Shift+G adds an entry to the glossary. When done translating, validate the tags (Ctrl+T) and generate the translated file and translation memory files (Ctrl+D).

Notice that some segments are just web addresses - those are there to serve as a reference and shouldn't be changed. In fact, you should copy that link and open it in your browser - it'll be helpful if you're not sure about where a certain segment belongs. Also, some pages may have previous, "manual" translations. We don't want to waste the efforts of those who came before us, so it's interesting to reuse parts of those translations, copying and pasting them into OmegaT (taking their quality in consideration, of course).

### Merging the translation

The translation has to be merged with the original file, but that should be pretty simple by now. Start Okapi Rainbow. Inside the translation kit folder, there's a file called manifest.rkm - add it to the Input List and set its encodings to utf8. Click *Utilities*, *Translation Kit Post-Processing*, *Execute* and *Continue*. That'll create a *done* folder in the translation kit folder, which contains the the translated wiki pages.

### Uploading and reviewing

Open the translated file in the *done* folder. It contains all translated pages in the wiki markup format, separated by five newlines, and with a link to the translated page 3 newlines above the body of text. Use the link to go to each page and edit it. If the page doesn't exist, it will be created. Paste the translated content, preview and save.

Use the preview phase to, well, review the translation and formatting. If you find a mistake, you should go back to OmegaT to fix it, making sure the correction will be saved in the translation memory. If you find a mistake caused by a failure in the filters, you should submit a bug with as much information as possible, or even try to fix it yourself.

You might feel tempted to save without previewing, as it could save you a few clicks and you can always correct any mistakes. DON'T DO IT! It makes a mess in the history of the page.

### Checking for updates

Some of the original pages might have been updated while you were translating them, so you should download them again and use the stripper script to check for updates, using the day you finished uploading pages as the date. If you find changes, you should make a new translation kit from the new exported file, using the translation memory from the first one in automatic mode, so you'll only need to deal with untranslated segments. Upload and review the changed pages.

### Uploading glossary and translation memories
