---
title: '方法 : DataView オブジェクトを Windows フォーム DataGridView コントロールにバインドする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 2b73d60a-6049-446a-85a7-3e5a68b183e2
ms.openlocfilehash: 98b3afcded257bc11dc3bca7d9a9310dc1a7cc89
ms.sourcegitcommit: 11f11ca6cefe555972b3a5c99729d1a7523d8f50
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="how-to-bind-a-dataview-object-to-a-windows-forms-datagridview-control"></a>方法 : DataView オブジェクトを Windows フォーム DataGridView コントロールにバインドする
<xref:System.Windows.Forms.DataGridView> コントロールには、データを表形式で表示するための強力で柔軟な機能が用意されています。 <xref:System.Windows.Forms.DataGridView> コントロールは、標準 Windows フォーム データ バインディング モデルをサポートするため、<xref:System.Data.DataView> およびその他の各種のデータ ソースにバインドします。 ただし、ほとんどの状況では、データ ソースとの対話の詳細を管理する <xref:System.Windows.Forms.BindingSource> コンポーネントにバインドします。  
  
 詳細については、<xref:System.Windows.Forms.DataGridView>を制御しを参照してください[DataGridView コントロールの概要](../../../../docs/framework/winforms/controls/datagridview-control-overview-windows-forms.md)です。  
  
### <a name="to-connect-a-datagridview-control-to-a-dataview"></a>DataGridView コントロールを DataView に接続するには  
  
1.  データベースからデータを取得する操作の詳細を処理するメソッドを実装します。 次のコード例では、`GetData` コンポーネントを初期化する <xref:System.Data.SqlClient.SqlDataAdapter> メソッドを実装し、これを使用して <xref:System.Data.DataSet> に値を設定します。 `connectionString` 変数は、使用しているデータベースに合った値に設定してください。 AdventureWorks SQL Server サンプル データベースがインストールされているサーバーにアクセスする必要があります。  
  
     [!code-csharp[DP DataViewWinForms Sample#LDVSample1GetData](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataViewWinForms Sample/CS/Form1.cs#ldvsample1getdata)]
     [!code-vb[DP DataViewWinForms Sample#LDVSample1GetData](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataViewWinForms Sample/VB/Form1.vb#ldvsample1getdata)]  
  
2.  フォームの <xref:System.Windows.Forms.Form.Load> イベント ハンドラーで、<xref:System.Windows.Forms.DataGridView> コントロールを <xref:System.Windows.Forms.BindingSource> コンポーネントにバインドし、`GetData` メソッドを呼び出してデータベースからデータを取得します。 <xref:System.Data.DataView> は、Contact [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)] に対する <xref:System.Data.DataTable> クエリから作成された後、<xref:System.Windows.Forms.BindingSource> コンポーネントにバインドされます。  
  
     [!code-csharp[DP DataViewWinForms Sample#LDVSample1FormLoad](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataViewWinForms Sample/CS/Form1.cs#ldvsample1formload)]
     [!code-vb[DP DataViewWinForms Sample#LDVSample1FormLoad](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataViewWinForms Sample/VB/Form1.vb#ldvsample1formload)]  
  
## <a name="see-also"></a>関連項目  
 [データ バインディングと LINQ to DataSet](../../../../docs/framework/data/adonet/data-binding-and-linq-to-dataset.md)
