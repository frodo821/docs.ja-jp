---
title: "XslTransform クラスによる XSLT プロセッサの実装"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-standard
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- csharp
- vb
ms.assetid: 88373fe2-4a6b-44f9-8a62-8a3e348e3a46
caps.latest.revision: "6"
author: mairaw
ms.author: mairaw
manager: wpickett
ms.openlocfilehash: 167cd81ecbc25ca243e3b4a7a6aa7327679528e0
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2017
---
# <a name="xsltransform-class-implements-the-xslt-processor"></a><span data-ttu-id="aa2f9-102">XslTransform クラスによる XSLT プロセッサの実装</span><span class="sxs-lookup"><span data-stu-id="aa2f9-102">XslTransform Class Implements the XSLT Processor</span></span>
> [!NOTE]
>  <span data-ttu-id="aa2f9-103"><xref:System.Xml.Xsl.XslTransform> では、[!INCLUDE[dnprdnext](../../../../includes/dnprdnext-md.md)] クラスが廃止されています。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-103">The <xref:System.Xml.Xsl.XslTransform> class is obsolete in the [!INCLUDE[dnprdnext](../../../../includes/dnprdnext-md.md)].</span></span> <span data-ttu-id="aa2f9-104"><xref:System.Xml.Xsl.XslCompiledTransform> クラスを使用して XSLT (Extensible Stylesheet Language for Transformations) 変換を実行できます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-104">You can perform Extensible Stylesheet Language for Transformations (XSLT) transformations using the <xref:System.Xml.Xsl.XslCompiledTransform> class.</span></span> <span data-ttu-id="aa2f9-105">参照してください[XslCompiledTransform クラスを使用して](../../../../docs/standard/data/xml/using-the-xslcompiledtransform-class.md)と[XslTransform クラスからの移行](../../../../docs/standard/data/xml/migrating-from-the-xsltransform-class.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-105">See [Using the XslCompiledTransform Class](../../../../docs/standard/data/xml/using-the-xslcompiledtransform-class.md) and [Migrating From the XslTransform Class](../../../../docs/standard/data/xml/migrating-from-the-xsltransform-class.md) for more information.</span></span>  
  
 <span data-ttu-id="aa2f9-106"><xref:System.Xml.Xsl.XslTransform> クラスは、『XSL Transformations (XSLT) Version 1.0』勧告を実装する XSLT プロセッサです。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-106">The <xref:System.Xml.Xsl.XslTransform> class is an XSLT processor implementing the XSL Transformations (XSLT) Version 1.0 recommendation.</span></span> <span data-ttu-id="aa2f9-107"><xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドはスタイル シートを検索して読み込み、<xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドは渡されたソース ドキュメントを変換します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-107">The <xref:System.Xml.Xsl.XslTransform.Load%2A> method locates and reads style sheets, and the <xref:System.Xml.Xsl.XslTransform.Transform%2A> method transforms the given source document.</span></span> <span data-ttu-id="aa2f9-108"><xref:System.Xml.XPath.IXPathNavigable> インターフェイスを実装している任意のストアを <xref:System.Xml.Xsl.XslTransform> のソース ドキュメントとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-108">Any store that implements the <xref:System.Xml.XPath.IXPathNavigable> interface can be used as the source document for the <xref:System.Xml.Xsl.XslTransform>.</span></span> <span data-ttu-id="aa2f9-109">[!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] では、現在、<xref:System.Xml.XPath.IXPathNavigable> インターフェイスを <xref:System.Xml.XmlDocument>、<xref:System.Xml.XmlDataDocument>、および <xref:System.Xml.XPath.XPathDocument> に実装しているので、これらすべてを変換用の入力ソース ドキュメントとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-109">The [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] currently implements the <xref:System.Xml.XPath.IXPathNavigable> interface on the <xref:System.Xml.XmlDocument>, the <xref:System.Xml.XmlDataDocument>, and the <xref:System.Xml.XPath.XPathDocument>, so all of these can be used as the input source document to a transformation.</span></span>  
  
 <span data-ttu-id="aa2f9-110"><xref:System.Xml.Xsl.XslTransform> の [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] オブジェクトは、次の名前空間で定義されている XSLT 1.0 仕様のみをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-110">The <xref:System.Xml.Xsl.XslTransform> object in the [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] only supports the XSLT 1.0 specification, defined with the following namespace:</span></span>  
  
```xml  
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" version="1.0">    
```  
  
 <span data-ttu-id="aa2f9-111"><xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドを使用して、次のいずれかのクラスからスタイル シートを読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-111">The style sheet can be loaded, using the <xref:System.Xml.Xsl.XslTransform.Load%2A> method, from one of the following classes:</span></span>  
  
-   <span data-ttu-id="aa2f9-112">XPathNavigator</span><span class="sxs-lookup"><span data-stu-id="aa2f9-112">XPathNavigator</span></span>  
  
-   <span data-ttu-id="aa2f9-113">XmlReader</span><span class="sxs-lookup"><span data-stu-id="aa2f9-113">XmlReader</span></span>  
  
-   <span data-ttu-id="aa2f9-114">URL を表す文字列</span><span class="sxs-lookup"><span data-stu-id="aa2f9-114">A string representing a URL</span></span>  
  
 <span data-ttu-id="aa2f9-115">これらの入力クラスには、それぞれに対応する別々の <xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-115">There is a different <xref:System.Xml.Xsl.XslTransform.Load%2A> method for each of the above input classes.</span></span> <span data-ttu-id="aa2f9-116">これらのクラスの 1 つと <xref:System.Xml.XmlResolver> クラスの組み合わせを引数として受け取るメソッドもあります。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-116">Some methods take in a combination of one of these classes and the <xref:System.Xml.XmlResolver> class as arguments.</span></span> <span data-ttu-id="aa2f9-117"><xref:System.Xml.XmlResolver> は、スタイル シート内の `<xsl:import>` または `<xsl:include>` によって参照されているリソースを検索します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-117">The <xref:System.Xml.XmlResolver> locates resources referenced by `<xsl:import>` or `<xsl:include>` found in the style sheet.</span></span> <span data-ttu-id="aa2f9-118">次に示すメソッドは、文字列、<xref:System.Xml.XmlReader>、または <xref:System.Xml.XPath.XPathNavigator> を入力として受け取ります。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-118">The following methods take a string, <xref:System.Xml.XmlReader>, or <xref:System.Xml.XPath.XPathNavigator> as input.</span></span>  
  
```vb  
Overloads Public Sub Load(String)  
```  
  
```csharp  
public void Load(string);  
```  
  
```vb  
Overloads Public Sub Load(String, XmlResolver)  
```  
  
```csharp  
public void Load(string, XmlResolver);  
```  
  
```vb  
Overloads Public Sub Load(XmlReader, XmlResolver, Evidence)  
```  
  
```csharp  
public void Load(XmlReader, XmlResolver, Evidence);  
```  
  
```vb  
Overloads Public Sub Load(XPathNavigator, XmlResolver, Evidence)  
```  
  
```csharp  
public void Load(XPathNavigator, XmlResolver, Evidence);  
```  
  
 <span data-ttu-id="aa2f9-119">上に示した <xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドの大半は、<xref:System.Xml.XmlResolver> をパラメーターとして受け取ります。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-119">Most of the <xref:System.Xml.Xsl.XslTransform.Load%2A> methods shown above take an <xref:System.Xml.XmlResolver> as a parameter.</span></span> <span data-ttu-id="aa2f9-120"><xref:System.Xml.XmlResolver> は、スタイル シートおよび xsl:import 要素と xsl:include 要素で参照されているスタイル シートの読み込みに使用します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-120">The <xref:System.Xml.XmlResolver> is used to load the style sheet and any style sheet(s) referenced in xsl:import and xsl:include elements.</span></span>  
  
 <span data-ttu-id="aa2f9-121">大半の <xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドは証拠もパラメーターとして受け取ります。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-121">Most of the <xref:System.Xml.Xsl.XslTransform.Load%2A> methods also take evidence as a parameter.</span></span> <span data-ttu-id="aa2f9-122">証拠パラメーターは、スタイル シートに関連付けられている <xref:System.Security.Policy.Evidence> です。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-122">The evidence parameter is the <xref:System.Security.Policy.Evidence> that is associated with the style sheet.</span></span> <span data-ttu-id="aa2f9-123">スタイル シートのセキュリティ レベルに応じて、スタイル シートに含まれているスクリプト、スタイル シートで使用されている `document()` 関数、<xref:System.Xml.Xsl.XsltArgumentList> で使用されている拡張オブジェクトなど、そのスタイル シートが参照しているリソースのセキュリティ レベルも変わります。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-123">The security level of the style sheet affects the security level of any subsequent resources it references, such as the script it contains, any `document()` functions it uses, and any extension objects used by the <xref:System.Xml.Xsl.XsltArgumentList>.</span></span>  
  
 <span data-ttu-id="aa2f9-124">URL パラメーターが含まれた <xref:System.Xml.Xsl.XslTransform.Load%2A> メソッドを使用してスタイル シートを読み込んだ場合、証拠が指定されていなければ、指定された URL およびそのサイトとゾーンを組み合わせてスタイル シートの証拠が計算されます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-124">If the style sheet is loaded using a <xref:System.Xml.Xsl.XslTransform.Load%2A> method that contains a URL parameter and no evidence is provided, the evidence of the style sheet is calculated by combining the given URL with its site and zone.</span></span>  
  
 <span data-ttu-id="aa2f9-125">URI も証拠も指定されていない場合は、スタイル シートに対して設定されている証拠が完全に信頼されます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-125">If no URI or evidence is provided, then the evidence set for the style sheet is fully trusted.</span></span> <span data-ttu-id="aa2f9-126">信頼されていないソースからスタイル シートを読み込んだり、信頼されていない拡張オブジェクトを <xref:System.Xml.Xsl.XsltArgumentList> に追加したりしないでください。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-126">Do not load style sheets from untrusted sources, or add untrusted extension objects into the <xref:System.Xml.Xsl.XsltArgumentList>.</span></span>  
  
 <span data-ttu-id="aa2f9-127">セキュリティ レベルと証拠とスクリプト影響の詳細については、次を参照してください。 [XSLT スタイル シートのスクリプトを使用して\<msxsl:script >](../../../../docs/standard/data/xml/xslt-stylesheet-scripting-using-msxsl-script.md)です。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-127">For more information about security levels and evidence and how it affects scripting, see [XSLT Stylesheet Scripting Using \<msxsl:script>](../../../../docs/standard/data/xml/xslt-stylesheet-scripting-using-msxsl-script.md).</span></span> <span data-ttu-id="aa2f9-128">セキュリティ レベルと証拠と拡張オブジェクトに与える影響については、次を参照してください。[スタイル シート パラメーターと拡張オブジェクト用の XsltArgumentList](../../../../docs/standard/data/xml/xsltargumentlist-for-style-sheet-parameters-and-extension-objects.md)です。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-128">For information about security levels and evidence and how it affects extension objects, see [XsltArgumentList for Style Sheet Parameters and Extension Objects](../../../../docs/standard/data/xml/xsltargumentlist-for-style-sheet-parameters-and-extension-objects.md).</span></span>  
  
 <span data-ttu-id="aa2f9-129">セキュリティ レベルと証拠とに与える影響については、`document()`関数を参照してください[外部の XSLT スタイル シートを解決してドキュメント](../../../../docs/standard/data/xml/resolving-external-xslt-style-sheets-and-documents.md)です。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-129">For information about security levels and evidence and how it affects the `document()` function, see [Resolving External XSLT Style Sheets and Documents](../../../../docs/standard/data/xml/resolving-external-xslt-style-sheets-and-documents.md).</span></span>  
  
 <span data-ttu-id="aa2f9-130">スタイル シートに対しては、多くの入力パラメーターを指定できます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-130">A style sheet can be supplied with a number of input parameters.</span></span> <span data-ttu-id="aa2f9-131">スタイル シートでは、拡張オブジェクトの関数を呼び出すこともできます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-131">The style sheet can also call functions on extension objects.</span></span> <span data-ttu-id="aa2f9-132">パラメーターおよび拡張オブジェクトのいずれも <xref:System.Xml.Xsl.XsltArgumentList> クラスを使用してスタイル シートに渡されます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-132">Both parameters and extension objects are supplied to the style sheet using the <xref:System.Xml.Xsl.XsltArgumentList> class.</span></span> <span data-ttu-id="aa2f9-133"><xref:System.Xml.Xsl.XsltArgumentList> の詳細については、「<xref:System.Xml.Xsl.XsltArgumentList>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-133">For more information about the <xref:System.Xml.Xsl.XsltArgumentList>, see <xref:System.Xml.Xsl.XsltArgumentList>.</span></span>  
  
## <a name="recommended-secure-use-of-xsltransform-class"></a><span data-ttu-id="aa2f9-134">XslTransform クラスの安全な使用方法</span><span class="sxs-lookup"><span data-stu-id="aa2f9-134">Recommended Secure Use of XslTransform Class</span></span>  
 <span data-ttu-id="aa2f9-135">スタイル シートのセキュリティ特権は、指定されている証拠によって異なります。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-135">The security privileges of the style sheet depend on the evidence provided.</span></span> <span data-ttu-id="aa2f9-136">次の表では、スタイル シートの場所と、指定する証拠の種類を説明します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-136">The following table summarizes the location of the style sheet and gives an explanation of what type of evidence to give.</span></span>  
  
-   <span data-ttu-id="aa2f9-137">XSLT スタイル シートに外部参照がない場合。またはスタイル シートが信頼できるコード ベースにある場合。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-137">The XSLT style sheet has no external references, or the style sheet comes from a code base that you trust.</span></span>  
  
    -   <span data-ttu-id="aa2f9-138">アセンブリの証拠を指定します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-138">Provide the evidence from your assembly:</span></span>  
  
         `Dim xslt = New XslTransform() xslt.Load(stylesheet, resolver, Me.GetType().Assembly.Evidence)`  
  
         `XsltTransform xslt = new XslTransform();  xslt.Load(stylesheet, resolver, this.GetType().Assembly.Evidence);`  
  
-   <span data-ttu-id="aa2f9-139">XSLT スタイル シートが外部ソースにある場合。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-139">The XSLT style sheet comes from an outside source.</span></span> <span data-ttu-id="aa2f9-140">ソースの出所が知られており、検証可能な URI がある。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-140">The origin of the source is known and there is a verifiable URI.</span></span>  
  
    -   <span data-ttu-id="aa2f9-141">URI を使用して証拠を作成します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-141">Create evidence using the URI.</span></span>  
  
         `Dim xslt As New XslTransform() Dim ev As Evidence = XmlSecureResolver.CreateEvidenceForUrl(stylesheetUri) xslt.Load(stylesheet, resolver, evidence)`  
  
         `XslTransform xslt = new XslTransform(); Evidence ev = XmlSecureResolver.CreateEvidenceForUrl(stylesheetUri); xslt.Load(stylesheet, resolver, evidence);`  
  
-   <span data-ttu-id="aa2f9-142">XSLT スタイル シートが外部ソースにある場合。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-142">The XSLT style sheet comes from an outside source.</span></span> <span data-ttu-id="aa2f9-143">ソースの出所は不明。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-143">The origin of the source is not known.</span></span>  
  
    -   <span data-ttu-id="aa2f9-144">証拠を `null` に設定します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-144">Set evidence to `null`.</span></span> <span data-ttu-id="aa2f9-145">スクリプト ブロックは処理されません。XSLT `document()` 関数はサポートされません。特権を持つ拡張オブジェクトは許可されません。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-145">Script blocks are not processed, the XSLT `document()` function is not supported, and privileged extension objects are disallowed.</span></span>  
  
         <span data-ttu-id="aa2f9-146">`resolver` パラメーターを `null` に設定することもできます。そうすれば、`xsl:import` 要素と `xsl:include` 要素は処理されません。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-146">Additionally, you can also set the `resolver` parameter to `null` This ensures that `xsl:import` and `xsl:include` elements are not processed.</span></span>  
  
-   <span data-ttu-id="aa2f9-147">XSLT スタイル シートが外部ソースにある場合。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-147">The XSLT style sheet comes from an outside source.</span></span> <span data-ttu-id="aa2f9-148">ソースの出所が不明であるが、スクリプトのサポートが必要。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-148">The origin of the source is not known, but you require script support.</span></span>  
  
    -   <span data-ttu-id="aa2f9-149">呼び出し元の証拠を要求します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-149">Request evidence from the caller.</span></span>  
  
## <a name="transformation-of-xml-data"></a><span data-ttu-id="aa2f9-150">XML データの変換</span><span class="sxs-lookup"><span data-stu-id="aa2f9-150">Transformation of XML Data</span></span>  
 <span data-ttu-id="aa2f9-151">スタイル シートが読み込まれた後で、いずれかの <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドを呼び出し、入力ソース ドキュメントを渡すと、変換が開始されます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-151">Once a style sheet is loaded, the transformation starts by calling one of the <xref:System.Xml.Xsl.XslTransform.Transform%2A> methods and supplying an input source document.</span></span> <span data-ttu-id="aa2f9-152"><xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドは、さまざまな変換出力を提供できるように、オーバーロードされます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-152">The <xref:System.Xml.Xsl.XslTransform.Transform%2A> method is overloaded to provide different transformation outputs.</span></span> <span data-ttu-id="aa2f9-153">変換の結果として得られる出力形式を次に示します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-153">The transformation can result in the following output formats:</span></span>  
  
-   <xref:System.Xml.XmlReader>  
  
-   <xref:System.Xml.XmlWriter>  
  
-   <xref:System.IO.TextWriter>  
  
-   <xref:System.IO.Stream>  
  
-   <span data-ttu-id="aa2f9-154">ファイルの文字列 URL</span><span class="sxs-lookup"><span data-stu-id="aa2f9-154">String URL of file</span></span>  
  
 <span data-ttu-id="aa2f9-155">最後の文字列 URL 形式は、URL 上にある入力ドキュメントを変換し、そのドキュメントを出力 URL に書き込む場合によく使用されます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-155">This last format, the string URL, provides for a commonly used scenario in transforming an input document located in a URL and writing the document to the output URL.</span></span> <span data-ttu-id="aa2f9-156">この <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドは、ファイルから XML ドキュメントを読み込み、XSLT 変換を実行し、出力をファイルに書き込む場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-156">This <xref:System.Xml.Xsl.XslTransform.Transform%2A> method is a convenience method to load an XML document from a file, perform the XSLT transformation, and write the output to a file.</span></span> <span data-ttu-id="aa2f9-157">これにより、入力ソース ドキュメントを作成および読み込んでからファイル ストリームに書き込む必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-157">This prevents you from having to create and load the input source document, and then write to a file stream.</span></span> <span data-ttu-id="aa2f9-158">文字列 URL を入出力として使う <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドを使用するコード サンプルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-158">The following code sample shows this use of the <xref:System.Xml.Xsl.XslTransform.Transform%2A> method using the string URL as input and output:</span></span>  
  
```vb  
Dim xsltransform As XslTransform = New XslTransform()  
xsltransform.Load("favorite.xsl")  
xsltransform.Transform("MyDocument.Xml", "TransformResult.xml", Nothing)  
```  
  
```csharp  
XslTransform xsltransform = new XslTransform();  
xsltransform.Load("favorite.xsl");  
xsltransform.Transform("MyDocument.xml", "TransformResult.xml", null);  
```  
  
## <a name="transforming-a-section-of-an-xml-document"></a><span data-ttu-id="aa2f9-159">XML ドキュメントのセクションの変換</span><span class="sxs-lookup"><span data-stu-id="aa2f9-159">Transforming a Section of an XML Document</span></span>  
 <span data-ttu-id="aa2f9-160">変換はドキュメント全体に対して行われます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-160">Transformations apply to the document as a whole.</span></span> <span data-ttu-id="aa2f9-161">つまり、ドキュメント ルート ノード以外のノードを指定しても、変換処理では、読み込んだドキュメントのすべてのノードがアクセスされます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-161">In other words, if you pass in a node other than the document root node, this does not prevent the transformation process from accessing all nodes in the loaded document.</span></span> <span data-ttu-id="aa2f9-162">結果ツリー フラグメントを変換するには、結果ツリー フラグメントだけが含まれた <xref:System.Xml.XmlDocument> を作成し、その <xref:System.Xml.XmlDocument> を <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドに渡します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-162">To transform a result tree fragment, you must create an <xref:System.Xml.XmlDocument> containing just the result tree fragment and pass that <xref:System.Xml.XmlDocument> to the <xref:System.Xml.Xsl.XslTransform.Transform%2A> method.</span></span> <span data-ttu-id="aa2f9-163">結果ツリー フラグメントの変換を実行する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-163">The following example performs a transformation on a result tree fragment.</span></span>  
  
```vb  
Dim xslt As New XslTransform()  
xslt.Load("print_root.xsl")  
Dim doc As New XmlDocument()  
doc.Load("library.xml")  
' Create a new document containing just the result tree fragment.  
Dim testNode As XmlNode = doc.DocumentElement.FirstChild  
Dim tmpDoc As New XmlDocument()  
tmpDoc.LoadXml(testNode.OuterXml)  
' Pass the document containing the result tree fragment   
' to the Transform method.  
Console.WriteLine(("Passing " + tmpDoc.OuterXml + " to print_root.xsl"))  
xslt.Transform(tmpDoc, Nothing, Console.Out, Nothing)  
```  
  
```csharp  
XslTransform xslt = new XslTransform();       
xslt.Load("print_root.xsl");  
XmlDocument doc = new XmlDocument();  
doc.Load("library.xml");  
// Create a new document containing just the result tree fragment.  
XmlNode testNode = doc.DocumentElement.FirstChild;   
XmlDocument tmpDoc = new XmlDocument();   
tmpDoc.LoadXml(testNode.OuterXml);  
// Pass the document containing the result tree fragment   
// to the Transform method.  
Console.WriteLine("Passing " + tmpDoc.OuterXml + " to print_root.xsl");  
xslt.Transform(tmpDoc, null, Console.Out, null);  
```  
  
 <span data-ttu-id="aa2f9-164">この例では、library.xml ファイルと print_root.xsl ファイルを入力として使用し、次の出力をコンソールに表示します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-164">The example uses the library.xml and print_root.xsl files as input and outputs the following to the console.</span></span>  
  
```  
Passing <book genre="novel" ISBN="1-861001-57-5"><title>Pride And Prejudice</title></book> to print_root.xsl   
Root node is book.  
```  
  
 <span data-ttu-id="aa2f9-165">library.xml</span><span class="sxs-lookup"><span data-stu-id="aa2f9-165">library.xml</span></span>  
  
```xml  
<library>  
  <book genre='novel' ISBN='1-861001-57-5'>  
     <title>Pride And Prejudice</title>  
  </book>  
  <book genre='novel' ISBN='1-81920-21-2'>  
     <title>Hook</title>  
  </book>  
</library>  
```  
  
 <span data-ttu-id="aa2f9-166">print_root.xsl</span><span class="sxs-lookup"><span data-stu-id="aa2f9-166">print_root.xsl</span></span>  
  
```xml  
<stylesheet version="1.0" xmlns="http://www.w3.org/1999/XSL/Transform" >  
  <output method="text" />   
  <template match="/">  
     Root node is  <value-of select="local-name(//*[position() = 1])" />   
  </template>  
</stylesheet>  
```  
  
## <a name="migration-of-xslt-from-net-framework-version-10-to-net-framework-version-11"></a><span data-ttu-id="aa2f9-167">.NET Framework Version 1.0 から .NET Framework Version 1.1 への XSLT の移行</span><span class="sxs-lookup"><span data-stu-id="aa2f9-167">Migration of XSLT from .NET Framework version 1.0 to .NET Framework version 1.1</span></span>  
 <span data-ttu-id="aa2f9-168">廃止された [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] バージョン 1.0 の [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] メソッドと新しい <xref:System.Xml.Xsl.XslTransform.Load%2A> バージョン 1.1 の  メソッドを次の表に示します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-168">The following table shows the obsolete [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] version 1.0 methods and new [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] version 1.1 methods for the <xref:System.Xml.Xsl.XslTransform.Load%2A> method.</span></span> <span data-ttu-id="aa2f9-169">新しいメソッドでは、証拠を指定することで、スタイル シートのアクセス許可を制限できます。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-169">The new methods enable you to limit the permissions of the style sheet by specifying evidence.</span></span>  
  
|<span data-ttu-id="aa2f9-170">旧式で .NET Framework version 1.0 の Load メソッド</span><span class="sxs-lookup"><span data-stu-id="aa2f9-170">Obsolete .NET Framework version 1.0 Load Methods</span></span>|<span data-ttu-id="aa2f9-171">交換用 .NET Framework version 1.1 の Load メソッド</span><span class="sxs-lookup"><span data-stu-id="aa2f9-171">Replacement .NET Framework version 1.1 Load Methods</span></span>|  
|------------------------------------------------------|---------------------------------------------------------|  
|<span data-ttu-id="aa2f9-172">Load(XPathNavigator input);</span><span class="sxs-lookup"><span data-stu-id="aa2f9-172">Load(XPathNavigator input);</span></span><br /><br /> <span data-ttu-id="aa2f9-173">Load(XPathNavigator input, XmlResolver resolver);</span><span class="sxs-lookup"><span data-stu-id="aa2f9-173">Load(XPathNavigator input, XmlResolver resolver);</span></span>|<span data-ttu-id="aa2f9-174">Load(XPathNavigator stylesheet, XmlResolver resolver, Evidence evidence);</span><span class="sxs-lookup"><span data-stu-id="aa2f9-174">Load(XPathNavigator stylesheet, XmlResolver resolver, Evidence evidence);</span></span>|  
|<span data-ttu-id="aa2f9-175">Load(IXPathNavigable stylesheet);</span><span class="sxs-lookup"><span data-stu-id="aa2f9-175">Load(IXPathNavigable stylesheet);</span></span><br /><br /> <span data-ttu-id="aa2f9-176">Load(IXPathNavigable stylesheet, XmlResolver resolver);</span><span class="sxs-lookup"><span data-stu-id="aa2f9-176">Load(IXPathNavigable stylesheet, XmlResolver resolver);</span></span>|<span data-ttu-id="aa2f9-177">Load(IXPathNavigable stylesheet, XmlResolver resolver, Evidence evidence);</span><span class="sxs-lookup"><span data-stu-id="aa2f9-177">Load(IXPathNavigable stylesheet, XmlResolver resolver, Evidence evidence);</span></span>|  
|<span data-ttu-id="aa2f9-178">Load(XmlReader stylesheet);</span><span class="sxs-lookup"><span data-stu-id="aa2f9-178">Load(XmlReader stylesheet);</span></span><br /><br /> <span data-ttu-id="aa2f9-179">Load(XmlReader stylesheet, XmlResolver resolver);</span><span class="sxs-lookup"><span data-stu-id="aa2f9-179">Load(XmlReader stylesheet, XmlResolver resolver);</span></span>|<span data-ttu-id="aa2f9-180">Load(XmlReader stylesheet, XmlResolver resolver, Evidence evidence);</span><span class="sxs-lookup"><span data-stu-id="aa2f9-180">Load(XmlReader stylesheet, XmlResolver resolver, Evidence evidence);</span></span>|  
  
 <span data-ttu-id="aa2f9-181">廃止された <xref:System.Xml.Xsl.XslTransform.Transform%2A> メソッドと新しい  メソッドを次の表に示します。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-181">The following table shows the obsolete and new methods for the <xref:System.Xml.Xsl.XslTransform.Transform%2A> method.</span></span> <span data-ttu-id="aa2f9-182">新しいメソッドは <xref:System.Xml.XmlResolver> オブジェクトを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-182">The new methods take an <xref:System.Xml.XmlResolver> object.</span></span>  
  
|<span data-ttu-id="aa2f9-183">.NET Framework バージョン 1.0 の Transform メソッドします。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-183">Obsolete .NET Framework version 1.0 Transform Methods</span></span>|<span data-ttu-id="aa2f9-184">.NET Framework version 1.1 での Transform メソッド</span><span class="sxs-lookup"><span data-stu-id="aa2f9-184">Replacement .NET Framework version Transform 1.1 Methods</span></span>|  
|-----------------------------------------------------------|--------------------------------------------------------------|  
|<span data-ttu-id="aa2f9-185">XmlReader Transform(XPathNavigator input, XsltArgumentList args)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-185">XmlReader Transform(XPathNavigator input, XsltArgumentList args)</span></span>|<span data-ttu-id="aa2f9-186">XmlReader Transform(XPathNavigator input, XsltArgumentList args, XmlResolver resolver)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-186">XmlReader Transform(XPathNavigator  input, XsltArgumentList args, XmlResolver resolver)</span></span>|  
|<span data-ttu-id="aa2f9-187">XmlReader Transform(IXPathNavigable input, XsltArgumentList args)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-187">XmlReader Transform(IXPathNavigable input, XsltArgumentList args)</span></span>|<span data-ttu-id="aa2f9-188">XmlReader Transform(IXPathNavigable input, XsltArgumentList args, XmlResolver resolver)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-188">XmlReader Transform(IXPathNavigable input, XsltArgumentList args, XmlResolver resolver)</span></span>|  
|<span data-ttu-id="aa2f9-189">Void Transform(XPathNavigator input, XsltArgumentList args, XmlWriter output)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-189">Void Transform(XPathNavigator input, XsltArgumentList args, XmlWriter output)</span></span>|<span data-ttu-id="aa2f9-190">Void Transform(XPathNavigator input, XsltArgumentList args, XmlWriter output, XmlResolver resolver)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-190">Void Transform(XPathNavigator input, XsltArgumentList args, XmlWriter output, XmlResolver resolver)</span></span>|  
|<span data-ttu-id="aa2f9-191">Void Transform(IXPathNavigable input, XsltArgumentList args, XmlWriter output)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-191">Void Transform(IXPathNavigable input, XsltArgumentList args, XmlWriter output)</span></span>|<span data-ttu-id="aa2f9-192">Void Transform(IXpathNavigable input, XsltArgumentList args, XmlWriter output, XmlResolver resolver)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-192">Void Transform(IXpathNavigable input, XsltArgumentList args, XmlWriter output, XmlResolver resolver)</span></span>|  
|<span data-ttu-id="aa2f9-193">Void Transform(XPathNavigator input, XsltArgumentList args, TextWriter output)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-193">Void Transform(XPathNavigator input, XsltArgumentList args, TextWriter output)</span></span>|<span data-ttu-id="aa2f9-194">Void Transform(XPathNavigator input, XsltArgumentList args, TextWriter output, XmlResolver resolver)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-194">Void Transform(XPathNavigator input, XsltArgumentList args, TextWriter output, XmlResolver resolver)</span></span>|  
|<span data-ttu-id="aa2f9-195">Void Transform(IXPathNavigable input, XsltArgumentList args, TextWriter output)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-195">Void Transform(IXPathNavigable input, XsltArgumentList args, TextWriter output)</span></span>|<span data-ttu-id="aa2f9-196">Void Transform(IXPathNavigable input, XsltArgumentList args, TextWriter output, XmlResolver resolver)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-196">Void Transform(IXPathNavigable input, XsltArgumentList args, TextWriter output, XmlResolver resolver)</span></span>|  
|<span data-ttu-id="aa2f9-197">Void Transform(XPathNavigator input, XsltArgumentList args, Stream output)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-197">Void Transform(XPathNavigator input, XsltArgumentList args, Stream output)</span></span>|<span data-ttu-id="aa2f9-198">Void Transform(XPathNavigator input, XsltArgumentList args, Stream output, XmlResolver resolver)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-198">Void Transform(XPathNavigator input, XsltArgumentList args, Stream output, XmlResolver resolver)</span></span>|  
|<span data-ttu-id="aa2f9-199">Void Transform(IXPathNavigable input, XsltArgumentList args, Stream output)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-199">Void Transform(IXPathNavigable input, XsltArgumentList args, Stream output)</span></span>|<span data-ttu-id="aa2f9-200">Void Transform(IXPathNavigable input, XsltArgumentList args, Stream output, XmlResolver resolver)</span><span class="sxs-lookup"><span data-stu-id="aa2f9-200">Void Transform(IXPathNavigable input, XsltArgumentList args, Stream output, XmlResolver resolver)</span></span>|  
|<span data-ttu-id="aa2f9-201">Void Transform(String input, String output);</span><span class="sxs-lookup"><span data-stu-id="aa2f9-201">Void Transform(String input, String output);</span></span>|<span data-ttu-id="aa2f9-202">Void Transform(String input, String output, XmlResolver resolver);</span><span class="sxs-lookup"><span data-stu-id="aa2f9-202">Void Transform(String input, String output, XmlResolver resolver);</span></span>|  
  
 <span data-ttu-id="aa2f9-203"><xref:System.Xml.Xsl.XslTransform.XmlResolver%2A?displayProperty=nameWithType> バージョン 1.1 では、[!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] プロパティが廃止されています。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-203">The <xref:System.Xml.Xsl.XslTransform.XmlResolver%2A?displayProperty=nameWithType> property is obsolete in [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] version 1.1.</span></span> <span data-ttu-id="aa2f9-204">代わりに、新しい<xref:System.Xml.Xsl.XslTransform.Transform%2A>を受け取るオーバー ロード、<xref:System.Xml.XmlResolver>オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="aa2f9-204">Instead, use the new <xref:System.Xml.Xsl.XslTransform.Transform%2A> overloads which take an <xref:System.Xml.XmlResolver> object.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="aa2f9-205">関連項目</span><span class="sxs-lookup"><span data-stu-id="aa2f9-205">See Also</span></span>  
 <xref:System.Xml.Xsl.XslTransform>  
 [<span data-ttu-id="aa2f9-206">XslTransform クラスによる XSLT 変換</span><span class="sxs-lookup"><span data-stu-id="aa2f9-206">XSLT Transformations with the XslTransform Class</span></span>](../../../../docs/standard/data/xml/xslt-transformations-with-the-xsltransform-class.md)  
 [<span data-ttu-id="aa2f9-207">変換における XPathNavigator</span><span class="sxs-lookup"><span data-stu-id="aa2f9-207">XPathNavigator in Transformations</span></span>](../../../../docs/standard/data/xml/xpathnavigator-in-transformations.md)  
 [<span data-ttu-id="aa2f9-208">変換における XPathNodeIterator</span><span class="sxs-lookup"><span data-stu-id="aa2f9-208">XPathNodeIterator in Transformations</span></span>](../../../../docs/standard/data/xml/xpathnodeiterator-in-transformations.md)  
 [<span data-ttu-id="aa2f9-209">XslTransform への XPathDocument の入力</span><span class="sxs-lookup"><span data-stu-id="aa2f9-209">XPathDocument Input to XslTransform</span></span>](../../../../docs/standard/data/xml/xpathdocument-input-to-xsltransform.md)  
 [<span data-ttu-id="aa2f9-210">XslTransform への XmlDataDocument の入力</span><span class="sxs-lookup"><span data-stu-id="aa2f9-210">XmlDataDocument Input to XslTransform</span></span>](../../../../docs/standard/data/xml/xmldatadocument-input-to-xsltransform.md)  
 [<span data-ttu-id="aa2f9-211">XslTransform への XmlDocument の入力</span><span class="sxs-lookup"><span data-stu-id="aa2f9-211">XmlDocument Input to XslTransform</span></span>](../../../../docs/standard/data/xml/xmldocument-input-to-xsltransform.md)