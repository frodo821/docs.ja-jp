---
title: WCF 開発ツールの使用
ms.date: 03/30/2017
ms.assetid: 054adb87-c244-4d5a-83d1-0b2b44bd454b
ms.openlocfilehash: 41d2ee2881b79ffb086a931ec6d271d409ac66db
ms.sourcegitcommit: 15109844229ade1c6449f48f3834db1b26907824
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="using-the-wcf-development-tools"></a>WCF 開発ツールの使用
このセクションでは、WCFservice の開発に役立つ Visual Studio 開発ツールについて説明します。  
  
 基盤として Visual Studio テンプレートを使用して、独自のサービスをすばやく作成し、デバッグし、サービスをテストする WCF サービスの自動ホストと WCF テスト クライアントを使用できます。 これらのツールによって、高速でシームレスなデバッグとテストのサイクルが実現し、初期の段階においてホスト モデルのコミットが必要なくなります。  
  
## <a name="the-wcf-developer-tools"></a>WCF の開発者用ツール  
 [WCF Visual Studio テンプレート](../../../docs/framework/wcf/wcf-vs-templates.md)  
  
 Visual Studio で、定義済みの Visual Studio プロジェクトと項目テンプレートを使用すると、WCF サービスや周辺アプリケーションを迅速に構築します。  
  
 [WCF サービス ホスト (WcfSvcHost.exe)](../../../docs/framework/wcf/wcf-service-host-wcfsvchost-exe.md)  
  
 WCF サービスの自動ホスト (WcfSvcHost.exe) では、Visual Studio デバッガーを自動的にホストし、実装しているサービスをテストする (F5) を起動することができます。 検出して潜在的なエラーを修正する WCF テスト クライアント (wcfTestClient.exe) または独自のクライアントを使用して、サービスをテストできます。  
  
 [WCF のテスト用クライアント (WcfTestClient.exe)](../../../docs/framework/wcf/wcf-test-client-wcftestclient-exe.md)  
  
 WCF テスト クライアント (WcfTestClient.exe) は、任意の型のパラメーターを入力し、そのサービス、およびサービスの応答を返信ビューに入力することができますを GUI ツールです。 テストできるように WCF サービスの自動ホストと組み合わせると、シームレスにサービスを提供します。  
  
 [XML からのデータ型クラスの生成](../../../docs/framework/wcf/generating-data-type-classes-from-xml.md)  
  
 クリップボードに格納されている XML データは、コード ページに貼り付けることができます。 データで定義されているクラスは、コード型に変換されます。  
  
## <a name="using-the-tools-without-administrator-privilege"></a>管理特権を必要としないツールの使用  
 名前空間の ACL (アクセス制御リスト) を作成する WCF サービスの開発を管理者特権のないユーザーを有効にするには、"http://+:8731/Design_Time_Addresses"Visual Studio のインストール中にします。 この ACL は (UI) に設定され、コンピューターにログオンしているすべての対話ユーザーが含まれます。 管理者は、この ACL にユーザーを追加または削除したり、追加のポートを開いたりできます。この ACL によって、既定の構成で、WCF テンプレートまたは WF テンプレートでデータを送受信できるようになります。 ユーザーが管理者特権を与えずに WCF サービスの自動ホスト (wcfSvcHost.exe) を使用することもできます。  
  
 システム特権のある管理者アカウントで [!INCLUDE[wv](../../../includes/wv-md.md)] の Netsh.exe ツールを使用すると、アクセスを変更できます。 Netsh.exe の使用例を次に示します。  
  
```  
netsh http add urlacl url=http://+:8001/MyService user=<domain>\<user>  
```  
  
 Netsh.exe の詳細については、次を参照してください。 [Netsh.exe ツールとコマンド ライン スイッチを使用する方法](http://go.microsoft.com/fwlink/?LinkId=97877)です。  
  
## <a name="see-also"></a>関連項目  
 [WCF Visual Studio テンプレート](../../../docs/framework/wcf/wcf-vs-templates.md)  
 [WCF サービス ホスト (WcfSvcHost.exe)](../../../docs/framework/wcf/wcf-service-host-wcfsvchost-exe.md)  
 [WCF のテスト用クライアント (WcfTestClient.exe)](../../../docs/framework/wcf/wcf-test-client-wcftestclient-exe.md)
