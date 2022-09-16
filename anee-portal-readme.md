# ANEE lexical portal data and code distribution

This directory contains the data and code used to build ANEE's lexical portal for investigating patterns in word use in ancient Akkadian. The data is specific to this language and period, but the code mostly isn't, and could be adapted to other settings. Unfortunately, some skill with Python and bash, as well as time and patience is required for this.

This package is intended more as code supplement than as a software package. Notes for running the pipeline in various configurations are given in this document, but for detailed questions, email me at sam.hardwick@iki.fi.

## Data

The data/ directory contains .csv files, which are lists of connections between
nodes:

        source;target;weight
        ṭūru;šiqlu;1.258
        ṭūdu;ṣarpu;0.81

Here, ṭūdu and are connected to each other with weight 0.81.

It also contains a dictionary in the form of a .tsv file, and a corpus report in .json form, containing information used to build a dictionary with English translations of each word.

The .tsv file is a "guide word" dictionary, containing intended translations for some, but not all, words.

The .json file is built from a corpus with translations, and as a consequence is messy, containing many redundant alternatives but also alternate meanings. This file is processed by a script together with the guide word dictionary to build the final dictionary used by the portal.

Finally, `missing-translations.txt` lists lemmas with missing translations, which are excluded from graph generation. Any lemmas with missing translations but _not_ listed here will be included in the graph, but with translations given as the same as the lemma.

## The portal

The portal/ directory contains our fork of gexf-js, a web viewer for graphs. We have added functionality in particular for incremental searching of words and their translations.

## Scripts

The data files are processed in multiple steps and can take various parameters. The scripts for doing this are in the scripts/ directory. These examples assume that the scripts are run in the `data/` directory.

### Requirements

Some third-party Python libraries are required:
- `sknetwork`, a graph library
- `lxml`, used for gexf parsing and building
- `fa2`, an implementation of the ForceAtlas2 clustering algorithm
- `numpy`
- `scipy`

### Building the dictionary

The dictionary is built with the Python script `process_lemmas.py`. This script can be run with default settings using the command:

        python ../scripts/process_lemmas.py oracc2021_dict.json guidewordtranslations.tsv > oracc2021_dict.tsv

### Building the graphs

#### .gexf and filtering

The .csv files are processed by `gexf_from_edgelist.py` into .gexf files. At this stage, unwanted words can be filtered out, which makes later processing steps faster. Supported by default are the options `--exclude-proper` and `--exclude-non-proper` for building graphs without proper nouns, or with only proper nouns, respectively. Example:

        python ../scripts/gexf_from_edgelist.py first_millennium.csv > first_millennium.gexf

#### `cluster.py`

The main job of clustering, ie. calculating positions for the nodes in 2D space and calculating clusters is done at this stage. Parameters related to this are currently in the form of variables in the script. Example:

        python ../scripts/cluster.py first_millennium.gexf first_millennium_layout.gexf

#### `insert_attribs.py`

This script inserts some additional information into .gexf files. In particular, translations for the nodes, URLs to external resources like corpus services and links to ego graphs (generated later). For example,

        python ../scripts/insert_attribs.py --write-translations-from oracc2021_dict.tsv --write-ego --write-korp -i first_millennium_layout.gexf -o first_millennium_layout_with_translations.gexf

This script is also used later in the pipeline to produce English-Akkadian graphs with the option `--swap-translation`.

#### `make_portal_package.sh`

This script generates ego graphs, arranges files into the directories the portal code expects, and generates .json versions of the graphs for faster loading by the portal. Following the example of `first_millennium` data from previously, a package may be built as follows:

        ../scripts/make_portal_package.sh ../data/first_millennium_layout_with_translations.gexf ../data/anee-portal-second-millennium "ANEE lexical portal of Akkadian: PMI, second millennium corpus (Akkadian-English)" "ANEE lexical portal of Akkadian: PMI, second millennium corpus (English-Akkadian)" 1

The arguments given are, in order,
1) The source .gexf file
2) The directory for the package to be built
3) The title of the portal with lemmas and translations in regular order
4) The title of the portal with translations and lemmas swapped
5) Optionally, a number for the degree of ego graph. This defaults to 1. Higher values may produce very large packages.

This results in the target directory containing a structure like this:

- `anee-portal-first-millennium/`
  - `egographs/` (containing a .gexf and .json file per lemma)
  - `first_millennium_layout_with_translations.gexf`
  - `first_millennium_layout_with_translations.json`
- `anee-portal-first-millennium-swapped/`
  - `egographs/` (containing a .gexf and .json file per lemma)
  - `first_millennium_layout_with_translations-swapped.gexf`
  - `first_millennium_layout_with_translations-swapped.json`

## Use

After following the steps for generating a data package, place the contents of `portal/` on a webserver. In this directory, place a generated `.gexf` and `.json` file, and either name them `index.gexf` and `index.json` or make corresponding links, eg.

        ln -s first_millennium_layout_with_translations.json index.json

Also copy over a generated `egographs/` directory to get the egographs to work.
