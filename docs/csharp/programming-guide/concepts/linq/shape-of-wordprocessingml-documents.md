---
title: WordprocessingML ドキュメントの構造 (C#)
ms.date: 07/20/2015
ms.assetid: 3791b5e0-c502-469b-bb75-a7bf6fdd0a94
ms.openlocfilehash: 0d8a80a8e7d33facd452e3b7fb31793c1ed55c58
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
---
# <a name="shape-of-wordprocessingml-documents-c"></a>WordprocessingML ドキュメントの構造 (C#)
このトピックでは、WordprocessingML ドキュメントの XML 構造について説明します。  
  
## <a name="microsoft-office-formats"></a>Microsoft Office 形式  
 2007 Microsoft Office システムのネイティブ ファイル形式は Office Open XML (一般的な呼称は Open XML) です。 Open XML は Ecma 標準の XML ベースの形式であり、現在は ISO-IEC 標準としての検討が進められている段階です。 Open XML 内のワード プロセッシング ファイルのマークアップ言語は WordprocessingML と呼ばれます。 このチュートリアルの例では、WordprocessingML ソース ファイルを入力として使用します。  
  
 Microsoft Office 2003 を使用しており、Microsoft Office Compatibility Pack for Word, Excel, and PowerPoint 2007 File Formats をインストールしている場合は、Office Open XML 形式でドキュメントを保存できます。  
  
## <a name="the-shape-of-wordprocessingml-documents"></a>WordprocessingML ドキュメントの構造  
 最初に理解する必要があるのは WordprocessingML ドキュメントの構造です。 WordprocessingML ドキュメントには、ドキュメントの段落を含む本文要素 (名前は `w:body`) が 1 つあります。 各段落には、1 つ以上のテキスト ラン (名前は `w:r`) が含まれています。 各テキスト ランには、1 つ以上のテキスト片 (名前は `w:t`) が含まれています。  
  
 非常に単純な WordprocessingML ドキュメントを次に示します。  
  
```xml  
<?xml version="1.0" encoding="utf-8" standalone="yes"?>  
<w:document  
xmlns:ve="http://schemas.openxmlformats.org/markup-compatibility/2006"  
xmlns:o="urn:schemas-microsoft-com:office:office"  
xmlns:r="http://schemas.openxmlformats.org/officeDocument/2006/relationships"  
xmlns:m="http://schemas.openxmlformats.org/officeDocument/2006/math"  
xmlns:v="urn:schemas-microsoft-com:vml"  
xmlns:wp="http://schemas.openxmlformats.org/drawingml/2006/wordprocessingDrawing"  
xmlns:w10="urn:schemas-microsoft-com:office:word"  
xmlns:w="http://schemas.openxmlformats.org/wordprocessingml/2006/main"  
xmlns:wne="http://schemas.microsoft.com/office/word/2006/wordml">  
  <w:body>  
    <w:p w:rsidR="00E22EB6"  
         w:rsidRDefault="00E22EB6">  
      <w:r>  
        <w:t>This is a paragraph.</w:t>  
      </w:r>  
    </w:p>  
    <w:p w:rsidR="00E22EB6"  
         w:rsidRDefault="00E22EB6">  
      <w:r>  
        <w:t>This is another paragraph.</w:t>  
      </w:r>  
    </w:p>  
  </w:body>  
</w:document>  
```  
  
 このドキュメントには、2 つの段落が含まれています。 各段落には 1 つのテキスト ランが含まれており、各テキスト ランには 1 つのテキスト片が含まれています。  
  
 WordprocessingML ドキュメントの内容を XML 形式で表示する最も簡単な方法は、Microsoft Word を使用して WordprocessingML ドキュメントを作成および保存し、次のプログラムを実行してコンソールに XML を出力することです。  
  
 この例では、WindowsBase アセンブリに含まれるクラスを使用します。 また、<xref:System.IO.Packaging?displayProperty=nameWithType> 名前空間内の型を使用します。  
  
```csharp  
const string documentRelationshipType =  
  "http://schemas.openxmlformats.org/officeDocument/2006/relationships/officeDocument";  
const string wordmlNamespace =  
  "http://schemas.openxmlformats.org/wordprocessingml/2006/main";  
XNamespace w = wordmlNamespace;  
  
using (Package wdPackage = Package.Open("SampleDoc.docx", FileMode.Open, FileAccess.Read))  
{  
    PackageRelationship relationship =  
        wdPackage  
        .GetRelationshipsByType(documentRelationshipType)  
        .FirstOrDefault();  
    if (relationship != null)  
    {  
        Uri documentUri =  
            PackUriHelper.ResolvePartUri(  
                new Uri("/", UriKind.Relative),  
                relationship.TargetUri);  
        PackagePart documentPart = wdPackage.GetPart(documentUri);  
  
        //  Get the officeDocument part from the package.  
        //  Load the XML in the part into an XDocument instance.  
        XDocument xdoc =  
            XDocument.Load(XmlReader.Create(documentPart.GetStream()));  
        Console.WriteLine(xdoc.Root);  
    }  
}  
```  
  
## <a name="external-resources"></a>外部リソース  
 [Office (2007) Open XML ファイル形式の概要](https://msdn.microsoft.com/library/ms406049.aspx)  
 [WordprocessingML の概要](https://msdn.microsoft.com/library/aa212812(office.11).aspx)  
 [WordProcessingML ファイルの構造](http://officeopenxml.com/anatomyofOOXML.php)  
 [WordprocessingML の概要](http://ericwhite.com/blog/introduction-to-wordprocessingml-series/)  
 [Office 2003: XML リファレンス スキーマのダウンロード ページ](https://www.microsoft.com/en-us/download/details.aspx?id=101)  
  
## <a name="see-also"></a>参照  
 [チュートリアル: WordprocessingML ドキュメント内のコンテンツの操作 (C#)](../../../../csharp/programming-guide/concepts/linq/tutorial-manipulating-content-in-a-wordprocessingml-document.md)
