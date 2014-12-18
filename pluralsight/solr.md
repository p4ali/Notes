## Overview (Geting started class)

* Architecture
* Solr configuration
* Content: Schema, solor config and indexing
* Searching and relevance
* Put UI on it

### Enterprise search
Practice of generating content and making it searchable to a defined audience out of multiple enterprise-type data sources like databases or CMS.
* Simplicity
* Intelligence analysis

### Solr
* Open source
* Dynamic content
* Fast and sophisticated search
* Active community
* Searches an inverted index instead of searching through the text
* Simplicity is the ultimate sophisticaiton
* solr cloud with Apache zookeeper

### Functions of search engine
* Indexing - organize all content for quick access
* Query parsing - understand what the user is looking fo
* search and browse - quickly find the information of interest
* security - filter out documents the user is not allowed to see
* relevance ranking - order the information in useful ways by relavance

## Architecture
[This picture](https://github.com/p4ali/Notes/blob/master/pluralsight/solr_arch.jpg) describe the components.
* Solr is sererization of Lucene, an open-source search platform
* Lucene is open-source **search** and **indexing** engine. Works with any docuemnt which has fields that can be extracted and indexed.

## Related Apache projects
* Hadoop: platform to create distributed systems
* Mahout: machine learning library
* Nutch: Web crawler
* OpenNLP: machine learning library for peocessing natural language
* Tika: parser to detect and extract meta data.

## Search Applicaiton
* It is to make user's life easier. Simplicity is the ultimate sophistication.
* Addtional aids include "did you mean", synonyms,autosuggest, spell check, phonetic search, lemmatize and more

## Play with Solr
* Download solr from [http://lucene.apache.org/solr/](http://lucene.apache.org/solr/)
* Unzip to /opt/solr-4.10.2
* The /opt/solr-4.10.2/example is a full solr application
* Start solr
* upload data with post.jar
```bash
# in a terminal
$ cd /opt/solr-4.10.2/example
$ java -jar start.jar # start solr web server in http://localhost:8983/solr

# in a different terminal
$ cd /opt/solr-4.10.2/example/exampledocs
$ java -jar post.jar -help # help doc
$ java -jar post.jar *.xml # import *.xml file into solr.
  implePostTool version 1.5
  Posting files to base url http://localhost:8983/solr/update using content-type application/xml..
  POSTing file gb18030-example.xml
  POSTing file hd.xml
  POSTing file ipod_other.xml
  POSTing file ipod_video.xml
  POSTing file manufacturers.xml
  POSTing file mem.xml
  POSTing file money.xml
  POSTing file monitor.xml
  POSTing file monitor2.xml
  POSTing file mp500.xml
  POSTing file sd500.xml
  POSTing file solr.xml
  POSTing file utf8-example.xml
  POSTing file vidcard.xml
  14 files indexed.
  COMMITting Solr index changes to http://localhost:8983/solr/update..
  Time spent: 0:00:00.262

# Once imported, you will see the num docs files changed from 0 to 32.
```

### Other upload mechanisms
* DataImportHandler
* Add XML, Json, CSV documents
* SOlrNet or SolrJ
* Multiple tools that post documents to Solr's index
 * ASPIRE

### Query
* [http://localhost:8983/solr/select?q=video](http://localhost:8983/solr/select?q=video)
* The above url mad up of
 * host name (localhsot) & port number (8389)
 * Application name (solr)
 * Request handler for queries (select)
 * Query itself (q=video)
* A core is a running lucene instance with all solr configuration.
* The query result has following format
```
response
  responseHeader
    status
    QTime
    params
      p1
      p2
      ...
      pn
  result
    doc1
      field1
      ...
      fieldn
    ...
    docn
```
