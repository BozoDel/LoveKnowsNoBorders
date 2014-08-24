LoveKnowsNoBorders
==================

A translation workflow and a set of filters for the translation of the LÖVE wiki. It may seem like overkill, but it can save a lot of work, especially in the future.

For all instructions in this page, the [Portuguese](https://github.com/BozoDel/LoveKnowsNoBorders-PT) language implementation, being the first, should serve as an example.

# Currently implemented languages

[Portuguese](https://github.com/BozoDel/LoveKnowsNoBorders-PT)

# Software needed

Many of the tools you'll need require an up-to-date Java Runtime Environment. The Oracle implementation is recommended over open ones. You'll need [Okapi Rainbow](http://www.opentag.com/okapi/wiki/index.php?title=Rainbow) for extracting the text for translation, and [SuperTMXMerge](https://github.com/amake/SuperTMXMerge/releases) for merging translation memories. Minor tasks may require a simple text editor, but not notepad (all the cool Windows kids use [Notepad++](http://notepad-plus-plus.org/)). The whole purpose of this workflow is to be able to use a CAT (Computer Assisted Translation) tool, so you'll need one. Any one that can deal with open standards should do, but see our recomendation below.

## CAT tool recommendation

This explanation will assume that you're using [OmegaT](http://omegat.org/), which is free and open source (and very good and growing in popularity). But you should be able to use other CAT tools, making a few or no changes. When you start OmegaT, it shows a *Learn to use OmegaT in 5 minutes!* screen that should cover almost everything you need to know.

In the OmegaT download page, there's two main versions: Standard and Latest. I highly recommend you to download the Latest, as it's very stable and has many features that the Standard doesn't - one of them is showing useful hints in the comments pane (since 3.1.0).

By default, OmegaT only shows the main, the glossary and the fuzzy matches panes. You'll probably want to enable the comments pane too (if you're using 3.1.0 or later version). Just click "Comments", the undock icon, and drag it wherever you like.

In *Options > Editing Behavior*, it's best to turn on the following options: *Insert the best fuzzy match*, *Allow translation to be equal to source*, *Go To Next Untranslated Segment stops when there is at least one alternative translation* and *Validate tags when leaving a segment*.

The Okapi filter already does a good job segmenting most text, but leaves snippets of code whole. So it's best to disable *Sentence-level Segmenting* in most cases. You have to configure this in *Project > Properties* every time you start a new OmegaT project.

## Glossary entries

There'll be many terms you'll want to add to the glossary, so that they are translated consistently by yourself and other translators. In OmegaT, the shortcut to create a glossary is Ctrl+Shift+G, which opens a panel containing three boxes, one for the source language term, one for the target language, and the third, optional, for clarifications (which are always welcome). If a single term has to be translated differently depending on context, make two different entries, with clarifications. Remember that the glossary is a simple tab-delimited text file, so you can edit it manually if necessary. And it's always a good idea to write the clarifications in English, so that they can serve as guidance for translators of other languages.

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

...and you'll get [OpenAL](http://en.wikipedia.org/wiki/OpenAL).

## Languages of the wiki

The LÖVE wiki is already partially translated into a handful of languages, which are listed in http://www.love2d.org/wiki/Help:i18n. Most pages have the i18n template in the bottom, which automatically displays links to versions of that page in all available languages. If a language is not listed, then you must ask an administrator to add it.

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

## On the translatability of LÖVE and Lua specific terms

Changes in some wiki templates (ListingFields and param, for the record) have enabled us to choose display names for links in all situations, so that terms in page names can be translated more consistently. For example, before those changes, the term *Source* would be translatable in some situations, but not all - but fortunately that's not the case anymore. We can say that limitations to the translation imposed by the structure of the wiki have been almost completely overcome.

However, there are other limitations imposed by LÖVE and Lua (and are common to programming languages as libraries in general). It is possible to make a clear distinction between terms affected by those limitations and those that aren't:

* Names of objects/types and enums are not really "part of the programming language" and can be translated

* Names of functions and LÖVE modules are "part of the programming language" and can't be translated

* Some page names are composed of an object name and a function name, for example, *Source:getPitch*. *Source* here can be replaced by any variable of the source type, so it can be translated as an isolated object/type would. But *getPitch* is a function and can't be translated.

* Constants are more complicated. The Lua function *type* returns the type of a given variable. If that variable is a number, the function will return "number" as a string of text. In most parts of the documentation, we would translate *number* as *número* (in Portuguese), but it's not possible if it's a constant. The function *type* will never return "número", and assuming otherwise can induce errors. It may not seem such a huge problem for functions that *output* constants, but it definitely is for functions that take constants as *input*. Therefore words used as constants can't be translated when used as such (a single word in a string of text), but can be translated in descriptions and explanations.

## User-defined variable and funcion names in Lua

The use of non-ASCII characters in Lua code is sometimes possible, but it may or may not work depending on the user's system, so it's not recommended, and we must keep that in mind when translating variable and function names. Our concern is only with user defined functions, or the ones used in the examples, not the ones native to LÖVE/Lua (those should not be changed)

An example: a variable named *distance* would be translated into Portuguese as *distância*, but it contains one non-ASCII character. Replacing it, we get *distancia*.

That restriction doesn't affect the names of objects/types or enums, because they're not code.

# How to

A few things are necessary in order to implement this workflow for a given language. First, one has to setup some sort of repository (it can be a GitHub page like this one!) which will contain translation memories, a glossary and a list of parts of the wiki with their status.

Sections of the wiki will go through a first translation that probably won't need to be updated in a long time. However, when the time comes, "touching up" the translation will be easier. There aren't many differences between the workflow of the first translation of a section and that of the following ones, but distinctions will be made when necessary.

## Setup

### Search & replace language setting

This projects contains a filter (`okf_regex@lovewiki.rfpm`), a set of search & replace steps (searchandreplace.utf8, which we'll refer to as S&R) and a Lua script (stripper.lua). Open the S&R in a text editor, you'll see that there are several occurrences of *LANGUAGE*. You must replace those occurrences with the name of the languague you're translating to (see [Languages of the wiki](https://github.com/BozoDel/LoveKnowsNoBorders#languages-of-the-wiki)). 

### The repository

It should have a status page, containing a list of the sections of the wiki, and their status, either not translated, being translated (plus translator's name and when the translation started) or translated (plus the translator's name and the date of when the translation was finished).

For our purposes, a section or page is considered not translated if it hasn't been translated through this workflow, that is, if there's a previous "manual" translation, it's still considered not translated. Setting the section as being translated is the first thing a translator does, even before downloading the pages. It's only considered translated when the translator has finished uploading all pages and checked for updates that might have been made in the meantime.

The LÖVE wiki is under a [GNU Free Documentation License v1.3](https://gnu.org/copyleft/fdl.html), so, as inappropriate as it may seem, the translation memories also have to be under that license. [Here](https://github.com/BozoDel/LoveKnowsNoBorders-PT/blob/master/licenses.md)'s an example of how to show the license.

### The templates

The wiki has some templates that show some sort of warning in English, called oldin, newin, newobjectnotice and New_feature. It's necessary to create an alternate version of those templates for each language. It's not strictly necessary to translate the documentation of the template, but it's nice.

The newobjectnotice template is pretty straightforward. You should got to the following address: https://www.love2d.org/wiki/Template:newobjectnotice_(LANGUAGE), but replacing LANGUAGE with the appropriate language name (see [Languages of the wiki](https://github.com/BozoDel/LoveKnowsNoBorders#languages-of-the-wiki)). The wiki will probably tell you that that page doesn't exist, so you need to create it. You should copy the content from https://www.love2d.org/wiki/Template:newobjectnotice, paste it into the new page and translate the part that says *This function can be slow if it is called repeatedly, such as from [[love.update]] or [[love.draw]]. If you need to use a specific resource often, create it once and store it somewhere it can be reused!*. Don't touch anything else. Notice that there are links. If you've read [Wiki links](https://github.com/BozoDel/LoveKnowsNoBorders#wiki-links) then you'll know how to deal with them.

New_feature is similarly simple. The address now is https://www.love2d.org/wiki/Template:New_feature_(LANGUAGE), replacing LANGUAGE, and the part to be translated is *Available since LÖVE*. The documentation is the part between *noinclude* tags.

Now, the oldin and newin templates have complications. They show not one, but two texts. The first text is much like the one in newobjectnotice. You can go ahead and create a translated version of them like you did to newobjectnotice. The text to be translated from https://www.love2d.org/wiki/Template:oldin is *Removed in LÖVE {{{1}}}*, and in https://www.love2d.org/wiki/Template:newin it's *Available since LÖVE {{{1}}}*. When the template is used, *{{{1}}}* will be replaced by a LÖVE version number, such as 0.9.1.

It's the second part of newin and oldin that's complicated. When used, if a *text* parameter is defined, then that text is displayed. Otherwise, it generates text based on other parameters. The text generation is already complex for English, and it could be hell to recreate for other languages. This translation workflow takes the much simpler route of defining the *text* parameter in **every** occurrence of those templates. The translator won't have to worry about doing that manually, it'll all be automated.

If you wish to translate the doc of each template, just add */doc* to the template's address. But again, this is not necessary.

## Translating

You should pick a section of the wiki to translate (see [Sections of the wiki](https://github.com/BozoDel/LoveKnowsNoBorders#sections-of-the-wiki) and [The repository](https://github.com/BozoDel/LoveKnowsNoBorders#the-repository)), go to the repository status page and mark that section as being translated by you.

### Exporting

Make a list of the pages in the the section you've chosen (use their internal wiki names, not their addresses). Go to the wiki's [Special:Export](http://www.love2d.org/wiki/Special:Export) page, paste the list in the big box and click *Export*. You should receive an XML file containing all the listed pages.

### Stripping

**WARNING: only tested on Linux. On Windows, it might be necessary to wrap the filepath in double quotes.**

If the selected section of the wiki hadn't been translated before (using this workflow), then you can skip to the next step. Otherwise, you'll only want to translate pages that have been updated since the last translation of that section, and that's the usefulness of stripper.lua. When used on the XML exported by the wiki, it cuts off all unnecessary pages, leaving only those that have been updated since the last translation. You have to have Lua (not just LÖVE) installed in your system to be able to use it.

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

* In the top panel, click *Utilities* and then *Search and Replace __without__ Filter*

* Click *Import*, find the searchandreplace.utf8 file and click *OK*

* Click *Execute*

A new file will be generated, its name will be something like nameoftheoriginalfile.out.xml.

### Creating the translation kit

Still using Okapi Rainbow:

* Clear the Input List

* Add the file created in the previous step to the List

* Edit its properties - again, set encodings to utf8, but now you must also specify the filter. Click *More*, a new window will open up. Under *Custom Configurations*, specify the folder where `okf_regex@lovewiki.fprm` is. It will now show up in the big filter list. Click on it an then click *Select* and *OK*.

* Click *Utilities* and *Translation Kit Creation*. Under *Package Format*, choose the appropriate one (OmegaT project, if you're using that CAT tool). Under *Output Location*, choose a folder and a name for the package. Click *Execute* and wait a few seconds.

This creates a translation kit that can be opened with CAT tools. The hints mentioned in [CAT tool recommendation](https://github.com/BozoDel/LoveKnowsNoBorders#cat-tool-recommendation) are set as *notes* in the translation kit and should be displayed by most CAT tools - in OmegaT, it's shown in the Comments pane. In this case, little pieces of the wiki formatting are used as hints, so when in doubt about what it means, just check the source page, and you'll get used to it in no time.

### Translating

If this workflow has been used at least once for a given language, then a translation memory and a glossary must have been created. You should add them to your project/translation kit before starting the translation.

If you're using OmegaT, the glossary should be put in the glossary folder. The compatibility of the glossary with other CAT tools hasn't been tested, but conversion shouldn't be too hard to do.

There are three translation memory files. One is compatible only with OmegaT, while the others are level 1 and level 2 tmx standard. If using OmegaT, copy all of them into the project's tm folder. You may even want to put it into tm/auto instead. The difference is that OmegaT will always ask you to check translations from the tm folder, while translations from tm/auto will be approved automatically.

Now you can open and translate the project as you normally would. If you've never used OmegaT, it shows a short tutorial when you start it. Ctrl+Shift+G adds an entry to the glossary. When done translating, validate the tags (Ctrl+T) and generate the translated file and translation memory files (Ctrl+D).

Notice that some segments are just web addresses - those are there to serve as a reference and shouldn't be changed. In fact, you should copy that link and open it in your browser - it'll be helpful if you're not sure about where a certain segment belongs. Also, some pages may have previous, "manual" translations. We don't want to waste the efforts of those who came before us, so it's interesting to reuse parts of those translations, copying and pasting them into OmegaT (taking their quality in consideration, of course).

### Merging the translation

The translation has to be merged with the original file, but that should be pretty simple by now. Start Okapi Rainbow. Inside the translation kit folder, there's a file called manifest.rkm - add it to the Input List and set its encodings to utf8. Click *Utilities*, *Translation Kit Post-Processing*, *Execute* and *Continue*. That'll create a *done* folder in the translation kit folder, which contains the translated wiki pages.

### Uploading and reviewing

Open the translated file in the *done* folder. It contains all translated pages in the wiki markup format, separated by five newlines, and with a link to the translated page 3 newlines above the body of text. Use the link to go to each page and edit it. If the page doesn't exist, it will be created. Paste the translated content, preview and save.

Use the preview phase to review the translation and formatting. If you find a mistake, you should go back to OmegaT to fix it, making sure the correction will be saved in the translation memory. If you find a mistake caused by a failure in the filters, you should submit a bug with as much information as possible, or even try to fix it yourself.

You might feel tempted to save without previewing, as it could save you a few clicks and you can always correct any mistakes afterwards. DON'T DO IT! It makes a mess in the history of the page.

### Checking for updates

Some of the original pages might have been updated while you were translating them, so you should download them again and use the stripper script to check for updates, using the day you *downloaded* the pages as the date. If you find changes, you should make a new translation kit from the new exported file, using the translation memory from the first one in automatic mode, so you'll only need to deal with untranslated segments. Upload and review the changed pages.

Notice that the stripper script has a little bit of leeway: it will notice if a change has been made to a source page in the same day you downloaded it, regardless of the time of the day.

### Merging glossary and translation memories

Now that the work is done, you should merge the glossary and the translation memories you created with the ones in the repository, so that they can be used by other translators. You probably have already downloaded them, but there's a chance that they were updated in the meantime, so download them again.

The glossary is a simple text file, with elements separated by tabs and newlines. If you're using the recommended latest version of OmegaT, the *glossary.txt* will already have the entries you added. Older versions of OmegaT or other CAT tools might create a separate glossary - in that case, all you have to do is copy the text of the new glossary and paste it in the bottom of the old one (with a decent text editor, not notepad).

The translation memories need to be combined using SuperTMXMerge. First of all, pay attention that level 1 TMXs have to be merged with level 1, level 2 with level 2, and OmegaT-specific with OmegaT-specific. You start the program by executing *SuperTMXMerge.sh* (on Linux - does it work on OSX?) or *SuperTMXMerge.bat* (on Windows). Click *Combine*, add both files and click *OK*. If there are any conflicts (that is, a source segment with two different translations), a new screen will pop up and you'll have to choose the most appropriate translation for each conflict, and then click *Done*. Name the new TMX file, choose an output folder and click *OK*.

Submitting the updated glossary and translation memories using git requires some technical knowledge that programmers (who are likely to be our translators) often have. So if you already know how to submit a pull request, you can skip the rest of this explanation.

If you want to learn how to git the right way, [this tutorial](https://try.github.io) is pretty quick and easy.

If you don't want to do it in the best way, there's also a sort-of-wrong way that wokrs. You should have a GitHub account (assuming the language-specific repo was setup on GitHub). Go to the repository and click *Fork* (upper-right corner). That should create a copy of the repository under your account, which you'll be redirected to. For each file you need to update, click on its name, click *Edit*, erase the content and paste the content from the corresponding file on your computer and click *Commit changes*. Once you updated all the files, click the *Pull Requests* icon, on the right, and then click on *New pull request*. A new page will show all the changes that were made, you should review them and, if they're ok, click *Create Pull Request*. You have just sent the files, so that the maintainer of the original repository can review and accept them. Congratulations!

# Contribute

If you want to edit the filter, the best option is to do it using Okapi Rainbow, though it's possible to edit it with a text editor. You can open the filter by clicking *Tools > Filter Configurations*, selecting the right folder in *Custom Configurations*, selecting the filter and clicking *Edit*. It's a [regex filter](http://www.opentag.com/okapi/wiki/index.php?title=Regex_Filter).

The S&R (documented [here](http://www.opentag.com/okapi/wiki/index.php?title=Search_and_Replace_Step)) can be opened with *Utilities > Search and Replace without Filter > Import*, but unlike the filter, it's probably best not to edit it with Rainbow, since the comments for each rule can only be seen inside a text editor, and they're lost if you export it with Rainbow.

In both cases, you need to be familiar with Java regex.
