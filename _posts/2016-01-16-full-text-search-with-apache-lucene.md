---
id: 255
title: Full-text search with Apache Lucene
date: 2016-01-16T10:36:49+00:00
author: newbiettn
layout: post
guid: http://newbiettn.com/?p=255
permalink: /2016/01/full-text-search-with-apache-lucene/
categories:
  - Uncategorized
tags:
  - apache
  - apache lucene
  - apache mahout
  - full-text search
  - index
  - indexing
  - java
  - lucene
  - mahout
  - query
  - search
---
Apache Lucene is a de facto open source library targeting search and search- related tasks such as indexing or querying.

### 1. How Lucene indexes documents algorithmically

Lucene uses the _inverted index data structure_ to index documents. The indexing strategy of Lucene is different from others when it creates segments and merges them, rather than uses a single index. Typically, an index segment consists of a dictionary index, a term dictionary and a posting dictionary.

<div id="attachment_256" style="width: 610px" class="wp-caption aligncenter">
  <img class="wp-image-256" src="http://newbiettn.com/wp-content/uploads/2016/01/indexing_lucene-1024x476.png" alt="The index diagram with the merge factor b is 3" width="600" height="279" srcset="http://newbiettn.com/wp-content/uploads/2016/01/indexing_lucene-1024x476.png 1024w, http://newbiettn.com/wp-content/uploads/2016/01/indexing_lucene-300x139.png 300w, http://newbiettn.com/wp-content/uploads/2016/01/indexing_lucene-768x357.png 768w" sizes="(max-width: 600px) 100vw, 600px" />
  
  <p class="wp-caption-text">
    <strong>Figure 1:</strong> The index diagram with the merge factor b is 3
  </p>
</div>

**Figure 1** illustrates an example of an index, where the merge factor equals three. In the example, there are 14 documents in the collection. Lucene repeatedly creates a segment for a document and periodically merges a group of 3 documents. The process is similar when Lucene keeps merging a group of 3 segments until there is no more segment to merge. After merging, all greyed segments will be removed and in total, Lucene merged 5 times after the indexing finishes. The approach of merging and deleting segments is considerably useful as the document collection does not change frequently. More importantly, the segment will never be modified. Instead, Lucene creates new segments when the document collection changes and later, it merges segments into new ones and deletes the old segments. This strategy ensures there is no conflict between reading writing indexes. Furthermore, this strategy also allows Lucene to avoid complex B-trees to store segments. Instead, all segments will be stored in flat files.

### 2. Extract text and index

Typically we can divide indexing documents into two distinct procedures, extracting text and creating index **(Figure 2)**.

<div id="attachment_272" style="width: 410px" class="wp-caption aligncenter">
  <img class="wp-image-272" src="http://newbiettn.com/wp-content/uploads/2016/01/Screen-Shot-2016-01-16-at-11.13.03-AM-252x300.png" alt="Figure 2. Indexing with Lucene breaks down into three main operations: extracting text from source documents, analyzing it, and saving it to the index" width="400" height="477" srcset="http://newbiettn.com/wp-content/uploads/2016/01/Screen-Shot-2016-01-16-at-11.13.03-AM-252x300.png 252w, http://newbiettn.com/wp-content/uploads/2016/01/Screen-Shot-2016-01-16-at-11.13.03-AM.png 562w" sizes="(max-width: 400px) 100vw, 400px" />
  
  <p class="wp-caption-text">
    <strong>Figure 2</strong>. Indexing with Lucene breaks down into three main operations: extracting text from source documents, analyzing it, and saving it to the index [Photo courtesy of Lucene in Action]
  </p>
</div>

In extracting procedure, it is common to use a versatile parser that can extract textual contents from documents. One of widely known tools we can use to parse documents is <a href="https://tika.apache.org" target="_blank">Apache Tika</a>. When parsing completes, we will have an input stream that needs to be indexed. It is the second procedure of our index process &#8211; creating an index.

More often than not, we do not index everything we retrieve from the input stream. We instead need to apply **analysis process** where we will discard unimportant terms from the textual content (e.g., punctuations, stopping words). In Lucene, the analysis process is handled by <span class="lang:default decode:true crayon-inline ">Analyzer</span> . The framework already provides handy and powerful analysers that we can use without customising in a majority of problems.

After analysing, the textual contents are ready to be indexed.

### **3. Building an index**

In this section, we will look at how we can create a new index and add documents to it.

#### 3.1 Construct an index writer

In Lucene, the index is created and maintained by <span class="lang:default decode:true crayon-inline ">IndexWriter</span> . The IndexWriter constructors can be defined in various ways but most often we simply need to supply

  * <span class="lang:java decode:true crayon-inline ">Directory d </span>where the index will be stored
  * <span class="lang:java decode:true crayon-inline">IndexWriterConfig conf</span>  holds all the configuration that is used to construct an <span class="lang:default decode:true crayon-inline">IndexWriter</span>

In the code below, we can see that I have define an instance of <span class="lang:java decode:true crayon-inline ">IndexWriterConfig</span>  with a specialised <span class="lang:java decode:true crayon-inline ">Analyzer</span> . The purpose is I want to analyse documents in a particular way that suits my personal needs (e.g., I want to keep specific words that built-in analysers will certainly remove).

In the given code sample, we use <span class="lang:default decode:true crayon-inline ">FSDirectory</span> . There are other alternative solutions, such as <span class="lang:default decode:true crayon-inline ">SimpleFSDirectory</span> , <span class="lang:default decode:true crayon-inline ">NIOFSDirectory</span> .

<pre class="lang:java decode:true" title="Construct an IndexWriter">/**
 * Create an Indexer using the given directory to store index files, and the
 * directory need to be indexed.
 * 
 * */
public Indexer(String anIndexDir, String aDataDir) {
	// set place to store indexes
	this.indexDir = anIndexDir;

	// set directory has to be indexed
	this.dataDir = aDataDir;

	try {
		// create Directory to store indexes, use FSDirectory.open
		// to automatically pick the most suitable directory implementation
		Directory dir = FSDirectory.open(new File(indexDir));

		// create an instance of Analyzer with my own custom analyzer
		analyzer = new MyCustomAnalyzer();

		// create configuration for new IndexWriter
		IndexWriterConfig conf = new IndexWriterConfig(Version.LUCENE_46,
				analyzer);

		// create IndexWriter, which can create new index, or
		// adds, removes, or updates documents in the index
		// however, it can not read or search
		this.writer = new IndexWriter(dir, conf);

	} catch (IOException e) {
		e.printStackTrace();
	}
}
</pre>

#### 

#### 3.2 Modelling document

In the framework textual content is modelled using the class <span class="lang:default decode:true crayon-inline ">Document</span> . This class allows to model a document instance with numerous attributes where for every attribute we can also define using the class <span class="lang:default decode:true crayon-inline ">Field</span> .

In the sample code below, I have define an <span class="lang:default decode:true crayon-inline ">Document</span>  instance with a number of attributes:

  * **Document ID**: because in practice we may have thousands of documents, I have used base64 to generate unique IDs for documents. Notice that we supply the <span class="lang:java decode:true crayon-inline ">StringField</span>  constructor with the variable <span class="lang:java decode:true crayon-inline ">Field.Store.YES</span> , which means the values of this attributes will be stored in index files. More importantly, we **do not index **the <span class="lang:default decode:true crayon-inline ">StringField</span>  by default.
  * **File path**: will be stored
  * **Content field**: as you may think, we need to index the content field for further full-text search. However, we may or may not store it in the index files. It is a matter of choice. In this particular case, I choose to store the field.

<pre class="lang:java decode:true ">Document doc = new Document();

// docId
// generated by base64 of filepath
String id = BaseEncoding.base64().encode(f.getCanonicalPath().getBytes(Charsets.US_ASCII));
doc.add(new StringField(MyDocumentIndexedProperties.ID_FIELD, id,
		Field.Store.YES));

// filepath
doc.add(new StringField(MyDocumentIndexedProperties.FILE_PATH_FIELD, f
		.getCanonicalPath(), Field.Store.YES));

// content field
TokenStream ts = analyzer.tokenStream(
		MyDocumentIndexedProperties.CONTENT_FIELD, new StringReader(
				handler.toString()));
Field contentField = new Field(
		MyDocumentIndexedProperties.CONTENT_FIELD,
		AnalyzerUtils.tokenStreamToString(ts), TYPE_STORED);
ts.close();
doc.add(contentField);</pre>

#### 

#### 3.3 Index document

While Lucene consists many components, it, at the same time, uses such extremely simple and intuitive methods to abstract the complex structures that it is fascinating seeing how it is simple to perform indexing.

<pre class="lang:java decode:true ">Document doc = new Document();

//modelling document in the same manner as section 3.2
//...
//...

//add document to the index
writer.addDocument(doc);

//close the index when finish
//which also means we commit the index to be ready to store
writer.close()</pre>

&nbsp;

### 4. Full-text search

Until now we have an index that can be used to search over. To initialise a search, we must input a search query, which can be a keyword or even a wild card. In Lucene, the search query will firstly be handled by <span class="lang:default decode:true crayon-inline ">QueryParser</span>  where the query will be parsed into meaningful structure, called <span class="lang:default decode:true crayon-inline ">Query</span> . After that, the <span class="lang:default decode:true crayon-inline ">IndexSearcher</span>  will consume <span class="lang:default decode:true crayon-inline ">Query</span>  instance in order to look up over the index.

<pre class="lang:java decode:true">// searcher
IndexSearcher searcher = new IndexSearcher(this.indexReader);

// query parser
QueryParser parser = new QueryParser(Version.LUCENE_46, "content", new SimpleAnalyzer());

// parsing query
Query q = parser.parse("....");

// look up and return top 10 relevant documents
Topdocs docs = searcher.search(query, 10);</pre>

### 5. Bottom line

So far we have seen a &#8220;hello world&#8221; example of using Lucene to perform full-text search. In real project, we can expect there are a number of possibilities where we need to tweak Lucene so that it can serve our search purpose at a higher level. An example scenario is when we need to support **like-this search** where the system allows the user to search textual contents that are similar to the keywords. Theoretically we can measure **the similarity **by using distance measure methods, such as Euclidean distance measure, cosine distance measure, or Manhattan distance measure, etc. Another scenario is when we can create a system that supports **synonym search **where we retrieve not only document contains the search keywords but also documents that has synonyms of keywords.

As said, in spite of the simplicity, Lucene adds a tremendous amount of support in building a powerful search tool. In the future post, I may discuss how we can combine both Apache Lucene and Apache Mahout (a machine learning framework) in order to index a corpus and use that index to perform clustering. Stay tuned!