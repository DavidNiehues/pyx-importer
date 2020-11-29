# Card importer for Pretend You're Xyzzy.

Given a spreadsheet of input card text and deck names (currently only xlsx supported), produces database output suitable for use with PYX. Any Hibernate dialect that PYX works with should also work here (only tested with Postgres and SQLite).


## CONFIGURATION:

Copy importer.template.properties to importer.properties and fill in, at the minimum, the hibernate and import blocks. If it WARNs about any unhandled special characters, add them to the replace block to replace them with HTML entities (it'll probably work if they're Unicode literals, but for maximum compatibility PYX prefers everything that isn't in the basic Latin character set to be entities... except for deck watermarks (see below), as those have a lower maximum character limit).

The deckinfo block is optional, but makes the decks "look nicer" in PYX. You can also assign multiple ids to the same name to combine them, if your input source isn't self-consistent.


## EXCEL FORMAT

Each sheet contains etiher only black or only white cards as specified in the configuration file (```importer.properties``` by default).
For each sheet there are two ways of formatting it that can also be configured via the configuration file.
1. Put the identifier of the card deck the cards belong to in the first row in the first column as a header. Then put all the cards under the header in the same column. You then need to set ```heading_named_count=1``` for that sheet in the configuration file.
2. (I did not test this way, it seems to be this way by reading the code). Leave the first row of the sheet empty and after that put each card in a single row and the identifier of the card deck in the column right next to it. If you start witht the first row, you need to set ```next_column_named_count=1``` for that sheet.

There seems to be the option to have several columns of cards in each sheet and to combine the two ways of formatting the sheets with each other. However, I did not try these out since the first approach described above works well.


## BUILDING:

You must have PYX installed in your local Maven repository. This is most easily accomplished by checking it out, and running ```mvn clean install```. The Hibernate ORM classes are used directly from that project.

After that, it should be a simple ```mvn clean package``` to produce a fat jar.


## RUNNING:

```java -jar target/pyx-importer-0.0.1-SNAPSHOT-jar-with-dependencies.jar``` will run with default options.


## OPTIONS:

```
-c, --configuration <File: filename>  Configuration file to use. (default:
                                        importer.properties)
--format [Boolean]                    Process rich-text formatting for card
                                        text. (default: true)
-h, --help                            Print this usage information.
--save [Boolean]                      Save parse results to database. (default:
                                        true)
--schema                              Output the required database schema and
                                        exit.
```
