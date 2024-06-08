---
id: 342
title: XPath语法
date: 2006-04-27T09:32:36+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=342
permalink: /xpath-syntax/
categories:
  - 我的世界
tags:
  - 'c#'
---
XPath uses path expressions to select nodes or node-sets in an XML document. The node is selected by following a path or steps. 
  
XPath使用路径表达式来选择XML文档的节或是节集。顺着路径或步骤来选择节。

<div class="content">
  XPath uses path expressions to select nodes or node-sets in an XML document. The node is selected by following a path or steps.<br /> XPath使用路径表达式来选择XML文档的节或是节集。顺着路径或步骤来选择节。</p> 
  
  <p>
    &#8211; &#8211; &#8211; &#8211; &#8211; &#8211;
  </p>
  
  <p>
    ## The XML Example Document<br /> XML实例文档
  </p>
  
  <p>
    We will use the following XML document in the examples below.<br /> 举例中我们将使用下面的XML文档
  </p>
  
  <p>
    ><?xml version="1.0" encoding="ISO-8859-1"?>
  </p>
  
  <p>
    ><bookstore>
  </p>
  
  <p>
    ><book><br /> 
    
    <title lang="eng">
      Harry Potter
    </title><price>29.99</price> </book>
  </p>
  
  <p>
    ><book><br /> 
    
    <title lang="eng">
      Learning XML
    </title><price>39.95</price> </book>
  </p>
  
  <p>
    ></bookstore>
  </p>
  
  <p>
    &#8211; &#8211; &#8211; &#8211; &#8211; &#8211;
  </p>
  
  <p>
    ## Selecting Nodes<br /> 选择节
  </p>
  
  <p>
    XPath uses path expressions to select nodes in an XML document. The node is selected by following a path or steps. The most useful path expressions are listed below:<br /> 一些非常有用的路径表达式：
  </p>
  
  <table border="1" cellspacing="0" class="ex" id="table1">
    <tr>
      <td valign="top" width="20%">
        **表达式**
      </td>
      
      <td valign="top">
        **描述**
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        *nodename*
      </td>
      
      <td valign="top">
        Selects all child nodes of the node[选择所有目前节的子节]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        /
      </td>
      
      <td valign="top">
        Selects from the root node[从根节进行选择]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        //
      </td>
      
      <td valign="top">
        Selects nodes in the document from the current node that match the selection no matter where they are [选择文档中相吻合的节而不管其在文档的何处]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        .
      </td>
      
      <td valign="top">
        Selects the current node[选择当前节]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        ..
      </td>
      
      <td valign="top">
        Selects the parent of the current node[当前节的父节]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        @
      </td>
      
      <td valign="top">
        Selects attributes[选择属性]
      </td>
    </tr>
  </table>
  
  <p>
    ### Examples<br /> 实例
  </p>
  
  <p>
    In the table below we have listed some path expressions and the result of the expressions:<br /> 下面我们所列举的表格有路径表达式以及其结果：
  </p>
  
  <table border="1" cellspacing="0" class="ex" id="table34">
    <tr>
      <td valign="top" width="30%">
        **路径表达式**
      </td>
      
      <td valign="top">
        **结果**
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        bookstore
      </td>
      
      <td valign="top">
        Selects all the child nodes of the bookstore element[选择所有bookstore元素的子节]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        /bookstore
      </td>
      
      <td valign="top">
        Selects the root element bookstore **Note:** If the path starts with a slash ( / ) it always represents an absolute path to an element!</p> 
        
        <p>
          [选择了bookstore的根元素。注意：如果路径的开始为(/)那此路径一定是到该元素的绝对路径]
        </p>
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        bookstore/book
      </td>
      
      <td valign="top">
        Selects all book elements that are children of bookstore[选择了所有在bookstore的子元素book元素所包含的所有元素（其实就为bookstore里book元素所包含的元素）]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        //book
      </td>
      
      <td valign="top">
        Selects all book elements no matter where they are in the document[选择所有为book元素的内容而不管book元素处于何处(有不同的父也没关系)]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        bookstore//book
      </td>
      
      <td valign="top">
        Selects all book elements that are descendant of the bookstore element, no matter where they are under the bookstore element[在bookstore元素内所有含有book元素的元素内容（只要book元素的祖元素为bookstore元素那都符合条件）]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        //@lang
      </td>
      
      <td valign="top">
        Selects all attributes that are named lang[选择所有属性名为lang的属性]
      </td>
    </tr>
  </table>
  
  <p>
    &#8211; &#8211; &#8211; &#8211; &#8211; &#8211;
  </p>
  
  <p>
    ## Predicates<br /> 谓语
  </p>
  
  <p>
    Predicates are used to find a specific node or a node that contains a specific value.<br /> 谓语用来指定明确的节所含有的特殊的值
  </p>
  
  <p>
    Predicates are always embedded in square brackets.<br /> 谓语被嵌入在中括号
  </p>
  
  <p>
    ### Examples<br /> 举例
  </p>
  
  <p>
    In the table below we have listed some path expressions with predicates and the result of the expressions:<br /> 下面的表格列举了一些使用了谓语的路径表达式以及其产生的结果：
  </p>
  
  <table border="1" cellspacing="0" class="ex" id="table35">
    <tr>
      <td valign="top" width="50%">
        **路径表达式**
      </td>
      
      <td valign="top">
        **结果**
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        /bookstore/book[1]
      </td>
      
      <td valign="top">
        Selects the first book element that is the child of the bookstore element[选择了bookstore里的第一个book元素]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        /bookstore/book[last()]
      </td>
      
      <td valign="top">
        Selects the last book element that is the child of the bookstore element[选择bookstore里最后一个book元素]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        /bookstore/book[last()-1]
      </td>
      
      <td valign="top">
        Selects the last but one book element that is the child of the bookstore element[bookstore中倒数第二个book元素]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        /bookstore/book[position()<3]
      </td>
      
      <td valign="top">
        Selects the first two book elements that are children of the bookstore element[在bookstore中前两个book元素]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        //title[@lang]
      </td>
      
      <td valign="top">
        Selects all the title elements that have an attribute named lang[选择所有含有lang属性的title元素]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        //title[@lang=&#8217;eng&#8217;]
      </td>
      
      <td valign="top">
        Selects all the title elements that have an attribute named lang with a value of &#8216;eng'[选择所有含有lang属性并且值为eng的title元素]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        /bookstore/book[price>35.00]
      </td>
      
      <td valign="top">
        Selects all the book elements of the bookstore element that have a price element with a value greater than 35.00[选择所有bookstore中book元素里price元素内容大于35.00的book元素]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        /bookstore/book[price>35.00]/title
      </td>
      
      <td valign="top">
        Selects all the title elements of the book elements of the bookstore element that have a price element with a value gr<br /> eater than 35.00[选择bookstore中book的子元素title，并且其兄弟元素price的内容得大于35.00]
      </td>
    </tr>
  </table>
  
  <p>
    &#8211; &#8211; &#8211; &#8211; &#8211; &#8211;
  </p>
  
  <p>
    ## Selecting Unknown Nodes<br /> 选择未知的节
  </p>
  
  <p>
    XPath wildcards can be used to select unknown XML elements.<br /> XPath的通配符可以用来选择未知的XML元素
  </p>
  
  <table border="1" cellspacing="0" class="ex" id="table12">
    <tr>
      <td valign="top" width="20%">
        **通配符**
      </td>
      
      <td valign="top">
        **描述**
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        *
      </td>
      
      <td valign="top">
        Matches any element node[相吻合的所有元素节]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        @*
      </td>
      
      <td valign="top">
        Matches any attribute node[相吻合的所有属性节]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        node()
      </td>
      
      <td valign="top">
        Matches any node of any kind[吻合任何类型的节]
      </td>
    </tr>
  </table>
  
  <p>
    ### Examples实例
  </p>
  
  <p>
    In the table below we have listed some path expressions and the result of the expressions:<br /> 下面的表格我们将列举一些路径表达式以及它们的结果
  </p>
  
  <table border="1" cellspacing="0" class="ex" id="table36">
    <tr>
      <td valign="top" width="30%">
        **路径表达式**
      </td>
      
      <td valign="top">
        **结果**
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        /bookstore/*
      </td>
      
      <td valign="top">
        Selects all the child nodes of the bookstore element[选择所有bookstore的子节]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        //*
      </td>
      
      <td valign="top">
        Selects all elements in the document[选择所有文档中的元素]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        //title[@*]
      </td>
      
      <td valign="top">
        Selects all title elements which have any attribute[选择元素为title并且其含有属性]
      </td>
    </tr>
  </table>
  
  <p>
    &#8211; &#8211; &#8211; &#8211; &#8211; &#8211;
  </p>
  
  <p>
    ## Selecting Several Paths<br /> 选择数个路径
  </p>
  
  <p>
    By using the | operator in an XPath expression you can select several paths.<br /> 通过在XPath中使用 | 你可以选择数个路径
  </p>
  
  <p>
    ### Examples<br /> 实例
  </p>
  
  <p>
    In the table below we have listed some path expressions and the result of the expressions:<br /> 下面的表格我们会列举一些路径表达式以及其结果：
  </p>
  
  <table border="1" cellspacing="0" class="ex" id="table37">
    <tr>
      <td valign="top" width="40%">
        **路径表达**
      </td>
      
      <td valign="top">
        **结果**
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        //book/title | //book/price
      </td>
      
      <td valign="top">
        Selects all the title AND price elements of all book elements[选择所有book里title和price元素]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        //title | //price
      </td>
      
      <td valign="top">
        Selects all the title AND price elements in the document[选择所有title和price元素]
      </td>
    </tr>
    
    <tr>
      <td valign="top">
        /bookstore/book/title | //price
      </td>
      
      <td valign="top">
        Selects all the title elements of the book element of the bookstore element AND all the price elements in the document[选择所有book里的title元素和所有price元素]
      </td>
    </tr>
  </table>
</div>