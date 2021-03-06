<!-- This readme is targeted at developers. The general user readme is in /.github/README.md -->

# Data Format for Digital Linguistics (DaFoDiL)

The DLx data format is a set of recommendations (i.e. schemas or specifications) for how to store linguistic data in JSON- a simple, human-readable text format which is supported by every major programming language, and is widely used for data storage and interchange on the web. The DLx format is useful for anybody who manages linguistic data and knows a programming language. All major programming languages support JSON natively, allowing you to easily convert to and from DaFoDiL format.

<!-- uncomment this when Lotus and TooLiP are live -->
<!-- If you do not have any programming experience and are looking for a web-based tool for managing linguistic data instead, check out the DLx [Lotus app][Lotus] or [Tools for Linguistic Productivity][TooLiP] (<abbr title="TooLiP">TooLiP</abbr>). -->

The format includes recommendations (called "schemas") for storing data about every kind of linguistic entity (e.g. Language, Morpheme, Text, etc.). It is part of a broader project called [Digital Linguistics][About] (DLx), which aims to create web-based tools for managing linguistic data, and to encourage best practices in digital linguistic data management.

This repository contains the specification for the Data Format for Digital Linguistics (abbreviated as <abbr title="Data Format for Digital Linguistics">DaFoDiL</abbr>), in the form of a number of schemas. There is one schema for each type of linguistic object (e.g. Language, Morpheme, Text, etc), and schemas for various non-linguistic objects as well (e.g. Person, Location, etc.). The schemas follow the [JSON Schema][JSON Schema] format for describing the structure of JSON data.

See the [documentation][Docs] for human-readable versions of the schemas, and an example of the schema in use.

[View the code for this project on GitHub.][GitHub]

Please consider citing this specification in scholarly articles using this repository's [Zenodo][Zenodo] DOI:

> Hieber, Daniel W. 2020. _Data Format for Digital Linguistics_. DOI:[10.5281/zenodo.1438589][Zenodo]

[![GitHub stars](https://img.shields.io/github/stars/digitallinguistics/spec.svg?style=social&label=Stars)][GitHub]
[![npm downloads](https://img.shields.io/npm/dt/@digitallinguistics/spec.svg)][npm]
[![GitHub issues](https://img.shields.io/github/issues/digitallinguistics/spec.svg)][Issues]
[![npm version](https://badge.fury.io/js/%40digitallinguistics%2Fspec.svg)][npm]
[![Build Status](https://github.com/digitallinguistics/spec/workflows/tests/badge.svg)][Actions]
[![license](https://img.shields.io/github/license/digitallinguistics/spec.svg)][License]
[![DOI](https://zenodo.org/badge/50221632.svg)][Zenodo]

## Contents & Quick Links

* [Open an Issue][Issues]
* [Contributing Guidelines][Contributing]
* [Basic Usage](#basic-usage)
* [Data Validation](#data-validation)
* [The Specification][Docs] (human-readable version)
* [The Schemas](#schemas)
* [Best Practices](#best-practices)
* [NDJSON](#ndjson)
* [Tests](#tests)

## Basic Usage

### Node.js

* Install the schemas: `npm install @digitallinguistics/spec`

* Install a JSON Schema validator to validate your data against the schemas: `npm install ajv`

* Validate the data:

    ```js
    // Imports
    const AJV     = require(`ajv`);
    const schemas = require(`@digitallinguistics/spec`);

    const ajv      = AJV(); // Initialize ajv
    const language = { /* your data to validate */ };

    // Validate your data
    const valid = ajv.validate(schemas.Language, data);
    if (!valid) console.error(ajv.errorsText());
    ```

### Browser

* Download the schemas to your project in a `/schemas` folder.

* Include a JSON Schema validator in your project to validate your data against the schemas:

    ```html
    <script src=https://cdnjs.cloudflare.com/ajax/libs/ajv/6.5.4/ajv.min.js></script>
    ```

* Validate the data:

    ```js
    (async function validate() {

      const ajv      = Ajv(); // Initialize ajv
      const language = { /* your data to validate */ };

      // Load the schema
      const res    = await fetch(`schemas/Language`);
      const json   = await res.json();
      const schema = JSON.parse(json);

      // Validate the data against the schema
      const valid = ajv.validate(schema, data);
      if (!valid) console.error(ajv.errorsText());

    })();
    ```

## Data Validation

If you need to validate your linguistic data against the DLx schemas, you can use one of the [JSON Schema validators][Validators]. This project uses the [`ajv`][ajv] validator for testing.

## Schemas

Schemas are located in the `/schemas` folder in both JSON and YAML formats.

You can install the schemas to your computer using npm (`npm install @digitallinguistics/spec`), or access them programmatically at the following URLS:

URL | Result
--- | ------
`https://schemas.digitallinguistics.io/{schema}` | latest version of the schema, as JSON
`https://schemas.digitallinguistics.io/{schema}.json` | latest version of the schema, as JSON
`https://schemas.digitallinguistics.io/{schema}.yml` | latest version of the schema, as YAML
`https://schemas.digitallinguistics.io/{schema}-{X.X.X}` | specified version of the schema, as JSON
`https://schemas.digitallinguistics.io/{schema}-{X.X.X}.json` | specified version of the schema, as JSON
`https://schemas.digitallinguistics.io/{schema}-{X.X.X}.yml` | specified version of the schema, as YAML

For example, you could access v1.0.0 of the `Language` schema in each of the following ways:

* `https://schemas.digitallinguistics.io/Language`
* `https://schemas.digitallinguistics.io/Language.json`
* `https://schemas.digitallinguistics.io/Language.yml`
* `https://schemas.digitallinguistics.io/Language-1.0.0`
* `https://schemas.digitallinguistics.io/Language-1.0.0.json`
* `https://schemas.digitallinguistics.io/Language-1.0.0.yml`

## Best Practices

The following is a list of principles and best practices to keep in mind when working with linguistic data in DLx format.

* **Storage vs. Use**

    * This specification describes how data should be _stored_, i.e. in a database or JSON file. It does **not** recommend how that data should be formatted when it is being managed or manipulated. Required properties could be missing during data entry, or data could be represented using (for instance) an Object instead of an Array while the data is being manipulated. It is only when you _store_ that data in a file or database that it must be in valid DLx format.

* **YAML vs. JSON**

    * The DaFoDiL schemas may be applied to either YAML or JSON documents. It may be easier to write your linguistic data in YAML format, and then convert it to JSON for storage in a database.

* **Documents**

    * The DaFoDiL format is designed to work well with _document databases_, where each item is stored as a single document (typically in JSON) rather than as records in a table. Schemas that include a comment that they are top-level database objects should be their own documents in the database. Other schemas will be subparts of those documents.

* **IDs & Cross-References**

    * The DLx schemas support unique identifiers in the form of opaque IDs or human-readable keys, or both (recommended - see below). Many schemas include an optional `id` field, which is meant to be representative of whatever opaque identifier scheme best suits your database. You could name this field `id`, `ID`, `dbid` (database ID), `localid`, `uuid`, `uri`, etc. etc. Thus when the `id` property is referenced in the schemas, it should not be taken literally as being the `id` field. Instead it refers to whichever field your database uses for unique, opaque identifiers. Whichever name you choose for the property should be used consistently in place of the `id` fields. No restrictions are placed on the format of the ID field other than that it must be non-empty, although best practice is for this field to be a string (preferably a UUID) or a number.

    * If your database depends on unique, opaque identifiers (e.g. a [UUID][UUID]), you should **also** use human-readable keys. For example, a Lexeme object representing the word "book" (the noun) might have an ID of `d0e51fcb-84af-44aa-ba16-67561e21c793`, but should also have a key `book1`. This helps users identify it as the lexeme "book", while simultaneously helping distinguish it from other "book" homonyms in the database (such as the word "book" used as verb, which might be `book2`).

    * Generally speaking, DLx data should be stored in denormalized (embedded) format whenever it is practical to do so. Sometimes, however, data needs to be normalized when embedding the data directly would be impractical or create infinite recursion. The DLx specification provides a `DatabaseReference` object for this purpose, used to reference other items in a database. For example, a Lexeme may have a reference to an Utterance in its `examples` field, and that Utterance might also have a reference to the Lexeme. Because it would be impossible to represent such a recursive relationship in a single JSON document, database references are used instead. Each DatabaseReference object contains `key`, `url`, `id`, `index`, and `referenceType` properties which allow you to uniquely identify other items in the database. Other properties are required or permitted as appropriate.

* **URLs**

    * Top-level schemas (and some subschemas) include a `url` property which should be used to indicate a URL where that resource can be retrieved in JSON format. It should not be used for the URL of a human-readable presentation of the resource (the `link` property should be used for this purpose). The resource does not have to be publicly available at this URL; it may require permissions to access.

    * Top-level schemas also include a `link` property. This is used to indicate a URL where a presentational format of the given resource can be viewed, for instance a text viewer or lexicon editor, etc.

* **Completeness**

    * An exported database should be complete, self-contained, and human-readable in the sense that a user should be able to find and follow any cross-references easily. For example, if the Lexeme `book1` has a cross-reference to the Lexeme `book2`, the `book1` Lexeme should reference the other Lexeme as `book2` and not just a database ID like `d0e51fcb-84af-44aa-ba16-67561e21c793`. Moreover, both `book1.json` and `book2.json` should be included in the export. The Text schema has optional `lexemes`, `orthographies`, and `texts` properties, allowing you to save/export all the data in a language corpus in a single file (if this is feasible for your project).

* **Optional, Required, & Empty Properties**

    * Typically, if an optional property is present, it should have data in it. If the data in the property is empty (e.g. an empty Array, an empty String, an Object without properties, etc.), you should remove that property before saving the data. In other words, do not store empty Strings, empty Arrays, etc. unless those properties are required. This helps keep storage costs to a minimum, while reducing clutter and maintaining human-readability.

    * Occasionally, the description for a schema imposes restrictions or guidelines that the schema itself technically does not. (This occurs in cases where it is impossible to capture a requirement in the JSON Schema format.) In these cases, implementations should adhere to the requirements of the schema description in addition to the requirements enforced by the JSON schema itself.

* **Context**

    * Schemas sometimes have different uses or interpretations depending on the context in which they appear. For example, when an Utterance appears in the `"utterances"` property of a Lexeme, it is an example utterance. When it appears in the `"utterances"` property of a Text, it is a transcribed utterance from that Text. When a schema appears within another schema, its `"description"` field will tell you how it should be used in that context.

    * Most schemas have an optional `type` field. This field is not required because it may conflict with database implementations which already make use of a `type` keyword for other purposes. The `type` field or some other similar field is however strongly recommended. If using the `type` field conflicts with your database model, `dlxType` is recommended instead.

* **Dates**

    * The schemas support both [internet date and date-time formats][DateTime], but date-time format is strongly recommended.

## NDJSON

The DLx format is also compatible with the [Newline Delimited JSON (NDJSON) format][NDJSON], which is "a convenient format for storing or streaming structured data that may be processed one record at a time". For example, you could place an entire lexicon into a single JSON file, so that each line of the file is a Lexeme object, formatted to fit on a single line.

## Tests

Tests are run using [Jasmine][Jasmine] in Node.js. Run them from the command line using `npm test`.

[About]:        https://digitallinguistics.io/about
[Actions]:      https://github.com/digitallinguistics/spec/actions
[ajv]:          https://www.npmjs.com/package/ajv
[Contributing]: https://github.com/digitallinguistics/spec/blob/master/.github/CONTRIBUTING.md
[DateTime]:     https://www.w3.org/TR/NOTE-datetime
[Docs]:         https://format.digitallinguistics.io
[GitHub]:       https://github.com/digitallinguistics/spec
[Issues]:       https://github.com/digitallinguistics/spec/issues
[Jasmine]:      https://jasmine.github.io/
[License]:      https://github.com/digitallinguistics/spec/blob/master/LICENSE.md
[Lotus]:        https://app.digitallinguistics.io
[NDJSON]:       http://ndjson.org/
[npm]:          https://www.npmjs.com/package/@digitallinguistics/spec
[JSON Schema]:  http://json-schema.org/
[TooLiP]:       https://tools.digitallinguistics.io
[UUID]:         https://www.uuidgenerator.net/
[Validators]:   http://json-schema.org/implementations.html#validators
[Zenodo]:       http://doi.org/10.5281/zenodo.594557
