# python-capture-model-tools
Python code to turn a pipe-delimited/csv of groups and fields into JSON that can be imported by the omeka capture model importer.

Initially for internal use by Digirati clients using the Annotation Studio to render annotation, transcription, tagging, and other crowd-sourcing capture models.

The Annotation Studio uses a single nested block of JSON-LD which defines the fields to be captured, and how those fields should be grouped and nested in the UI.

This block of JSON-LD is generated by quering the Omeka API for:
* Item Sets -- which correspond to Groups of fields.
* Items -- which correspond to individual Element fields.

This block of JSON-LD can be generated by Wilko (a standalone Flask service) or natively by the Annotation Studio model in Omeka.

This tool generates an importable version of the JSON-LD model that can be imported used the Capture Model Importer model, and which will generate the Omeka Item Sets and Items as required to provide the full capture model via Wilko or the Annotation Studio module.

 
# Changes since previous version

* Dropped use of templates for the Crowd Source Group and Crowd Source Elements. JSON generated directly from the Python object created for each CSV row.
* Use QNames [https://en.wikipedia.org/wiki/QName] for the CSV column headers
* Automatically expand and contract QNames during JSON-LD generation.
* Use @context.json file for namespace lookup
* Default boolean and non-boolean values set automatically, to reduce the number of columns that need data entered.
* use of @id and QName pairs for linked data vocab (including Madoc vocab) throughout
* less verbose, cleaner Python code.



# Installation

These tools are best run in a Virtualenv.

```bash
git clone https://github.com/digirati-co-uk/python-capture-model-tools.git
cd python-capture-model-tools
virtualenv .
pip install -r requirements.txt
```

Howver, they can be installed outside a Virtualenv.

```bash
git clone https://github.com/digirati-co-uk/python-capture-model-tools.git
cd python-capture-model-tools
pip install -r requirements.txt
```

# Usage

The capture model tools can be run on the commandline, with commandline arguments passed in. The arguments are as follows:

```
 '-i', '--input', help='Input CSV file name', required=True
 '-o', '--output', help='Output JSON file name', required=True
 '-b', '--url_base', help='Base url for the Omeka instance', required=False
 '-t', '--top_index', help='Numbered element to treat as the top level group', required=False
 '-g', '--group_id', help='ID for the Crowd Source Group resource template', required=False
 '-e', '--element_id', help='ID for the Crowd Source Element resource template', required=False
 '-c', '--irclass', help='ID for the Interactive Resource class', required=False
 '-u', '--user', help='Omeka User ID for the Owner', required=False
 '-x', '--context', help='IDA Context', required=False
```

### Input

The tool expects a pipe-delimited '|' file, with column heads as per `template.csv` provided as part of this repo.

e.g.

`-i gle.csv`

### Output

The tool will produce a JSON-LD document.

e.g.

`-o gle.json`

### URL Base

The JSON-LD uses the address of the server where the model will be deployed throughout, to ensure correct `@id`s that will resolve.

e.g. for NLW: 

`-b http://nlw-omeka.digtest.co.uk`

### Top Index

The CSV should use numbered identifiers in each row, under the `dcterms:identifier` column.

`-t 1`

tells the tool to start building the model with the row numbered `1`.

