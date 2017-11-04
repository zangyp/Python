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
