---
title: Windows Communication Foundation サンプルの実行
ms.date: 03/30/2017
ms.assetid: db8a83da-95c1-4a21-a9d2-48caeb6398ea
ms.openlocfilehash: 68b05b590e80a65ba8816c0dcfd8d140b71eb8c8
ms.sourcegitcommit: 15109844229ade1c6449f48f3834db1b26907824
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="running-the-windows-communication-foundation-samples"></a>Windows Communication Foundation サンプルの実行
単一コンピューターまたはコンピューター間の構成では、Windows Communication Foundation (WCF) サンプルを実行できます。 サンプルは、単一コンピューターでそのまま実行できます。 複数コンピューター構成で実行するには、サンプルの構成ファイルの設定を変更する必要があります。 単一コンピューターおよび複数コンピューターの構成でサンプルを実行する手順を、次に示します。 インターネット インフォメーション サービス (IIS) でホストされるサービスと自己ホスト型のサービスのサンプルとでは、手順が異なります。 ほとんどのサンプルは IIS でホストされます。サンプルのホスト方法を判断するには、サンプルの Readme 情報を参照してください。  
  
 [!INCLUDE[wv](../../../../includes/wv-md.md)] では、IIS でホストされていないサンプルには、リスナーを Http.sys に登録するための管理特権が必要です。 Httpcfg.exe を使用して、サービスのリッスン アドレスをサービスが実行されているアカウントに登録するか、管理者権限で実行されているコマンド プロンプトでサービスを起動します。  
  
> [!NOTE]
>  ビルドまたは WCF サンプルを実行して、前に実行することを確認する、 [Windows Communication Foundation サンプルの 1 回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)です。  
  
### <a name="to-run-the-sample-on-the-same-machine"></a>サンプルを同じコンピューターで実行するには  
  
1.  サービスが IIS によってホストされている場合は、次のアドレスを入力して、ブラウザーを使用して、サービスにアクセスできることを確認してください:http://localhost/servicemodelsamples/service.svcです。 これに応答して、確認ページが表示されます。 [確認] ページが表示されない場合は、次を参照してください。[トラブルシューティングのヒント](http://msdn.microsoft.com/library/8787c877-5e96-42da-8214-fa737a38f10b)です。  
  
2.  サービスが自己ホスト型の場合は、言語固有のフォルダーの下の \service\bin にある Service.exe を実行します。 サービス アクティビティがサービス コンソール ウィンドウに表示されます。  
  
3.  Client.exe を \client\bin 実行\\、言語固有のフォルダーの下。 クライアント アクティビティがクライアント コンソール ウィンドウに表示されます。  
  
4.  クライアントとサービスできない場合は通信するためを参照してください。[トラブルシューティングのヒント](http://msdn.microsoft.com/library/8787c877-5e96-42da-8214-fa737a38f10b)です。  
  
### <a name="to-run-the-sample-across-machines"></a>サンプルを複数コンピューターで実行するには  
  
1.  サービスが IIS でホストされている場合 :  
  
    1.  サービス コンピューターで、ServiceModelSamples という仮想ディレクトリを作成します。 バッチ ファイルに含まれている Setupvroot.bat [Windows Communication Foundation サンプルの 1 回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)ディスク ディレクトリと仮想ディレクトリの作成に使用することができます。  
  
    2.  サービス プログラム ファイルを %SystemDrive%\Inetpub\wwwroot\servicemodelsamples からサービス コンピューターの ServiceModelSamples 仮想ディレクトリにコピーします。 このファイルが \bin ディレクトリにあることを確認します。  
  
    3.  ブラウザーを使用して、サービスにクライアント コンピューターからアクセスできるかどうかをテストします。  
  
     サービスが自己ホスト型の場合、次の手順を実行します。  
  
    1.  サービス コンピューターで、サービス ファイルを保持するディレクトリを作成します。  
  
    2.  サービス プログラム ファイルを、言語固有のフォルダーにある \service\bin\ フォルダーからサービス コンピューターにコピーします。  
  
    3.  サービスの構成ファイルで、エンドポイント定義のアドレス値をサービスの新しいアドレスに変更します。 アドレスの "localhost" への参照をすべて完全修飾ドメイン名に置き換えます。  
  
    4.  コマンド プロンプトから Service.exe を起動します。  
  
2.  クライアント プログラム ファイルを、言語固有のフォルダーにある \client\bin\ フォルダーからクライアント コンピューターにコピーします。  
  
3.  エンドポイント アドレスを設定します。  
  
    1.  サービスの実行に使用されているのがドメイン アカウントではない場合、クライアント構成ファイルを開き、エンドポイント定義のアドレス値をサービスの新しいアドレスに変更します。 アドレスの "localhost" への参照をすべて完全修飾ドメイン名に置き換えます。  
  
    2.  サービスの実行に使用されているのがドメイン アカウントの場合、サービスに対して Svcutil.exe を実行し、クライアントの構成を再生成します。 詳細については、Svcutil.exe を実行して、次を参照してください。 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)です。 サンプルの構成ファイルではなく、生成されたファイルを使用します。 生成された構成ファイルには、追加の ID 情報があります (また、サービス エンドポイントへの接続に必要なすべての設定が、既定の設定であるにもかかわらず含まれています)。 Id 情報の詳細については、次を参照してください。[サービス Id と認証](../../../../docs/framework/wcf/feature-details/service-identity-and-authentication.md)、および[ \<identity >](../../../../docs/framework/configure-apps/file-schema/wcf/identity.md)です。  
  
4.  クライアント コンピューターで、コマンド プロンプトから Client.exe を起動します。  
  
### <a name="to-debug-a-service"></a>サービスをデバッグするには  
  
1.  ソリューション (クライアントとサービスの両方) を使用して、ビルド、**ビルド**メニューまたは CTRL + SHIFT + B します。  
  
2.  サービスが IIS でホストされている場合 :  
  
    1.  アドレスを入力して、ブラウザーを使用して、サービスをアクティブ化http://localhost/servicemodelsamples/service.svcです。  
  
    2.  ソリューションでは、選択、**デバッグ**メニューおよび**プロセスにアタッチする**メニュー項目。  
  
    3.  選択、**プロセスのすべてのユーザーを表示する**チェック ボックスをオンします。  
  
    4.  デバッグ対象のホスト ワーカー プロセス W3wp.exe を選択します (Windows XP では ASPNet_wp.exe を選択します)。  
  
3.  これで、サービス内にブレークポイントを設定し、例外に対してブレークポイントを有効にできます。  
  
4.  クライアント プロジェクトの項目を右クリックして選択**デバッグ**、**新しいインスタンスを開始**です。  
  
### <a name="to-clean-up-after-the-sample"></a>サンプルの実行後にクリーンアップするには  
  
-   サービスがセキュリティの目的で IIS でホストされている場合、サンプルの使用が終わったら、このセットアップで付与された仮想ディレクトリの定義とアクセス許可を削除してください。  
  
## <a name="see-also"></a>関連項目  
 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)  
 [ワークグループに属しており、コンピューター間で、サンプルの実行](http://msdn.microsoft.com/library/a451a525-e7ce-452d-9da9-620221260113)  
 [トラブルシューティングのヒント](http://msdn.microsoft.com/library/8787c877-5e96-42da-8214-fa737a38f10b)
