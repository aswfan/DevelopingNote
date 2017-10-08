1.    创建索引。

索引创建原理大致分为以下几步：

- 分词，将原文档传给分词组件进行分词，得到词元。

- 词元处理，将词元传给语言处理组件进行一些语言处理，例如：变小写，转词根。

- 索引，对处理后的词元建立词典。例如，词‘中国’出现在ID为2，5，7的文档中，出现频率分别为1，5，3次。

具体实现如下：

创建索引时需要两个目录，一个是索引文件的存放目录（indexPath），一个是待索引的文件目录（docsPath）。在创建索引时，我们首先要加载或创建索引目录。常用的目录创建方式有两种：

- 通过RAMDirectory()类创建一个内存目录，内存目录优点是速度快，缺点是程序退出后索引目录数据就会丢失。
	
	RAMDirectory directory = new RAMDirectory();

- 通过FSDirectory类加载一个文件目录，该方式创建的索引数据保存在磁盘上，不会因为程序的退出而消失。下文都是针对FSDirectory方式来讲解Lucene的基本使用。
	
	Directory dir = FSDirectory.open(new File(indexPath));

然后通过IndexWriter对象来创建和维护索引。Analyzer为索引分词器，主要是对获取的文本进行分词操作。由于Lucene是由外国人开发的，所以本身对中文的分词效果不是很好。由于中文不像英文那样，词与词之间有明显的分隔符（英文一般以空格区分），所以中文分词在实现上就比英文分词复杂困难的多，而且歧义识别和新词识别一直是中文分词中的难点。

回归正题，IndexWriterConfig对象用来设置IndexWriter一些初始配置，一旦IndexWriter对象创建完成，改变IndexWriterConfig的配置对已创建的IndexWriter对象不会产生影响。

	Analyzer analyzer = new StandardAnalyzer(Version.LUCENE_40);
	IndexWriterConfig iwc = new IndexWriterConfig(Version.LUCENE_40, analyzer);               
	iwc.setOpenMode(OpenMode.CREATE_OR_APPEND);//设置索引维护的方式
	iwc.setRAMBufferSizeMB(256.0);//设置用来缓冲文档的RAM大小
	IndexWriter writer = new IndexWriter(dir, iwc);

接下来需要创建Document文档，并通过IndexWriter对象将文档添加到索引库中。Document文档可以是一个HTML页面，一篇文本文档，一封电子邮件。一个Document可以包含多个Field，Field代表文档的属性。此处file为文件对象。

	Document doc = new Document();       
	doc.add(new StringField("path", file.getPath(), Field.Store.YES));
	doc.add(new TextField("filename", file.getName(), Field.Store.YES));

注意：StringField被索引不被分词，整个值被看作为一个单独的token而被索引。

TextField既被索引又被分词，但是没有词向量。

Filed类型还包含LongField、DoubleField、IntFiled等，具体的含义请参考官方API。

最后需要将文档添加到索引库中。

	writer.addDocument(doc);
	writer.close();

2.    检索索引。

检索索引的原理大致如下：

- 对查询语句进行词法分析、语法分析和语言处理。词法分析主要是识别单词和关键字。语法分析主要是根据查询语句的语法规则来形成一颗语法树。语言处理与索引建立过程中语言处理几乎相同。

- 搜索索引，得到符合语法树的文档

- 根据得到的文档和查询语句的相关性，对结果进行排序。

具体实现如下：

检索索引首先要读取索引文档，并建立IndexSearch对象。

	IndexReader reader = DirectoryReader.open(FSDirectory.open(new File(indexPath)));
	IndexSearcher searcher = new IndexSearcher(reader);

然后需要对查询语句进行分析，Lucene通过Query接口实现类来对查询条件进行分析。Lucene支持单Filed查询、多Field查询、逻辑组合查询、模糊查询、范围查询、通配符查询等。各种查询方式的具体实现本文暂时不讨论。

- 单字段查询

		Analyzer analyzer = new StandardAnalyzer(Version.LUCENE_40);
		QueryParser queryParser = new QueryParser(Version.LUCENE_40, " filename",analyzer);
		queryParser.setDefaultOperator(QueryParser.OR_OPERATOR);//'或'模式
		Query query = queryParser.parse(condition);

- 多字段查询

		String[] fields = new String[]{"filename"," path "};
		Analyzer analyzer = new StandardAnalyzer(Version.LUCENE_40);
		//MultiFieldQueryParser multiFieldQueryParser = new MultiFieldQueryParser(Version.LUCENE_40, fields, analyzer);
		//Query query = multiFieldQueryParser.parse(condition);
		BooleanClause.Occur[] flags = {BooleanClause.Occur.SHOULD,
		                                                    BooleanClause.Occur.MUST};
		Query query = MultiFieldQueryParser.parse(Version.LUCENE_40, condition, fields, flags, analyzer);

接下来，通过IndexSearcher对象进行查找。对查询出来的结果可以进行高亮显示关键字，取摘要等处理。

    TopDocs results = searcher.search(query,10);
    ScoreDoc[] hits = results.scoreDocs;
    for(ScoreDoc hit:hits){
        Document doc = searcher.doc(hit.doc);
        SimpleHTMLFormatter formatter = new SimpleHTMLFormatter("<font color='red'>", "</font>");
        Highlighter highlighter = new Highlighter(formatter, new QueryScorer(query));
        Fragmenter fragmenter =new SimpleFragmenter(80);
        highlighter.setTextFragmenter(fragmenter);
        Information information = new Information();
        information.setId(hit.doc);                               
        information.setPath(doc.get("path"));
        if(highlighter.getBestFragment(analyzer, "filename", doc.get("filename"))==null){
            information.setFileName(doc.get("filename"));
        }else{
            information.setFileName(highlighter.getBestFragment(analyzer, "filename", doc.get("filename")));
        }
        informations.add(information);
    }

3.    更新索引。

Lucene索引的更新为调用IndexWriter的updateDocument方法，该方法接受两个参数，第一个Term词向量，第二个为新增的文档，可以是单个文档，也可以是批量文档。Lucene会先根据Term删除索引库中原有的文档，再向索引库中添加新的document。Term的第一参数表示要在文档的哪个Field上查找，第二个代表查询关键字。

	writer.updateDocument(new Term("path", file.getPath()), doc);

4.    删除索引。

Lucene索引的删除的可以根据Query对象来删除，也可以根据Term来删除，也可以一次删除全部索引。

	//根据Term删除
	writer.deleteDocuments(new Term("path", file.getPath()));
	//删除全部索引
	writer.deleteAll();
