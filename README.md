helm-words
==========

Customizable Helm extension for easy looking up words in various online/offline dictionaries and thesauri.

The extension is using `aspell` to look up suggested word completions in multiple `aspell` dictionaries installed in the system. Once a word is selected, it can be looked up using a dictionary directly within Emacs (e.g. using the [dictionary.el](http://www.emacswiki.org/DictMode) package) or using an online dictionary specified by an URL and displayed in your browser. Those dictionaries can be language specific and `aspell` is used to suggest the right language. Generally, any Emacs command that accepts a word as an argument or any URL can be used.

Moreover, the package provides an emacs integration with one particular online dictionary (http://dict.pl) and allows for looking up Polish words and displaying the results directly within Emacs.


Usage
-----
To use, just invoke `helm-words` and start typing a word. The word will be matched to all specified dictionaries as shown below:
![](https://github.com/pronobis/helm-flyspell/blob/master/images/screenshot1.png)

Once a selection is made (for a word and a language), a dictionary can be invoked using the persistent action or by selecting from one of the additional actions:
![](https://github.com/pronobis/helm-flyspell/blob/master/images/screenshot2.png)


Configuration
-------------

First, you should select the `aspell` dictionaries that should be used for looking up word suggestions. Then, for each of those, you can specify a separate set of actions that can be taken for a selected word. This can be configured using the `helm-words-dictionaries` variable. You can modify it by either setting it in your `~/.emacs` file, or by using `customize-group` and selecting `helm-words`.

The general structure of the variable value is a list of languages that will be used by `aspell` to look-up words in which each item is specified by the language code and a list of configuration options. The configuration for each language is another list in which we first specify the full name of the language, then a set of actions to take on each word. In the helm spirit, the first action is the default one and is also used as a persistent action. Actions can be defined either by an URL in case of which "%s" will be replaced by the selected word, or by providing an Emacs function that takes one argument, the selected word.

The default value of the variable is shown below and can also serve as an example:
```
'(
   ("en". ("English"
          ("Emacs Dictionary" dictionary-search)
          ("English Dictionary" "http://dictionary.reference.com/browse/%s?s=t")
          ("Thesaurus" "http://www.thesaurus.com/browse/%s?s=t")
          ("English-Polish Dictionary" "http://portalwiedzy.onet.pl/tlumacz.html?qs=%s&tr=ang-pol&x=38&y=9")
          ))
  ("pl" . ("Polish"
           ("Emacs Dict.pl" helm-words-dict-pl-search)
           ("Polish Dictionary" "http://sjp.pwn.pl/szukaj/%s.html")
           ("Thesaurus" "http://www.synonimy.pl/synonim/%s/")
           ("Polish-English Dictionary" "http://portalwiedzy.onet.pl/tlumacz.html?qs=%s&tr=pol-ang&x=39&y=8")))
  ))
```

In the example above, we will use two `aspell` dictionaries: English ("en") and Polish ("pl"). For, each of them we define a set of actions that can be invoked on a word. The first, default action is specified by an Emacs function. It uses the [dictionary.el](http://www.emacswiki.org/DictMode) for English and an internal parser command for the (http://dict.pl) dictionary. All following actions open a browser on a specified dictionary/thesaurus URL.
