---
title: "ASP.NET アプリケーションでの SqlDependency | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: ff226ce3-f6b5-47a1-8d22-dc78b67e07f5
caps.latest.revision: 3
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 3
---
# ASP.NET アプリケーションでの SqlDependency
ここでは、ASP.NET の <xref:System.Web.Caching.SqlCacheDependency> オブジェクトを使用して、<xref:System.Data.SqlClient.SqlDependency> を間接的に使用する方法の例を示します。  <xref:System.Web.Caching.SqlCacheDependency> オブジェクトでは <xref:System.Data.SqlClient.SqlDependency> を使用して通知をリッスンし、キャッシュを適切に更新します。  
  
> [!NOTE]
>  このサンプル コードを実行するには、「[クエリ通知の有効化](../../../../../docs/framework/data/adonet/sql/enabling-query-notifications.md)」のスクリプトを実行してクエリ通知を有効にしておく必要があります。  
  
## サンプル アプリケーションについて  
 このサンプル アプリケーションでは、1 つの ASP.NET Web ページを使用して、SQL Server データベースである **AdventureWorks** に格納されている製品情報を <xref:System.Web.UI.WebControls.GridView> コントロールに表示します。  ページが読み込まれる際に、<xref:System.Web.UI.WebControls.Label> コントロールに現在の時刻が入力されます。  次に、<xref:System.Web.Caching.SqlCacheDependency> オブジェクトを定義し、最長 3 分間キャッシュ データを格納するように <xref:System.Web.Caching.Cache> オブジェクトのプロパティを設定します。  その後、データベースに接続し、データを取得します。  ページが読み込まれ、アプリケーションが実行されると、ASP.NET ではキャッシュからデータが取得されます。これは、ページ上の時刻が変更されないことで確認できます。  監視しているデータが変化すると、ASP.NET によりキャッシュが無効化され、`GridView` コントロールに最新データが再入力されることで、`Label` コントロールに表示される時刻が更新されます。  
  
## サンプル アプリケーションの作成  
 サンプル アプリケーションを作成して実行するには、次の手順に従います。  
  
1.  新しい ASP.NET Web サイトを作成します。  
  
2.  Default.aspx ページに <xref:System.Web.UI.WebControls.Label> コントロールと <xref:System.Web.UI.WebControls.GridView> コントロールを追加します。  
  
3.  ページのクラス モジュールを開き、次のディレクティブを追加します。  
  
    ```vb  
  
    Option Strict On  
    Option Explicit On  
  
    Imports System.Data.SqlClient  
    ```  
  
    ```csharp  
    using System.Data.SqlClient;  
    using System.Web.Caching;  
    ```  
  
4.  ページの `Page_Load` イベントに次のコードを追加します。  
  
     [!code-csharp[DataWorks SqlDependency.AspNet#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlDependency.AspNet/CS/Default.aspx.cs#1)]
     [!code-vb[DataWorks SqlDependency.AspNet#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlDependency.AspNet/VB/Default.aspx.vb#1)]  
  
5.  `GetConnectionString` および `GetSQL` という 2 つのヘルパー メソッドを追加します。  定義される接続文字列は、統合セキュリティを利用します。  使用しているアカウントが、必要とされるデータベースへのアクセス許可を持っており、サンプル データベースの **AdventureWorks** で通知が有効になっていることを確認する必要があります。  詳細については、「[Special Considerations When Using Query Notifications](http://msdn.microsoft.com/ja-jp/a83c8dc8-4fb9-4ffd-a2a5-c07cf4a203c7)」を参照してください。  
  
     [!code-csharp[DataWorks SqlDependency.AspNet#2](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlDependency.AspNet/CS/Default.aspx.cs#2)]
     [!code-vb[DataWorks SqlDependency.AspNet#2](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlDependency.AspNet/VB/Default.aspx.vb#2)]  
  
### アプリケーションのテスト  
 このアプリケーションは Web フォームに表示されるデータをキャッシングし、操作が行われなければ 3 分ごとにデータを更新します。  データベースに変化があれば、キャッシュは直ちに更新されます。  Visual Studio からアプリケーションを実行すると、ブラウザーにページが読み込まれます。  表示されるキャッシュ更新時刻は、最後にキャッシュが更新された時刻を示します。  3 分間の待機後、ページを更新し、ポストバック イベントが発生します。  ページに表示される時刻が変更されることに注意してください。  3 分経過する前にページを更新した場合は、ページに表示される時刻は変わりません。  
  
 次に、Transact\-SQL の UPDATE コマンドを使用して、データベースのデータを更新し、ページを更新します。  表示される時刻を見ると、データベースの新しいデータを使ってキャッシュが更新されたことがわかります。  キャッシュは更新されますが、ページに表示される時刻はポストバック イベントが発生するまで変更されないことに注意してください。  
  
## 参照  
 [SQL Server のクエリ通知](../../../../../docs/framework/data/adonet/sql/query-notifications-in-sql-server.md)   
 [ADO.NET Managed Providers and DataSet Developer Center \(ADO.NET マネージ プロバイダーと DataSet デベロッパー センター\)](http://go.microsoft.com/fwlink/?LinkId=217917)