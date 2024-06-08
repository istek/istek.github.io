---
id: 340
title: XPath Nodes(节)
date: 2006-04-27T09:30:12+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=340
permalink: /xpath-nodes-section/
categories:
  - 我的世界
tags:
  - 'c#'
---
In XPath, there are seven kinds of nodes: element, attribute, text, namespace, processing-instruction, comment, and document (root) nodes. 
  
在XPath中有七种nodes(节)：元素，属性，文字，命名空间，处理说明，注释，和文档(根)节。

* * *

<div id="toc_container" class="toc_white no_bullets">
  <p class="toc_title">
    文章目录
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#XPath_Terminology">XPath Terminology</a><ul>
        <li>
          <a href="#Nodes">Nodes/节</a>
        </li>
        <li>
          <a href="#Atomic_values">Atomic values</a>
        </li>
        <li>
          <a href="#Items">Items</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Relationship_of_Nodes">Relationship of Nodes</a><ul>
        <li>
          <a href="#Parent">Parent/父</a>
        </li>
        <li>
          <a href="#Children">Children/子</a>
        </li>
        <li>
          <a href="#Siblings">Siblings/兄</a>
        </li>
        <li>
          <a href="#Ancestors">Ancestors/祖</a>
        </li>
        <li>
          <a href="#Descendants">Descendants/孙</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

## <span id="XPath_Terminology">XPath Terminology</span> {#xpathterminology}

XPath术语

### <span id="Nodes">Nodes/节</span> {#nodes}

In XPath, there are seven kinds of nodes: element, attribute, text, namespace, processing-instruction, comment, and document (root) nodes. XML documents are treated as trees of nodes. The root of the tree is called the document node (or root node). 
  
XML文档被视为数状的节。树的根部被称为文档的节(或根节)。

Look at the following XML document: 
  
观察下面的XML文档：

    <?xml version="1.0" encoding="ISO-8859-1"?>  
    <bookstore>  
    <book>  
    <title lang="en">Harry Potter</title>  
    <author>J K. Rowling</author>  
    <year>2005</year>  
    <price>29.99</price>  
    </book>  
    </bookstore>  
    

Example of nodes in the XML document above: 
  
上面举例的XML文档的节有：

    <bookstore> (document node)
    
    <author>J K. Rowling</author> (element node)
    
    lang="en" (attribute node)  
    

### <span id="Atomic_values">Atomic values</span> {#atomicvalues}

原子值

Atomic values are nodes with no children or parent. 
  
原子值是那些没有子或父的节（无上下关系）。

Example of atomic values: 
  
举例中的原子值：

    J K. Rowling  
    "en"
    

### <span id="Items">Items</span> {#items}

项目

Items are atomic values or nodes. 
  
项目是原子值或节。

* * *

## <span id="Relationship_of_Nodes">Relationship of Nodes</span> {#relationshipofnodes}

节之间的关系

### <span id="Parent">Parent/父</span> {#parent}

Each element and attribute has one parent. 
  
每个元素和属性有一父亲。

In the following example; the book element is the parent of the title, author, year, and price: 
  
下面的举例中：book元素是title，author，year和price的父亲

    <book>  
    <title>Harry Potter</title>  
    <author>J K. Rowling</author>  
    <year>2005</year>  
    <price>29.99</price>  
    </book>  
    

### <span id="Children">Children/子</span> {#children}

Element nodes may have zero, one or more children. 
  
元素节可能有0个或多个子

In the following example; the title, author, year, and price elements are all children of the book element: 
  
下面的举例中：title,author,year和price元素都是book元素的子元素

    <book>  
    <title>Harry Potter</title>  
    <author>J K. Rowling</author>  
    <year>2005</year>  
    <price>29.99</price>  
    </book>  
    

### <span id="Siblings">Siblings/兄</span> {#siblings}

Nodes that have the same parent. 
  
指那些有相同父的

In the following example; the title, author, year, and price elements are all siblings: 
  
下面的举例中title, author, year, 和 price元素都为兄弟

    <book>  
    <title>Harry Potter</title>  
    <author>J K. Rowling</author>  
    <year>2005</year>  
    <price>29.99</price>  
    </book>  
    

### <span id="Ancestors">Ancestors/祖</span> {#ancestors}

A node&#8217;s parent, parent&#8217;s parent, etc. 
  
节的父，父的父….都为祖

In the following example; the ancestors of the title element are the book element and the bookstore element: 
  
下面的举例中：book元素和bookstore元素都为title元素的祖元素

    <bookstore>  
    <book>  
    <title>Harry Potter</title>  
    <author>J K. Rowling</author>  
    <year>2005</year>  
    <price>29.99</price>  
    </book>  
    </bookstore>  
    

### <span id="Descendants">Descendants/孙</span> {#descendants}

A node&#8217;s children, children&#8217;s children, etc. 
  
节的子，子的子…都为孙

In the following example; descendants of the bookstore element are the book, title, author, year, and price elements: 
  
下面的举例中：bookstore元素的孙有book,title,author,year以及price元素

    <bookstore>  
    <book>  
    <title>Harry Potter</title>  
    <author>J K. Rowling</author>  
    <year>2005</year>  
    <price>29.99</price>  
    </book>  
    </bookstore>