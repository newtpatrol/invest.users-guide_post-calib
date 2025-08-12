Hello world!

These source files, in restructured text format, are designed to be compiled into stand-alone HTML documents
(and sometimes PDF) using the Sphinx documentation generator (http://sphinx.pocoo.org/).

See requirements.txt for the python dependencies required to build the documentation.

Some source files reference tables from the [invest-sample-data](https://bitbucket.org/natcap/invest-sample-data/src/master/) repo.
A clone of `invest-sample-data` must exist in the top level of this repo before you build the documentation.
Execute the following command to clone `invest-sample-data` and check out the correct revision:

`make invest-sample-data`

Execute the following command to build HTML documentation from the reStructuredText source:

`make html`

Then find the html documents in `build/html` and view them in a web browser to evaulate for correctness.

## Branching & Development Guidelines

Edits to the User Guide that pertain to the currently released version of InVEST can be made directly on this repo's `main` branch.

Edits that pertain to a yet-to-be-released feature of InVEST should be made on the corresponding `release/X.X` branch in this repo. That branch should be merged into `main` at the same time as the corresponding invest release.

## Style Guidelines

Our [style guide google doc](https://docs.google.com/document/d/1BHwHDu_I-x0s_2GsbUb4rfVmXMkl7kl97sx2suBTLh8/edit?usp=sharing) is actively being developed.
For anything not listed in our style guide, follow the [Google developer documentation style guidelines](https://developers.google.com/style).

## Requirements

`requirements.txt` is the complete list of requirements needed to build the user's guide.
However, `pip install -r requirements.txt` will fail in a fresh environment because `natcap.invest` depends on `gdal`, which cannot be `pip install`ed unless the GDAL library and headers already exist on the system.

Since the GDAL library and headers can be installed with `conda`, an `environment.yml` is included that will install GDAL with `conda`, and then the rest of the requirements with `pip`.

## Internationalization
Nothing special needs to be done when routinely editing the source RST files. See the InVEST internationalization README for more background.

There are three components of the user's guide that have separate mechanisms of translation: text in the source RST files; text provided by Sphinx such as "Note" or "Warning"; and text that's imported from `natcap.invest` by the custom `:investspec:` role.

---
All commands below should be run from the project root directory, and `<LANG>` should be replaced with the appropriate ISO 639-1 language code.

---

### To update translations for a language

#### 1. Update translations for the source RST text
Run `make gettext` to extract the messages and create new POT files. This uses the Sphinx gettext builder to extract a message from each document node. This is nice because it works without having to modify the source RST in any way. There will be one POT file for each source RST file and they will be created in `build/gettext`.

Update the PO files for the language from the new POT files:
```
$ sphinx-intl update --locale-dir source/locales --pot-dir build/gettext --language <LANG>
```
Or to update PO files for all languages:
```
$ sphinx-intl update --locale-dir source/locales --pot-dir build/gettext
```

Send the PO files in `source/locales/<LANG>/LC_MESSAGES` to the translator for the language. The translator will fill in the translations and send them back.

Replace the PO files in `source/locales/<LANG>/LC_MESSAGES` with the updated ones from the translator.

#### 2. Update translations for text imported from `natcap.invest`
Update the relevant translations in `natcap.invest` (see the InVEST internationalization README).


### To add a new language

#### 1. Add a new language for the source RST text
Follow the same directions above to [update translations](#1-update-translations-for-the-source-rst-text). The `sphinx-intl update` command will create new PO files for a language if it doesn't already exist.

#### 2. Add a new language for the text provided by Sphinx
The language will most likely be on the [list of languages supported by Sphinx](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-language), and so nothing needs to be done. But if not, we'll need to figure out how to provide our own message catalog for Sphinx-generated messages.

#### 3. Add a new language for the text imported from `natcap.invest`
Make sure that `natcap.invest` supports the new language as well (see the InVEST internationalization README).


## To build docs in a different language
```
make SPHINXOPTS="-D language=<LANG>" html
```

