---
id: 344
title: XPath轴及运算符
date: 2006-04-27T09:34:30+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=344
permalink: /xpath-axes-and-operators/
categories:
  - 我的世界
tags:
  - 'c#'
---
An axis defines a node-set relative to the current node. 
  
轴定义了相对于当前节的节集

<div id="toc_container" class="toc_white no_bullets">
  <p class="toc_title">
    文章目录
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#The_XML_Example_Document">The XML Example Document</a>
    </li>
    <li>
      <a href="#XPath_Axes">XPath Axes</a>
    </li>
    <li>
      <a href="#Location_Path_Expression">Location Path Expression</a><ul>
        <li>
          <a href="#Examples">Examples</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#XPath_Operators">XPath Operators</a>
    </li>
    <li>
      <a href="#The_XML_Example_Document-2">The XML Example Document</a>
    </li>
    <li>
      <a href="#Selecting_Nodes">Selecting Nodes</a>
    </li>
    <li>
      <a href="#Select_all_book_Nodes">Select all book Nodes</a>
    </li>
    <li>
      <a href="#Select_the_First_book_Node">Select the First book Node</a>
    </li>
    <li>
      <a href="#Select_the_prices">Select the prices</a>
    </li>
    <li>
      <a href="#Selecting_price_Nodes_with_Price35">Selecting price Nodes with Price>35</a>
    </li>
    <li>
      <a href="#Selecting_title_Nodes_with_Price35">Selecting title Nodes with Price>35</a>
    </li>
  </ul>
</div>

## <span id="The_XML_Example_Document">The XML Example Document</span> {#thexmlexampledocument}

XML举例文档

We will use the following XML document in the examples below. 
  
我么将使用该XML文档进行下面的举例说明

> <?xml version="1.0" encoding="ISO-8859-1"?>
> 
> <bookstore>
> 
> <book> 
    
> 
> 
> <title lang="eng">
>   Harry Potter
> </title><price>29.99</price> </book>
> 
> <book> 
    
> 
> 
> <title lang="eng">
>   Learning XML
> </title><price>39.95</price> </book>
> 
> </bookstore>

* * *

## <span id="XPath_Axes">XPath Axes</span> {#xpathaxes}

XPath轴

<table border="1" cellspacing="0" class="ex" id="table1">
  <tr>
    <td valign="top" width="38%">
      <strong>轴名</strong>
    </td>
    
    <td valign="top" width="62%">
      <strong>结果</strong>
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      ancestor
    </td>
    
    <td valign="top">
      Selects all ancestors (parent, grandparent, etc.) of the current node[选择了当前节的所有祖（父，祖父，等等）]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      ancestor-or-self
    </td>
    
    <td valign="top">
      Selects all ancestors (parent, grandparent, etc.) of the current node and the current node itself[选择当前节的所有祖并且还有当前节自己]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      attribute
    </td>
    
    <td valign="top">
      Selects all attributes of the current node[选择所有当前节的属性]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      child
    </td>
    
    <td valign="top">
      Selects all children of the current node[选择所有当前节的子]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      descendant
    </td>
    
    <td valign="top">
      Selects all descendants (children, grandchildren, etc.) of the current node[选择所有当前节的孙（子，孙子，等等）]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      descendant-or-self
    </td>
    
    <td valign="top">
      Selects all descendants (children, grandchildren, etc.) of the current node and the current node itself[选择当前节的所有孙以及它本身]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      following
    </td>
    
    <td valign="top">
      Selects everything in the document after the closing tag of the current node[选择所有在关闭当前节标签后的所有内容]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      following-sibling
    </td>
    
    <td valign="top">
      Selects all siblings after the current node[选择所有当前节后的兄]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      namespace
    </td>
    
    <td valign="top">
      Selects all namespace nodes of the current node[选择所有当前节的命名空间]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      parent
    </td>
    
    <td valign="top">
      Selects the parent of the current node[选择当前节的父]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      preceding
    </td>
    
    <td valign="top">
      Selects everything in the document that is before the start tag of the current node[选择当前节之前的所有内容]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      preceding-sibling
    </td>
    
    <td valign="top">
      Selects all siblings before the current node[选择所有当前节之前的兄]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      self
    </td>
    
    <td valign="top">
      Selects the current node[选择当前节]
    </td>
  </tr>
</table>

&#8211; &#8211; &#8211; &#8211; &#8211; &#8211;

## <span id="Location_Path_Expression">Location Path Expression</span> {#locationpathexpression}

路径表达试定位

A location path can be absolute or relative. 
  
定位路径可以是绝对的也可以是相对的

An absolute location path starts with a slash ( / ) and a relative location path does not. In both cases the location path consists of one or more steps, each separated by a slash: 
  
绝对定位的路径由(/)开始，而相对定位就不这样。定位的路径由一个或多个步骤所组成，每部分由(/)相分隔：

> An absolute location path:
> 
> /step/step/…
> 
> A relative location path:
> 
> step/step/…

Each step is evaluated against the nodes in the current node-set. 
  
在当前的节集中每步的赋值是逆向的

A step consists of:

  * an axis (defines the tree-relationship between the selected nodes and the current node)
  * a node-test (identifies a node within an axis)[在轴中鉴定节]
  * zero or more predicates (to further refine the selected node-set)[0个或多个谓语可以来更好的选择节]

The syntax for a location step is: 
  
定位的语法

> axisname::nodetest[predicate]

### <span id="Examples">Examples</span> {#examples}

实例

<table border="1" cellspacing="0">
  <tr>
    <td valign="top" width="38%">
      <strong>Example</strong>
    </td>
    
    <td valign="top" width="62%">
      <strong>结果</strong>
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      child::book
    </td>
    
    <td valign="top">
      Selects all book nodes that are children of the current node[选择当前节点下所有为book的子节点]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      attribute::lang
    </td>
    
    <td valign="top">
      Selects the lang attribute of the current node[选择当前节点下所有属性为lang的内容]
    </td>
  </tr>
  
  <tr>
    <td valign="top">
      child::<em></td> 
      
      <td valign="top">
        Selects all children of the current node[选择当前节下所有的子节]
      </td></tr> 
      
      <tr>
        <td valign="top">
          attribute::</em>
        </td>
        
        <td valign="top">
          Selects all attributes of the current node[选择当前节所有的属性]
        </td>
      </tr>
      
      <tr>
        <td valign="top">
          child::text()
        </td>
        
        <td valign="top">
          Selects all text child nodes of the current node[选择当前节点所有子节点的文字]
        </td>
      </tr>
      
      <tr>
        <td valign="top">
          child::node()
        </td>
        
        <td valign="top">
          Selects all child nodes of the current node[选择所有当前节点的子节点]
        </td>
      </tr>
      
      <tr>
        <td valign="top">
          descendant::book
        </td>
        
        <td valign="top">
          Selects all book descendants of the current node[选择当前节点所有为book的孙节点]
        </td>
      </tr>
      
      <tr>
        <td valign="top">
          ancestor::book
        </td>
        
        <td valign="top">
          Selects all book ancestors of the current node[选择所有当前祖节点为book的节点]
        </td>
      </tr>
      
      <tr>
        <td valign="top">
          ancestor-or-self::book
        </td>
        
        <td valign="top">
          Selects all book ancestors of the current node – and the current as well if it is a book node[当前节点和其祖节点为book的节点]
        </td>
      </tr>
      
      <tr>
        <td valign="top">
          child::*/child::price
        </td>
        
        <td valign="top">
          Selects all price grandchildren of the current node[当前节点所有含price的孙子节点]
        </td>
      </tr></tbody> </table> 
      
      <div class="content">
        An XPath expression returns either anode-set, a string, a Boolean, or a number.</p> 
        
        <hr />
        
        <h2 id="xpathoperators">
          <span id="XPath_Operators">XPath Operators</span>
        </h2>
        
        <p>
          Below is a list of the operators that can be used in XPath expressions:
        </p>
        
        <p>
          <t able="" border="1" cellspacing="0" class="ex"></t>
        </p>
        
        <tr>
          <td valign="top" width="15%">
            <strong>Operator</strong>
          </td>
          
          <td valign="top" width="30%">
            <strong>Description</strong>
          </td>
          
          <td valign="top" width="30%">
            <strong>Example</strong>
          </td>
          
          <td valign="top" width="25%">
            <strong>Return value</strong>
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            |
          </td>
          
          <td valign="top">
            Computes two node-sets
          </td>
          
          <td valign="top">
            //book | //cd
          </td>
          
          <td valign="top">
            Returns a node-set with all book and cd elements
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            +
          </td>
          
          <td valign="top">
            Addition
          </td>
          
          <td valign="top">
            6 + 4
          </td>
          
          <td valign="top">
            10
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            –
          </td>
          
          <td valign="top">
            Subtraction
          </td>
          
          <td valign="top">
            6 – 4
          </td>
          
          <td valign="top">
            2
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            *
          </td>
          
          <td valign="top">
            Multiplication
          </td>
          
          <td valign="top">
            6 * 4</p>
          </td>
          
          <td valign="top">
            24
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            div
          </td>
          
          <td valign="top">
            Division
          </td>
          
          <td valign="top">
            8 div 4
          </td>
          
          <td valign="top">
            2
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            =
          </td>
          
          <td valign="top">
            Equal
          </td>
          
          <td valign="top">
            price=9.80
          </td>
          
          <td valign="top">
            true if price is 9.80 <br /> false if price is 9.90
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            !=
          </td>
          
          <td valign="top">
            Not equal
          </td>
          
          <td valign="top">
            price!=9.80
          </td>
          
          <td valign="top">
            true if price is 9.90 <br /> false if price is 9.80
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            <
          </td>
          
          <td valign="top">
            Less than
          </td>
          
          <td valign="top">
            price<9.80
          </td>
          
          <td valign="top">
            true if price is 9.00 <br /> false if price is 9.80
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            <=
          </td>
          
          <td valign="top">
            Less than or equal to
          </td>
          
          <td valign="top">
            price<=9.80
          </td>
          
          <td valign="top">
            true if price is 9.00 <br /> false if price is 9.90
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            >
          </td>
          
          <td valign="top">
            Greater than
          </td>
          
          <td valign="top">
            price>9.80
          </td>
          
          <td valign="top">
            true if price is 9.90 <br /> false if price is 9.80
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            >=
          </td>
          
          <td valign="top">
            Greater than or equal to
          </td>
          
          <td valign="top">
            price>=9.80
          </td>
          
          <td valign="top">
            true if price is 9.90 <br /> false if price is 9.70
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            or
          </td>
          
          <td valign="top">
            or
          </td>
          
          <td valign="top">
            price=9.80 or price=9.70
          </td>
          
          <td valign="top">
            true if price is 9.80 <br /> false if price is 9.50
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            and
          </td>
          
          <td valign="top">
            and
          </td>
          
          <td valign="top">
            price>9.00 and price<9.90
          </td>
          
          <td valign="top">
            true if price is 9.80 <br /> false if price is 8.50
          </td>
        </tr>
        
        <tr>
          <td valign="top">
            mod
          </td>
          
          <td valign="top">
            Modulus (division remainder)
          </td>
          
          <td valign="top">
            5 mod 2
          </td>
          
          <td valign="top">
            1
          </td>
        </tr>
      </div>
      
      <div class="content">
        Let&#8217;s try to learn some basic XPath syntax by looking at some examples. <br /> 让我们来尝试通过观察一些实例来学习基础的XPath语法</p> 
        
        <hr />
        
        <h2 id="thexmlexampledocument">
          <span id="The_XML_Example_Document-2">The XML Example Document</span>
        </h2>
        
        <p>
          We will use the following XML document in the examples below. <br /> 我们将使用下面这个XML文档来进行实例
        </p>
        
        <p>
          &#8220;books.xml&#8221;:
        </p>
        
        <blockquote>
          <p>
            <?xml version="1.0" encoding="ISO-8859-1"?>
          </p>
          
          <p>
            <bookstore>
          </p>
          
          <p>
            <book category="COOKING"> <br /> 
            
            <title lang="en">
              Everyday Italian
            </title>
            
            <br /> <author>Giada De Laurentiis</author> <br /> <year>2005</year> <price>30.00</price> </book>
          </p>
          
          <p>
            <book category="CHILDREN"> <br /> 
            
            <title lang="en">
              Harry Potter
            </title>
            
            <br /> <author>J K. Rowling</author> <br /> <year>2005</year> <price>29.99</price> </book>
          </p>
          
          <p>
            <book category="WEB"> <br /> 
            
            <title lang="en">
              XQuery Kick Start
            </title>
            
            <br /> <author>James McGovern</author> <br /> <author>Per Bothner</author> <br /> <author>Kurt Cagle</author> <br /> <author>James Linn</author> <br /> <author>Vaidyanathan Nagarajan</author> <br /> <year>2003</year> <price>49.99</price> </book>
          </p>
          
          <p>
            <book category="WEB"> <br /> 
            
            <title lang="en">
              Learning XML
            </title>
            
            <br /> <author>Erik T. Ray</author> <br /> <year>2003</year> <price>39.95</price> </book>
          </p>
          
          <p>
            </bookstore>
          </p>
        </blockquote>
        
        <p>
          <a href="http://www.netvtm.com/w3s/xpath/books.xml">View the &#8220;books.xml&#8221; file in your browser</a>.
        </p>
        
        <hr />
        
        <h2 id="selectingnodes">
          <span id="Selecting_Nodes">Selecting Nodes</span>
        </h2>
        
        <p>
          选择节点
        </p>
        
        <p>
          We will use the Microsoft XMLDOM object to load the XML document and the selectNodes() function to select nodes from the XML document: <br /> 我们使用了XMLDOM对象来加载XML文档并用selectNode()函数来进行XML文档上节点的选择：
        </p>
        
        <blockquote>
          <p>
            set xmlDoc=CreateObject(&#8220;Microsoft.XMLDOM&#8221;) <br /> xmlDoc.async=&#8221;false&#8221; <br /> xmlDoc.load(&#8220;books.xml&#8221;)
          </p>
          
          <p>
            xmlDoc.selectNodes(<em>path expression</em>)
          </p>
        </blockquote>
        
        <hr />
        
        <h2 id="selectallbooknodes">
          <span id="Select_all_book_Nodes">Select all book Nodes</span>
        </h2>
        
        <p>
          选择所有book节点
        </p>
        
        <p>
          The following example selects all the book nodes under the bookstore element: <br /> 下面这个实例就会选择所有bookstore元素以下的book节点：
        </p>
        
        <blockquote>
          <p>
            xmlDoc.selectNodes(&#8220;/bookstore/book&#8221;)
          </p>
        </blockquote>
        
        <p>
          <a href="http://www.netvtm.com/w3s/xpath/tryit.asp?filename=try_xpath_select_cdnodes">如果你有IE5以上的版本你可以自己来做一下</a>.
        </p>
        
        <hr />
        
        <h2 id="selectthefirstbooknode">
          <span id="Select_the_First_book_Node">Select the First book Node</span>
        </h2>
        
        <p>
          选择第一个book节点
        </p>
        
        <p>
          The following example selects only the first book node under the bookstore element:
        </p>
        
        <blockquote>
          <p>
            xmlDoc.selectNodes(&#8220;/bookstore/book[0]&#8221;)
          </p>
        </blockquote>
        
        <p>
          <a href="http://www.netvtm.com/w3s/xpath/tryit.asp?filename=try_xpath_select_cdnodes_first">If you have IE 5 or higher you can try it yourself</a>.
        </p>
        
        <p>
          <strong>Note:</strong> IE 5 and 6 has implemented that [0] should be the first node, but according to the W3C standard it should have been [1]!!
        </p>
        
        <hr />
        
        <h2 id="selecttheprices">
          <span id="Select_the_prices">Select the prices</span>
        </h2>
        
        <p>
          选择prices
        </p>
        
        <p>
          The following example selects the text from all the price nodes:
        </p>
        
        <blockquote>
          <p>
            xmlDoc.selectNodes(&#8220;/bookstore/book/price/text()&#8221;)
          </p>
        </blockquote>
        
        <p>
          <a href="http://www.netvtm.com/w3s/xpath/tryit.asp?filename=try_xpath_select_pricenodes_text">If you have IE 5 or higher you can try it yourself</a>.
        </p>
        
        <hr />
        
        <h2 id="selectingpricenodeswithprice35">
          <span id="Selecting_price_Nodes_with_Price35">Selecting price Nodes with Price>35</span>
        </h2>
        
        <p>
          选择price大于35的price节点
        </p>
        
        <p>
          The following example selects all the price nodes with a price higher than 35:
        </p>
        
        <blockquote>
          <p>
            xmlDoc.selectNodes(&#8220;/bookstore/book <br /> [price>35]/price&#8221;)
          </p>
        </blockquote>
        
        <p>
          <a href="http://www.netvtm.com/w3s/xpath/tryit.asp?filename=try_xpath_select_pricenodes_35">If you have IE 5 or higher you can try it yourself</a>.
        </p>
        
        <hr />
        
        <h2 id="selectingtitlenodeswithprice35">
          <span id="Selecting_title_Nodes_with_Price35">Selecting title Nodes with Price>35</span>
        </h2>
        
        <p>
          选择Price大于35的title节点
        </p>
        
        <p>
          The following example selects all the title nodes with a price higher than 35:
        </p>
        
        <blockquote>
          <p>
            xmlDoc.selectNodes(&#8220;/bookstore/book[price>35]/title&#8221;)
          </p>
        </blockquote>
        
        <p>
          <a href="http://www.netvtm.com/w3s/xpath/tryit.asp?filename=try_xpath_select_pricenodes_high">If you have IE 5 or higher you can try it yourself</a>.
        </p>
      </div>