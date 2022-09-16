# Week3: Parsing Raw Data

## Extracting Data From CSV files

Pandas functions for reading tabular data

- `read_csv()`: Read delimited data from a file, URL, or file-like object. Use comma as default delimiter.
- `read_table()`: Read delimited data from a file, URL, or file-like object. Use tab as default delimiter.
- `read_fwf()`: read data in fixed-width column format, i.e., no delimiters.
- `read_clipboard()`: Version of read_table that reads data from the clipboard. Useful for converting tables from web pages.

## Extracting Data From JSON files

- JSON: one of the most commonly used formats for transferring data between web services and other applications via HTTP.
- JSON is completely language independent but uses conventions that are familiar to programmers of the C-family of languages.
- JSON is built on two structures:
  - A collection of name/value pairs. In various languages, this is realised as an object, record, struct, dictionary, hash table, keyed list, or associative array.
  - An ordered list of values. In most languages, this is realised as an array, vector, list, or sequence.

**Three basic elements:**

- Object: an unordered set of name/value pairs
- Array: an array is an ordered collection of values
- Value: a string in double quotes, or a number, or true or false or null, or an object or an array.

**pandas json functions:**

- `read_json()`: Convert a JSON string to pandas object
- `json_normalize()`: “Normalise” semi-structured JSON data into a flat table

## Extracting Data From XML files

XML is a software- and hardware-independent tool for storing and transporting data.

- It simplifies data sharing and platform changes — no need to worry about issues of exchanging data between incompatible systems
- It simplifies data transport — XML stores data in plain text format
- It simplifies data availability — With XML, data can be available to all kinds of "reading machines"
- XML was designed to be both human- and machine-readable.

According to the DOM (Document Object Model), everything in an XML document is a node.

The DOM says:

- The entire document is a document node
- Every XML element is an element node
- The text in the XML elements are text nodes
- Every attribute is an attribute node
- Comments are comment nodes

