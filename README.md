# WikiExtractor
[WikiExtractor.py](http://medialab.di.unipi.it/wiki/Wikipedia_Extractor) is a Python script that extracts and cleans text from a [Wikipedia database dump](http://download.wikimedia.org/).
It is an extension of the WikiExtractor script written by Giuseppe Attardi.

This version is simplified in its usage and it allows to easily select only a subset of article pages to extract.

The tool is written in Python and requires Python 2.7 or Python 3.3+ but no additional library.

For further information, see the [project Home Page](http://medialab.di.unipi.it/wiki/Wikipedia_Extractor) or the [Wiki](https://github.com/attardi/wikiextractor/wiki).

# Wikipedia Cirrus Extractor

`cirrus-extractor.py` is a version of the script that performs extraction from a Wikipedia Cirrus dump.
Cirrus dumps contain text with already expanded templates.

Cirrus dumps are available at:
[cirrussearch](http://dumps.wikimedia.org/other/cirrussearch/).

# Details

WikiExtractor performs template expansion by preprocessing the whole dump and extracting template definitions.

In order to speed up processing:

- multiprocessing is used for dealing with articles in parallel
- a cache is kept of parsed templates (only useful for repeated extractions).

## Installation

The script may be invoked directly, however it can be installed by doing:

    (sudo) python setup.py install

## Usage

The script must be invoked with 1 argument at least: the path to the Wikipedia dump from which data will be extracted.

The output is stored in several files of similar size in a given directory.
Each file will contains several documents in this [document format](http://medialab.di.unipi.it/wiki/Document_Format).

    usage: WikiExtractor.py [-h] [-o OUTPUT] [-b n[KMG]] [-c] [--json] [--html]
                            [-l] [-s] [--lists] [-ns ns1,ns2]
                            [--templates TEMPLATES] [--no-templates] [-r]
                            [--min_text_length MIN_TEXT_LENGTH]
                            [--filter_disambig_pages] [-it abbr,b,big]
                            [-de gallery,timeline,noinclude] [--keep_tables]
                            [--processes PROCESSES] [-q] [--debug] [-a] [-v]
                            input

    Wikipedia Extractor:
    Extracts and cleans text from a Wikipedia database dump and stores output in a
    number of files of similar size in a given directory.
    Each file will contain several documents in the format:

        <doc id="" revid="" url="" title="">
            ...
            </doc>

    If the program is invoked with the --json flag, then each file will
    contain several documents formatted as json ojects, one per line, with
    the following structure

        {"id": "", "revid": "", "url":"", "title": "", "text": "..."}

    Template expansion requires preprocesssng first the whole dump and
    collecting template definitions.

    positional arguments:
      input                 XML wiki dump file

    optional arguments:
      -h, --help            show this help message and exit
      --processes PROCESSES
                            Number of processes to use (default 1)

    Output:
      -o OUTPUT, --output OUTPUT
                            directory for extracted files (or '-' for dumping to
                            stdout)
      -b n[KMG], --bytes n[KMG]
                            maximum bytes per output file (default 1M)
      -c, --compress        compress output files using bzip
      --json                write output in json format instead of the default one

    Processing:
      --no-templates        Do not expand templates
      -it abbr,b,big, --ignored_tags abbr,b,big
                            comma separated list of tags that will be dropped,
                            keeping their content
      -de gallery,timeline,noinclude, --discard_elements gallery,timeline,noinclude
                            comma separated list of elements that will be removed
                            from the article text


Option --no-templates significantly speeds up the extractor, avoiding the cost
of expanding [MediaWiki templates](https://www.mediawiki.org/wiki/Help:Templates).


## Extraction of specific pages

The current implementation extracts music related pages.
In order to select which pages to consider it is simply sufficient to modify the keepPage method.

This method takes as input a whole  article page and its title.
You can check the latter or the categories the page is inserted into.

It is also possible to provide a list of pages to be extracted.
The titles will be loaded using the method loadDictArticles into a dictionary.
An example consist in providing pairs song_name \t artist.
Then using a more elaborated keepPage method (which also handles disambiguation) it is possible to extract these pages only.
This example is inserted commented in the code.

NOTE: if the list of pages to extract is quite long, the execution of the code will be way slower compared to a standard one (where all the pages are extracted or only the one related to a category).

