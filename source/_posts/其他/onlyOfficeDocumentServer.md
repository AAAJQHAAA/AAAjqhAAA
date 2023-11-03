---
title: OnlyOffice Document Server教程
date: 2023-11-03 15:58:33
category: 其他
tags:
  - OnlyOffice
  - OnlyOffice Document Server
---
# OnlyOffice Document Server教程
- [官网教程地址](https://api.onlyoffice.com/)
## 开始
### 基本概念
- OnlyOffice是一个开源办公套件，包括文本文档、电子表格、演示文稿和可填写表格的编辑器。它提供了以下功能：
  - 创建、编辑和查看文本文档、电子表格、演示文稿和可填写表格；
  - 多人实时协作处理文件。
- OnlyOffice还支持WOPI协议，该协议用于将您的应用程序与在线office集成。
- OnlyOffice API用于让开发人员将`OnlyOffice文档/电子表格/演示文稿编辑器`集成到自己的网站中，并设置和管理编辑器。
- API JavaScript文件通常可以在以下编辑器文件夹中找到：
  - `https://documentserver/web-apps/apps/api/documents/api.js`
  - 其中`documentserver`是Document server的服务器的地址。
- 要嵌入编辑器的目标HTML文件需要有一个占位符div标记，其中将传递有关编辑器参数的所有信息：
```html
<div id="placeholder"></div>
<script type="text/javascript" src="https://documentserver/web-apps/apps/api/documents/api.js"></script>
```
- 包含可变参数的页面代码如下所示：
```js
var docEditor = new DocsAPI.DocEditor("placeholder", config);
```
- 其中config是一个对象：
```js
config = {
    "document": {
        "fileType": "docx",
        "key": "Khirz6zTPdfd7",
        "title": "Example Document Title.docx",
        "url": "https://example.com/url-to-example-document.docx"
    },
    "documentType": "word",
    "editorConfig": {
        "callbackUrl": "https://example.com/url-to-callback.ashx"
    }
};
```
  - 其中`example.com`是文档管理和存储服务器地址。请参阅[如何工作](#开始)部分，以了解有关DocumentServer服务客户端-服务器交互的更多信息。
- 一切ok则`docEditor`对象可以用来调用文档编辑器方法。 
- 上面的示例包括正确启动所需的所有参数。
- 还可以更改其他非强制性参数，以实现其他功能（更改文档的访问权限、显示文档的不同信息等）。请参阅[高级参数](#开始)部分，了解这些参数是什么以及如何更改它们。
- 为了防止替换重要参数，请以令牌的形式向请求添加加密签名。
### 获取OnlyOffice文档
- 可用于Windows、Linux和Docker。
- 要在本地服务器上安装，请按照OnlyOffice帮助中心中的说明进行操作：
  - [Windows安装](https://helpcenter.onlyoffice.com/installation/docs-developer-install-windows.aspx)
  - [Linux安装](https://helpcenter.onlyoffice.com/installation/docs-developer-install-ubuntu.aspx)
  - [Docker安装](https://helpcenter.onlyoffice.com/installation/docs-developer-install-docker.aspx)
- 在使用API文档之前，如有必要，建议进行以下设置：
  - 配置文件中配置服务器设置；
  - 切换到HTTPS协议，以便使用SSL证书进行安全连接；
  - 添加额外的字体，以增强编辑器的工作；
  - 为应用程序界面添加您自己的颜色主题。
### 立即尝试
- 您可以打开各种文件类型进行编辑、查看、共同编辑、审查或查看重新命名的工作原理。
- 点击`</>`按钮查看相应的[示例源代码](https://api.onlyoffice.com/editors/try)。
### 特定于语言的例子
- 任何协作解决方案都可以通过文档编辑和共同编辑得到增强。在本节中，您将学习如何将ONLYOFFICE文档集成到您自己的网站中。
- 每个实例包括用于文本文档、电子表格、演示文稿和可填写表单的在线编辑器，以及一个测试示例。
- 测试示例是一个简单的文档管理系统，可以内置到您的应用程序中进行测试。默认情况下包含的测试示例是在Node.js上编写的。默认为关闭状态。要启用它，请按照开始屏幕上的说明操作。
- 集成示例用于演示文档编辑器功能以及将document Server连接到您自己的应用程序的方法。如果没有适当的代码修改，不要在你自己的服务器上使用这些例子!如果您启用了任何测试示例，请在投入生产之前禁用它。
- 重要的安全信息，在使用测试示例时，请牢记以下安全方面:
  - 由于不需要授权，因此没有保护存储免受未经授权的访问。
  - 链接中没有对参数替换的检查，因为参数是由代码根据预先安排的脚本生成的。
  - 在编辑后保存文件的请求中没有数据检查，因为每个测试示例仅用于来自ONLYOFFICE Document Server的请求。
  - 没有禁止使用来自其他站点的测试示例，因为它们打算与来自另一个域的ONLYOFFICE文档服务器进行交互。
- 要开始将文档编辑器集成到您自己的网站中，您需要做以下事情:
  - 1、下载文档服务器安装并在本地服务器上进行设置:
  - 2、选择编程语言并下载在线编辑器集成到您的网站的示例代码:
  - 3、编辑示例中的配置文件，更改步骤1中安装的编辑器的默认路径和编辑器配置可用的其他参数。
  - 4、如果示例和Document Server安装在不同的计算机上，请确保安装了示例的服务器可以使用您在配置文件中指定的地址(而不是documentserver)访问Document Server。您还必须确保Document Server能够访问安装了示例的服务器，并使用您在配置文件中指定的地址，而不是example.com。
结果应该看起来像下面的演示预览。






