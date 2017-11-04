Whoosh是一个用于索引文本和搜索索引的类和函数库。它允许你为你的内容开发自定义的搜索引擎。例如，如果您正在创建博客软件，您可以使用Whoosh添加一个搜索功能来允许用户搜索博客条目。  

### 简单介绍
```python
>>> from whoosh.index import create_in
>>> from whoosh.fields import *
>>> schema = Schema(title=TEXT(stored=True), path=ID(stored=True), content=TEXT)
>>> ix = create_in("indexdir", schema)
>>> writer = ix.writer()
>>> writer.add_document(title=u"First document", path=u"/a",
...                     content=u"This is the first document we've added!")
>>> writer.add_document(title=u"Second document", path=u"/b",
...                     content=u"The second one is even more interesting!")
>>> writer.commit()
>>> from whoosh.qparser import QueryParser
>>> with ix.searcher() as searcher:
...     query = QueryParser("content", ix.schema).parse("first")
...     results = searcher.search(query)
...     results[0]
...
{"title": u"First document", "path": u"/a"}
```

### `Index`和`Schema`对象
要开始使用Whoosh，需要一个索引(Index)对象。第一次创建索引时，必须定义索引的模式(Schema)。该模式列出索引中的字段。字段是索引中每个文档的信息，例如标题或文本内容。可以对字段进行索引(意味着可以搜索)和/或存储(意味着检索的值与结果返回;这对于像标题这样的字段很有用)。　　
该模式有两个字段:“标题”和“内容”:　　
```python
from whoosh.fields import Schema, TEXT

schema = Schema(title=TEXT, content=TEXT)
```
在创建索引时，只需要创建模式一次。该模式被存储在索引中。  
当您创建Schema对象时，您可以使用关键字参数来将字段名映射到字段类型。字段及其类型的列表定义了索引和可搜索的内容。Whoosh提供了一些非常有用的预定义字段类型，您可以很容易地创建自己的字段类型。  
* `whoosh.fields.ID`
* `whoosh.fields.STORED`
* `whoosh.fields.KEYWORD`
* `whoosh.fields.TEXT`
* `whoosh.fields.NUMERIC`
* `whoosh.fields.NUMERIC`
* `whoosh.fields.BOOLEAN`
* `whoosh.fields.DATETIME`
* `whoosh.fields.NGRAM and whoosh.fields.NGRAMWORDS`  

(作为一种快捷方式，如果您不需要向字段类型传递任何参数，您只需提供类名，Whoosh将为您实例化该对象。)
```python
from whoosh.fields import Schema, STORED, ID, KEYWORD, TEXT

schema = Schema(title=TEXT(stored=True), content=TEXT,
                path=ID(stored=True), tags=KEYWORD, icon=STORED)
```  

一旦您有了schema，您就可以使用`create_in`来创建一个索引:
```python
import os.path
from whoosh.index import create_in

if not os.path.exists("index"):
    os.mkdir("index")
ix = create_in("index", schema)
```
在创建了索引之后，可以使用`open_dir`便利函数打开它:  
```python
from whoosh.index import open_dir

ix = open_dir("index")
```

### `IndexWriter`对象  
好了，我们有了一个Index对象，现在我们可以开始添加文档了。Index对象的`writer()`方法返回一个IndexWriter对象，该对象允许您将文档添加到索引中。IndexWriter的`add document(kwargs)`方法接受将字段名映射到值的关键字参数:  
```python
writer = ix.writer()
writer.add_document(title=u"My document", content=u"This is my document!",
                    path=u"/a", tags=u"first short", icon=u"/icons/star.png")
writer.add_document(title=u"Second try", content=u"This is the second example.",
                    path=u"/b", tags=u"second short", icon=u"/icons/sheep.png")
writer.add_document(title=u"Third time's the charm", content=u"Examples are many.",
                    path=u"/c", tags=u"short", icon=u"/icons/book.png")
writer.commit()
```
有两个注意点:  
* 你不需要为每一个字段都填入一个值。Whoosh不关心你是否从文档中删除了一个字段。  
* 索引文本字段必须传递一个unicode值。存储但未被索引的字段(存储字段类型)可以通过任何可选的对象。  

如果您有一个索引和存储的文本字段，您可以索引一个unicode值，但是如果有必要的话，可以存储一个不同的对象(通常不是，但有时这是非常有用的):  
```python
writer.add_document(title=u"Title to be indexed", _stored_title=u"Stored title")
```
在IndexWriter上调用`commit()`将添加的文档保存到索引:  
```python
writer.commit()
```
一旦你的文档被提交到索引，你就可以搜索它们了。  

### `Searcher`对象  
要开始搜索索引，我们需要一个搜索器对象:  
```python
searcher = ix.searcher()
```
你通常会使用一个声明来打开搜索器所以搜索者在你完成它的时候会自动关闭(searcher对象代表了许多打开的文件，所以如果您没有显式地关闭它们，并且系统的收集速度很慢，那么您就可以用文件句柄来运行):  
```python
with ix.searcher() as searcher:
    ...
```
这与下面的方式是等价的:  
```python
try:
    searcher = ix.searcher()
    ...
finally:
    searcher.close()
```
搜索器的search()方法接受一个查询对象。您可以直接构造查询对象，或者使用查询解析器来解析查询字符串。  
例如，这个查询将匹配包含“apple”和“bear”在“内容”字段中的文档:  
```python
# Construct query objects directly

from whoosh.query import *
myquery = And([Term("content", u"apple"), Term("content", "bear")])
```
要解析查询字符串，您可以使用`qparser`模块中的默认查询解析器。对`QueryParser`构造函数的第一个参数是搜索的默认字段。这通常是“正文文本”字段。第二个可选参数是用于理解如何解析字段的模式:  
```python
# Parse a query string

from whoosh.qparser import QueryParser
parser = QueryParser("content", ix.schema)
myquery = parser.parse(querystring)
```
有了搜索器和查询对象之后，就可以使用Searcher的search()方法来运行查询并得到一个结果对象:  
```python
>>> results = searcher.search(myquery)
>>> print(len(results))
1
>>> print(results[0])
{"title": "Second try", "path": "/b", "icon": "/icons/sheep.png"}
```
默认的QueryParser实现了与Lucene非常相似的查询语言。它让您可以将术语与元素的术语联系起来，也可以将分组术语与括号、范围、前缀和wilcard查询联系起来，并指定不同的字段来搜索。默认情况下，它与子句连接在一起(因此，默认情况下，您指定的所有术语必须在文档的文档中匹配):  
```python
>>> print(parser.parse(u"render shade animate"))
And([Term("content", "render"), Term("content", "shade"), Term("content", "animate")])

>>> print(parser.parse(u"render OR (title:shade keyword:animate)"))
Or([Term("content", "render"), And([Term("title", "shade"), Term("keyword", "animate")])])

>>> print(parser.parse(u"rend*"))
Prefix("content", "rend")
```
Whoosh包含了处理搜索结果的额外特性，例如：  
* 根据索引字段的值对结果进行排序，而不是通过relelvance。
* 在原始文档的摘录中突出显示搜索词。
* 根据发现的最上面的几个文档来扩展查询项。
* 对结果进行分页(如“显示结果1-20，第4页1”)。
