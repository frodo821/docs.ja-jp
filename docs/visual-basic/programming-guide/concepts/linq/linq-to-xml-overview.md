---
title: LINQ to XML の概要 (Visual Basic)
ms.date: 07/20/2015
ms.assetid: 502661e0-bc5d-438d-94c2-7efb63bb6fbd
ms.openlocfilehash: 3818177c2ed14b1159b8e63e324894d7e89b72b6
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
---
# <a name="linq-to-xml-overview-visual-basic"></a>LINQ to XML の概要 (Visual Basic)
XML は、多くのコンテキストでデータを書式設定する方法として広く採用されてきました。 たとえば、Web、構成ファイル、Microsoft Office Word ファイル、データベースで XML が使用されています。  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] は、XML によるプログラミングのために再設計された最新の方法です。 ドキュメント オブジェクト モデル (DOM) のメモリ内ドキュメント変更機能を備え、[!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] クエリ式をサポートします。 このクエリ式は、XPath と構文は異なりますが、機能が似ています。  
  
## <a name="linq-to-xml-developers"></a>LINQ to XML の開発者  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] は、さまざまな開発者を対象としています。 何らかの処理を行うだけの平均的な開発者にとっては、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] によって SQL と同じようにクエリを作成できるので、XML の操作がより簡単になります。 プログラマは、短時間の学習で簡潔かつ強力なクエリを、選択したプログラミング言語で記述できるようになります。  
  
 熟練した開発者は、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] を使用することで生産性を大きく高めることができます。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] を使用すると、より少ないコードで、表現性と簡潔性に優れた強力なコードを記述できます。 また、同時に複数のデータ ドメインからクエリ式を使用できます。  
  
## <a name="what-is-linq-to-xml"></a>LINQ to XML とは  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] は、[!INCLUDE[dnprdnshort](~/includes/dnprdnshort-md.md)] プログラミング言語から XML を操作できるようにする、LINQ に対応したメモリ内 XML プログラミング インターフェイスです。  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] は、XML ドキュメントをメモリに読み込むという点で、ドキュメント オブジェクト モデル (DOM) に似ています。 ドキュメントに対するクエリや変更を行うことができ、変更したドキュメントをファイルに保存したり、シリアル化してインターネット経由で送信したりできます。 ただし、 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] DOM と異なります: 軽量である新しいオブジェクト モデルを提供し、処理を容易に利用している言語機能の Visual Basic でします。  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の最も重要な利点は、[!INCLUDE[vbteclinqext](~/includes/vbteclinqext-md.md)] と統合されていることです。 この統合により、メモリ内の XML ドキュメントに対するクエリを記述して、要素および属性のコレクションを取得できます。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] のクエリ機能は、構文は異なりますが、XPath および XQuery と機能面で互換性があります。 統合の[!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)]Visual Basic より強力な型指定とコンパイル時チェック、およびデバッガー サポートの強化を提供します。  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] のもう 1 つの利点は、クエリの結果を <xref:System.Xml.Linq.XElement> および <xref:System.Xml.Linq.XAttribute> オブジェクト コンストラクターに対するパラメーターとして使用できるので、XML ツリーを作成するための強力な方法が利用可能になります。 *関数型構築*と呼ばれるこの方法では、開発者が XML ツリーの構造を簡単に変換できます。  
  
 たとえば、「[サンプル XML ファイル: 一般的な購買発注書 (LINQ to XML)](http://msdn.microsoft.com/library/0606c09f-6e43-4f8d-95c8-e8e2e08d2348)」で説明されているように、典型的な XML の購買発注書を使用することもできます。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] を使用することで、次のクエリを実行して購買発注書のすべての品目要素の部品番号属性を取得できます。  
  
```vb  
Dim partNos = _  
    From item In purchaseOrder...<Item> _  
    Select item.@PartNumber  
```  
  
 もう 1 つの例として、金額が $100 を超える品目を部品番号順に並べた一覧が必要であるとします。 この情報を取得するには、次のクエリを実行します。  
  
```vb  
Dim partNos = _  
From item In purchaseOrder...<Item> _  
Where (item.<Quantity>.Value * _  
       item.<USPrice>.Value) > 100 _  
Order By item.<PartNumber>.Value _  
Select item  
```  
  
 これらの [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] 機能に加え、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では XML プログラミング インターフェイスが機能強化されています。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] を使用すると、次のことを実行できます。  
  
-   ファイルまたはストリームからの XML の読み込み  
  
-   ファイルまたはストリームへの XML のシリアル化  
  
-   関数型構築を使用した XML の新規作成  
  
-   XPath に類似した軸を使用した XML に対するクエリの実行  
  
-   <xref:System.Xml.Linq.XContainer.Add%2A>、<xref:System.Xml.Linq.XNode.Remove%2A>、<xref:System.Xml.Linq.XNode.ReplaceWith%2A>、<xref:System.Xml.Linq.XElement.SetValue%2A> などのメソッドを使用した、メモリ内の XML ツリーの操作  
  
-   XSD を使用した XML ツリーの検証  
  
-   上記の機能を組み合わせて使用した XML ツリーの構造の変換  
  
## <a name="creating-xml-trees"></a>XML ツリーの作成  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] でのプログラミングで最も重要な利点の 1 つは、XML ツリーを簡単に作成できるという点です。 たとえば、小さな XML ツリーを作成するにすることができますコードよう記述します。  
  
```vb  
Dim contacts = _  
<Contacts>  
    <Contact>  
        <Name>Patrick Hines</Name>  
        <Phone Type="Home">206-555-0144</Phone>  
        <Phone Type="Work">425-555-0145</Phone>  
        <Address>  
            <Street1>123 Main St</Street1>  
            <City>Mercer Island</City>  
            <State>WA</State>  
            <Postal>68042</Postal>  
        </Address>  
    </Contact>  
</Contacts>  
```  
  
 Visual Basic コンパイラに XML リテラルに変換[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]メソッドの呼び出しです。  
  
 詳細については、次を参照してください。 [XML ツリーを作成する」(Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/creating-xml-trees.md)です。  
  
## <a name="see-also"></a>関連項目  
 <xref:System.Xml.Linq>  
 [はじめに (LINQ to XML)](../../../../visual-basic/programming-guide/concepts/linq/getting-started-linq-to-xml.md)  
 [Visual Basic における LINQ to XML の概要](../../../../visual-basic/programming-guide/language-features/xml/overview-of-linq-to-xml.md)  
 [XML](../../../../visual-basic/programming-guide/language-features/xml/index.md)
