现在的软件项目都不是独立的一个项目，都是多系统协调工作。这样的话就涉及到系统间的通讯，通讯就会跟报文传输扯上关系。系统间使用怎样的报文格式进行传输呢？有的使用固定长度格式报文；有的使用变长格式报文；有的使用XML格式报文。

## XML简介

XML（Extensible Markup Language，可扩展标记语言），是一种通用的数据交换格式。

特性：

- xml具有平台无关性，是一门独立的标记语言
- xml具有自我描述性

XML文档节点的类型主要有：

1. document：文档，代表整个文档（DOM树的根节点）
2. element：元素/标签/标注/节点
3. attribute：属性
4. PCDATA（Parsed Character Data）：文本
5. comment：注释
6. DOCTYPE：主要验证文档内容的正确性
7. ENTITIES：实体
8. CDATA（Character Data）：代表文档中的CDATA区段，文本不会被解析器解析。



基本语法：

```
1. XML文档声明：
    <?xml version="1.0" encoding="UTF-8"?> 

2. 标记（元素/标签/节点）
    XML文档，由一个个的标记组成。
	语法：
		开始标记：<标记名称>
		结束标记：</标记名称>
		
        标记名称：	自定义名称，必须遵守以下命名规则：
                    1.名称可以含字母、数字以及其他的字符
                    2.名称不能以数字或者标点符号开始
                    3.名称不能以字符“xml”（或者XML、Xml）开始
                    4.名称不能包含空格，不能包含冒号（:），因为冒号是用于命名空间的保留字
                    5.名称区分大小写

        标记内容：开始标记与结束标记之间的内容
        
	例如，我们通过标记，描述一个人名：
    	<name>李伟杰<name>

3. 一个XML文档中，必须有且仅有一个根标记。
   正例:
     <names>
       <name>张三</name>
       <name>李四</name>
     </names>
   反例:
     <name>李四</name>
     <name>麻子</name>

4. 标记可以嵌套，但是不允许交叉嵌套。

5. 标记名称允许重复。

6. 标记除了开始标记、结束标记和标记内容以外，还有属性。
	标记中的属性，在标记开始时描述，由属性名和属性值组成。

	格式：
		在开始标记中，描述属性。
		可以包含0-n个属性，每一个属性是一个键值对。
		属性名不允许重复，键与值之间使用等号连接，多个属性之间使用空格分隔。
		属性值必须被引号引住。

	例：
         <persons>
           <person id="10001" groupid="1">
             <name>李四</name>
             <age>18</age>
           </person>
           <person id="10002" groupid="1">
             <name>李四</name>
             <age>20</age>
           </person>
         </persons>

7. 注释
	注释不能写在文档声明前，不能嵌套注释。
	
	格式：
		注释开始：	<!--
		注释结束：	-->

8. 命名空间
	可避免标记命名冲突。
	
9. CDATA
	字符数据，<![CDATA[字符数据]]>，字符数据不进行转义。

10. 实体
	使用方式为：	&实体
	XML中有五个预定义的实体:
        实体名称	含义
        <			小于
        >			大于
        &			和号
        '			单引号
        "			引号
```

## XML约束

### DTD约束

DTD （DocType Definition），规定了文档中所使用的元素、实体、元素的属性、元素与实体之间的关系。可以根据 DTD 检查数据，以验证其是否符合规定和要求，这可以保证数据的正确和有效。

DTD主要定义方式：

1.内部定义法，DTD放在XML文件内部。

**book.xml：**

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE bookstore [
        <!ELEMENT bookstore (book+)>
        <!ELEMENT book (bookname,author,price)>
        <!ATTLIST book id CDATA #REQUIRED>
        <!ELEMENT bookname (#PCDATA)>
        <!ELEMENT author (#PCDATA)>
        <!ELEMENT price (#PCDATA)>]>
<bookstore>
    <book id="1">
        <bookname>带你飞培训教程</bookname>
        <author>huangjinjin</author>
        <price>1.00元</price>
    </book>
    <book id="2">
        <bookname>降龙十八讲秘籍</bookname>
        <author>洪七公</author>
        <price>0.01元</price>
    </book>
</bookstore>
```

2.外部定义

**bookstore.dtd：**

```dtd
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE bookstore [
        <!ELEMENT bookstore (book+)>
        <!ELEMENT book (bookname,author,price)>
        <!ATTLIST book id CDATA #REQUIRED>
        <!ELEMENT bookname (#PCDATA)>
        <!ELEMENT author (#PCDATA)>
        <!ELEMENT price (#PCDATA)>]>
```

**bookstore.xml：**

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE bookstore SYSTEM "bookstore.dtd">
<bookstore>
    <book id="1">
        <bookname>带你飞培训教程</bookname>
        <author>huangjinjin</author>
        <price>1.00元</price>
    </book>
    <book id="2">
        <bookname>降龙十八讲秘籍</bookname>
        <author>洪七公</author>
        <price>0.01元</price>
    </book>
</bookstore>
```

### Schema约束

XML Schema（XML Schema Definition，XSD）。与DTD不同的是，Schema通过XML语法定义文档结构，可以定义更多的数据类型和更严格的限制。

XML Schema对XML文件的主要约定有：

- 定义可出现在XML文档中的元素
- 定义可出现在XML文档中的属性
- 定义哪个元素是子元素
- 定义子元素的次序
- 定义子元素的数目
- 定义元素是否为空，或者是否可包含文本
- 定义元素和属性的数据类型
- 定义元素和属性的默认值以及固定值

使用Schema的优点：

- XML Schema可针对未来的需求进行扩展
- XML Schema更完善，功能更强大
- XML Schema基于XML编写
- XML Schema支持完善的数据类型和限制
- XML Schema支持命名空间



**bookstore.xsd**

```scheme
<?xml version="1.0" encoding="UTF-8" ?>
<schema xmlns="http://www.w3.org/2001/XMLSchema"
        targetNamespace="http://www.mybook.org/BookSchema"
        elementFormDefault="qualified">
    <!--定义一个标签-->
    <element name="bookstore">
        <!--复合类型,就是该标签含有子标签-->
        <complexType>
            <!--含有不限制个数的子标签-->
            <sequence maxOccurs="unbounded">
                <!--定义一个子标签-->
                <element name="book">
                    <complexType>
                        <sequence>
                            <element name="bookname" type="string"></element>
                            <element name="author" type="string"></element>
                            <element name="price" type="string"></element>
                        </sequence>
                        <attribute name="id" type="string"></attribute>
                    </complexType>
                </element>
            </sequence>
        </complexType>
    </element>
</schema>
```

**bookstore.xml**

```xml
<?xml version="1.0" encoding="utf-8" ?>
<bookstore
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns="http://www.mybook.org/BookSchema"
        xsi:schemaLocation="http://www.mybook.org/BookSchema  book.xsd">
    <book id="1">
        <bookname>带你飞培训教程</bookname>
        <author>huangjinjin</author>
        <price>1.00元</price>
    </book>
    <book id="2">
        <bookname>降龙十八讲秘籍</bookname>
        <author>洪七公</author>
        <price>0.01元</price>
    </book>
</bookstore>
```

## XML解析

### SAX解析

Simple APIs for XML，即XML简单应用程序接口。SAX不是官方标准，但它是XML社区事实上的标准，几乎所有的XML解析器都支持它。

SAX解析器逐行读取XML文本解析，每当解析到一个标签的开始/结束/内容/属性时，触发事件。SAX解析器采用的是事件驱动机制，我们可以编写程序在这些事件发生时，进行相应的处理。

优点：

- 能够立即开始分析，而不是等待所有的数据被处理
- 逐行加载，节省内存，有助于解析大于系统内存的文档。
- 有时不必解析整个文档，它可以在某的条件得到满足时停止解析。

缺点：

- 单行解析，无法定位文档层次，无法同时访问同一文档的不同部分数据（因为逐行解析，当解析第n行时，第n-1行已经被释放了，无法在进行操作了）
- 无法得知事件发生时元素的层次，只能自己维护节点的父/子关系
- 只读解析方式，无法修改XML文档的内容

案例：

**Book.java：**

```java
package com.kkb.bean;

/**
 * @Author: 马盛杰
 * @Description:Book实体类
 * @Date Created in 2021-09-09 16:00
 * @Modified By:
 */
public class Book {
    private String id;
    private String bookname;
    private String author;
    private String price;

    public Book() {
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getBookname() {
        return bookname;
    }

    public void setBookname(String bookname) {
        this.bookname = bookname;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public String getPrice() {
        return price;
    }

    public void setPrice(String price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Book{" +
                "id='" + id + '\'' +
                ", bookname='" + bookname + '\'' +
                ", author='" + author + '\'' +
                ", price='" + price + '\'' +
                '}';
    }
}
```

**TestSAX.java：**

```java
package com.kkb.test;

import com.kkb.bean.Book;
import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import java.util.ArrayList;

/**
 * @Author: 马盛杰
 * @Description:测试SAX解析XML文档
 * @Date Created in 2021-09-09 15:28
 * @Modified By:
 */
public class TestSAX {
    public static void main(String[] args) {
        // 获取Sax解析工程对象
        SAXParserFactory factory = SAXParserFactory.newInstance();
        try {
            SAXParser saxParser = factory.newSAXParser();
            // 新建XML处理器
            DefaultHandler handler = new BookSAXParserHandler();
            saxParser.parse("D:\\Java\\IdeaProjects\\Demo_XML\\src\\book.xml", handler);
            System.out.println("共有" + ((BookSAXParserHandler) handler).getBookList().size() + "本书，详细信息如下：");
            for (Book book : ((BookSAXParserHandler) handler).getBookList()) {
                System.out.print("id=" + book.getId() + "，");
                System.out.print("bookname=" + book.getBookname() + "，");
                System.out.print("author=" + book.getAuthor() + "，");
                System.out.println("price=" + book.getPrice() + "。");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static class BookSAXParserHandler extends DefaultHandler{
        private String value = null;
        private Book book = null;
        private int bookIndex = 0;

        private ArrayList<Book> bookList = new ArrayList<>();

        public ArrayList<Book> getBookList() {
            return bookList;
        }

        @Override
        public void startDocument() throws SAXException {
            super.startDocument();
            System.out.println("SAX解析开始");
        }

        @Override
        public void endDocument() throws SAXException {
            super.endDocument();
            System.out.println("SAX解析结束");
        }

        @Override
        public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
            super.startElement(uri, localName, qName, attributes);
            if (qName.equals("book")) {
                bookIndex++;
                // 创建一个book对象
                book = new Book();
                // 开始解析book元素的属性
                System.out.println("============开始遍历某一本书的内容=================");
                // 不知道book元素下属性的名称以及个数，如何获取属性名以及属性值
                int num = attributes.getLength();
                for (int i = 0; i < num; i++) {
                    System.out.print("book元素的第" + (i + 1) + "个属性名是：" + attributes.getQName(i));
                    System.out.println(", 属性值是：" + attributes.getValue(i));
                    if (attributes.getQName(i).equals("id")) {
                        book.setId(attributes.getValue(i));
                    }
                }
            }else {
                System.out.println("节点名是：" + qName +", ");
            }

        }

        @Override
        public void endElement(String uri, String localName, String qName) throws SAXException {
            super.endElement(uri, localName, qName);
            // 判断是否针对一本书已经遍历结束
            if (qName.equals("book")) {
                bookList.add(book);
                book = null;
                System.out.println("===========结束遍历某一本书的内容=================");
            } else if (qName.equals("bookname")) {
                book.setBookname(value);
            } else if (qName.equals("author")) {
                book.setAuthor(value);
            }  else if (qName.equals("price")) {
                book.setPrice(value);
            }
        }

        @Override
        public void characters(char[] ch, int start, int length) throws SAXException {
            super.characters(ch, start, length);
            value = new String(ch, start, length);
            if (!value.trim().equals("")) {
                System.out.println("节点值是：" + value);
            }
        }
    }
}
```

### DOM解析

Document Object Model，即文档对象模型，是W3C组织推荐处理XML的一种方式。通常需要加载整个文档并在内存中建立文档树模型，程序员可以通过操作文档树，来完成数据的获取、修改、删除等。

**优点：**

- 文档在内存中加载形成树结构，有助于更好的理解层级结构，代码容易编写
- 访问是双向的，可以在任何时候在树中双向解析数据

**缺点：**

- 一次性读取，对内存的耗费比较大

案例：

**TestDOM.java：**

```java
package com.kkb.test;

import org.w3c.dom.Document;
import org.w3c.dom.NamedNodeMap;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

/**
 * @Author: 马盛杰
 * @Description: 测试DOM解析XML
 * @Date Created in 2021-09-09 17:24
 * @Modified By:
 */
public class TestDOM {
    public static void main(String[] args) {
        // 创建一个DocumentBuilderFactory的对象
        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
        // 创建一个DocumentBuilder的对象
        try {
            // 创建DocumentBuilder对象
            DocumentBuilder db = dbf.newDocumentBuilder();
            // 通过DocumentBuilder对象的parser方法加载books.xml文件到当前项目下
            Document document = db.parse("D:\\Java\\IdeaProjects\\Demo_XML\\src\\book1.xml");
            // 获取所有book节点的集合
            NodeList bookList = document.getElementsByTagName("book");
            // 通过nodelist的getLength()方法可以获取bookList的长度
            System.out.println("一共有" + bookList.getLength() + "本书");
            // 遍历每一个book节点
            for (int i = 0; i < bookList.getLength(); i++) {
                System.out.println("=================下面开始遍历第" + (i + 1) + "本书的内容=================");
                // 通过 item(i)方法 获取一个book节点，nodelist的索引值从0开始
                Node book = bookList.item(i);
                // 获取book节点的所有属性集合
                NamedNodeMap attrs = book.getAttributes();
                System.out.println("第 " + (i + 1) + "本书共有" + attrs.getLength() + "个属性");
                // 遍历book的属性
                for (int j = 0; j < attrs.getLength(); j++) {
                    // 通过item(index)方法获取book节点的某一个属性
                    Node attr = attrs.item(j);
                    // 获取属性名 值
                    System.out.print("属性名：" + attr.getNodeName()+", 属性值" + attr.getNodeValue());
                }
                // 解析book节点的子节点
                NodeList childNodes = book.getChildNodes();
                // 遍历childNodes获取每个节点的节点名和节点值
                System.out.println("第" + (i + 1) + "本书共有" + childNodes.getLength() + "个子节点");
                for (int k = 0; k < childNodes.getLength(); k++) {
                    // 区分出text类型的node以及element类型的node
                    if (childNodes.item(k).getNodeType() == Node.ELEMENT_NODE) {
                        // 获取了element类型节点的节点名
                        System.out.print("第" + (k + 1) + "个节点的节点名：" + childNodes.item(k).getNodeName());
                        // 获取了element类型节点的节点值
                        System.out.println("--节点值是：" + childNodes.item(k).getFirstChild().getNodeValue());
                    }
                }
                System.out.println("======================结束遍历第" + (i + 1) + "本书的内容=================");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```

## Java中常用的XML解析框架

### JDOM

目的是成为Java特定文档模型，它简化与XML的交互并且比使用DOM实现更快。由于是第一个Java特定模型，JDOM一直得到大力推广和促进。

JDOM文档声明其目的是“使用20%（或更少）的精力解决80%（或更多）Java/XML问题”。

优点：

- 使用具体类而不是接口，简化了DOM的API。
- 大量使用了Java集合类，方便了Java开发人员。

缺点：

- 没有较好的灵活性
- 性能不是很优异

案例：

**TestJDom.java：**

```java
package com.kkb.test;

import java.io.File;
import java.io.FileInputStream;
import java.util.List;
// 需要先引入jdom2的jar包
import org.jdom2.Document;
import org.jdom2.Element;
import org.jdom2.input.SAXBuilder;

/**
 * @Author: 马盛杰
 * @Description: 测试JDOM解析XML文档
 * @Date Created in 2021-09-09 17:48
 * @Modified By:
 */
public class TestJDom {
    public static void main(String[] args) {
        try {
            // 创建一个SAXBuilder对象
            SAXBuilder sb = new SAXBuilder();
            File file = new File("D:\\Java\\IdeaProjects\\Demo_XML\\src\\book.xml");
            FileInputStream in = new FileInputStream(file);
            // 构造文档对象
            Document doc = sb.build(in);
            // 获取根元素bookstore
            Element root = doc.getRootElement();
            // 取名字为book的所有元素
            List<Element> books = root.getChildren("book");
            for (int i = 0; i < books.size(); i++) {
                Element e = books.get(i);
                List<Element> sube = e.getChildren();
                System.out.println("第" + (i + 1) + "本书信息：");
                e = sube.get(0);
                System.out.println(e.getName() + "=" + e.getText());
                e = sube.get(1);
                System.out.println(e.getName() + "=" + e.getText());
                e = sube.get(2);
                System.out.println(e.getName() + "=" + e.getText());

            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}
```

### DOM4J

#### 解析本地文件案例

**TestDom4j.java：**

```java
package com.kkb.test;

import com.kkb.bean.Book;
//需要先引入dom4j的jar包
import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.FileInputStream;
import java.util.List;

/**
 * @Author: 马盛杰
 * @Description:测试DOM4解析XML文档
 * @Date Created in 2021-09-09 21:30
 * @Modified By:
 */
public class TestDom4j {
    public static void main(String[] args) throws Exception {
        //1.创建XML读取工具对象
        SAXReader reader = new SAXReader();
        //2.通过读取工具,读取XML文档的输入流,并得到文档对象
        Document document = reader.read(new FileInputStream("D:\\Java\\IdeaProjects\\Demo_XML\\src\\book.xml"));
        //3.通过文档对象,获取文档的根节点对象
        Element root = document.getRootElement();
        //4.获取根节点的所有子节点
        List<Element> elements = root.elements();
        Book book = null;
        for (Element ele : elements) {
            //1.获取id属性值
            String id = ele.attribute("id").getValue();
            //2.获取子节点bookname的内容
            String bookname = ele.elementText("bookname");
            //3.获取子节点author的内容
            String author = ele.elementText("author");
            //4.获取子节点price的内容
            String price = ele.elementText("price");
            //5.构建book对象并打印其信息
            book = new Book(id,bookname,author,price);
            System.out.println(book);
        }
    }
}
```

#### 解析web响应案例

**TestDom4j_web.java：**

```java
package com.kkb.test;

//需要先引入dom4j的jar包
import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.InputStream;
import java.net.URL;

/**
 * @Author: 马盛杰
 * @Description:测试DOM4解析XML文档
 * @Date Created in 2021-09-09 21:30
 * @Modified By:
 */
public class TestDom4j_web {
    public static void main(String[] args) throws Exception {
        //1.创建XML读取工具对象
        SAXReader reader = new SAXReader();
        //2.通过读取工具,读取网络连接的输入流,并得到文档对象
        String phone = "18516955565";
        URL url = new URL("http://apis.juhe.cn/mobile/get?phone=" + phone + "&dtype=xml&key=9f3923e8f87f1ea50ed4ec8c39cc9253");
        InputStream inputStream = url.openConnection().getInputStream();
        Document document = reader.read(inputStream);
        //3.通过文档对象,获取文档的根节点对象
        Element root = document.getRootElement();
        //4.解析内容
        String code = root.elementText("resultcode");
        if ("200".equals(code)) {
            Element result = root.element("result");
            String province = result.elementText("province");
            String city = result.elementText("city");
            if (province.equals(city)) {
                System.out.println("手机号码归属地为：" + city);
            } else {
                System.out.println("手机号码归属地为：" + province + " " + city);
            }
        } else {
            System.out.println("请输入正确的手机号码");
        }
    }
}
```

#### XPATH解析

```
路径表达式：通过路径快速查找一个或一组元素
    1.	/	:	从根节点开始查找
    2.	//	:	从发起查找的节点位置 查找后代节点
    3.	.	:	查找当前节点
    4.	..	:	查找父节点
    5.	@	:	选择属性
                属性使用方式:
                [@属性名='值']
                [@属性名>'值']
                [@属性名<'值']
                [@属性名!='值']
     books:  路径： //book[@id='1']//name
     books
     	book id=1
     		name
     		info
         book id=2
         	name
         	info                
```

**测试类TestDom4j_xpath.java：**

```java
package com.kkb.test;

//需要先引入dom4j的jar包和jaxen的jar包
import com.kkb.bean.Book;
import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.Node;
import org.dom4j.io.SAXReader;

import java.io.FileInputStream;
import java.util.List;

/**
 * @Author: 马盛杰
 * @Description:测试DOM4解析XML文档
 * @Date Created in 2021-09-09 21:30
 * @Modified By:
 */
public class TestDom4j_xpath {
    public static void main(String[] args) throws Exception {
        //1.创建XML读取工具对象
        SAXReader reader = new SAXReader();
        //2.通过读取工具,读取XML文档的输入流,并得到文档对象
        Document document = reader.read(new FileInputStream("D:\\Java\\IdeaProjects\\Demo_XML\\src\\book.xml"));
        //3.通过文档对象,获取文档的根节点对象
        Element root = document.getRootElement();
        List<Node> books = root.selectNodes("//book");
        Book book = null;
        for(Node bookNode: books){
            String id = ((Element) bookNode).attribute("id").getValue();
            String bookname = bookNode.selectSingleNode("bookname").getText();
            String author = bookNode.selectSingleNode("author").getText();
            String price = bookNode.selectSingleNode("price").getText();
            book = new Book(id,bookname,author,price);
            System.out.println(book);
        }
    }
}
```

### XStream

XStream是个很强大的工具，能将Java对象和XML之间互相转化。XStream不在意Java类中成员变量是私有还是公有，也不在乎是否有默认构造函数。XStream也支持注解方式，这些都是为了简化输出而设计。

XStream常用注解说明有：

- @XStreamAlias 注解可在类与属性上，设置别名；
- @XStreamAliasType 注解设置在类，设置类型别名；
- @XStreamAsAttribute 注解设置在属性上，作用是将类内成员作为父节点属性输出；
- @XStreamConverter 注解设置在属性上，注入转化器；
- @XStreamConverters 注解设置在属性上，注入多个转化器；
- @XStreamImplicit 常用在集合属性上，表明只把集合里的元素列序号到 XML中；
- @XStreamInclude
- @XStreamOmitField 注解可设置在属性上，表明该属性不会被序列化到 XML 中。

**Person.java：**

```java
package com.kkb.bean;

import com.thoughtworks.xstream.annotations.XStreamAlias;
import com.thoughtworks.xstream.annotations.XStreamAliasType;
import com.thoughtworks.xstream.annotations.XStreamAsAttribute;
import com.thoughtworks.xstream.annotations.XStreamOmitField;

/**
 * @Author: 马盛杰
 * @Description:Person实体类
 * @Date Created in 2021-09-10 0:14
 * @Modified By:
 */
@XStreamAliasType("p")
public class Person {
    @XStreamAsAttribute
    private int id;

    @XStreamAlias("n")
    private String name;

    @XStreamAlias("a")
    @XStreamOmitField
    private int age;

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```

**TestXStream.java：**

```java
package com.kkb.test;

import com.kkb.bean.Person;
// 不光需要引入XStream的jar包，还需要引入xpp3_min和xmlpull的jar包，否则会运行报错
import com.thoughtworks.xstream.XStream;

/**
 * @Author: 马盛杰
 * @Description:测试XStream进行XML序列化与反序列化
 * @Date Created in 2021-09-10 0:12
 * @Modified By:
 */
public class TestXStream {
    public static void main(String[] args) {
        Person person=new Person();
        person.setName("独孤求败");
        person.setAge(100);
        XStream xstream = new XStream();
        // 进行注解的检测  如果没有这行注解将不起作用
        xstream.autodetectAnnotations(true);
        //XML序列化
        String xml = xstream.toXML(person);
        System.out.println(xml);
        //XML反序列化
        Person person1=(Person)xstream.fromXML(xml);
        System.out.println(person1.getName());
        System.out.println(person1.getAge());
    }
}
```

输出：

```
<com.kkb.bean.Person id="0">
  <n>独孤求败</n>
</com.kkb.bean.Person>
独孤求败
0
```

## Java生成XML

**TestGenXML.java：**

```java
package com.kkb.test;

import org.dom4j.Document;
import org.dom4j.DocumentHelper;
import org.dom4j.Element;
import org.dom4j.io.XMLWriter;

import java.io.FileOutputStream;

/**
 * @Author: 马盛杰
 * @Description:测试DOM4J生成XML
 * @Date Created in 2021-09-10 11:01
 * @Modified By:
 */
public class TestGenXML {
    public static void main(String[] args) throws Exception {
        //1.    通过文档帮助器，创建空的文档对象
        Document doc = DocumentHelper.createDocument();
        //2.    向文档对象中，添加第一个节点（根节点）
        Element books = doc.addElement("books");
        //3.    向根节点中加入两个子节点
        for (int i = 0; i < 2; i++) {
            //1.添加子节点book
            Element book = books.addElement("book");
            //2.book节点添加id属性,值为i
            book.addAttribute("id", i + "");
            //3.book节点添加name子节点,内容是"金苹果"
            book.addElement("name").setText("金苹果");
            //4.book节点添加info子节点,内容是"农民辛勤种植金苹果的历程"
            book.addElement("info").setText("农民辛勤种植金苹果的历程");
        }
        //4.  创建文件的输出流
        FileOutputStream fos = new FileOutputStream("D:\\Java\\IdeaProjects\\Demo_XML\\src\\books.xml");
        //5.  将文件输出流 , 转换为XML文档输出流
        XMLWriter xw = new XMLWriter(fos);
        //6.  写出XML文档
        xw.write(doc);
        //7.  释放资源
        xw.close();
        System.out.println("代码执行完毕");
    }
}
```

## JSON

```json
JavaScript Object Natation，JS对象简谱，是一种轻量级的数据交换格式。
	一个对象，由一个大括号表示。
		括号中通过键值对来描述对象的属性
		格式：
			键与值之间使用冒号连接，多个键值对之间使用逗号分隔。
			键值对的键应使用引号引住（通常Java解析时，键不使用引号会报错，而JS能正确解析）。
			键值对中的值，可以是JS中的任意类型数据
			
	数组格式：
		在JSON格式中可以与对象互相嵌套
		[元素1,元素2...]
	
	案例：
        {
            "name":"伟杰老师",
            "age":18,
            "pengyou":["张三","李四","王二","麻子",{
                "name":"野马老师",
                "info":"像匹野马一样狂奔在技术钻研的道路上"}],
            "heihei":{
            	"name":"大长刀",
            	"length":"40m"}
         }
```

## JSON的序列化即反序列化

### Gson

**TestGson.java：**

```java
package com.kkb.test;

// 先导入Gson的jar包
import com.google.gson.Gson;
import com.kkb.bean.Book;

/**
 * @Author: 马盛杰
 * @Description:测试Gson实现序列化与反序列化
 * @Date Created in 2021-09-10 11:33
 * @Modified By:
 */
public class TestGson {
    public static void main(String[] args) {
        Book book = new Book("1","金苹果","李伟杰","10.1元");
        String json = new Gson().toJson(book);
        System.out.println(json);

        Book book1 = new Gson().fromJson(json, Book.class);
        System.out.println(book1);
    }
}
```

输出：

```
{"id":"1","bookname":"金苹果","author":"李伟杰","price":"10.1元"}
Book{id='1', bookname='金苹果', author='李伟杰', price='10.1元'}
```

### FastJson

**TestFastJson.java：**

```java
package com.kkb.test;

// 先导入fastjson的jar包
import com.alibaba.fastjson.JSON;
import com.kkb.bean.Book;

/**
 * @Author: 马盛杰
 * @Description:测试FastJson实现JSON序列化与反序列化
 * @Date Created in 2021-09-10 11:44
 * @Modified By:
 */
public class TestFastJson {
    public static void main(String[] args) {
        Book book = new Book("1","金苹果","李伟杰","10.1元");
        String json = JSON.toJSONString(book);
        System.out.println(json);

        Book book1 = JSON.parseObject(json,Book.class);
        System.out.println(book1);
    }
}
```





