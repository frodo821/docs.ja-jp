---
title: はじめに Tutorial1
ms.date: 03/30/2017
helpviewer_keywords:
- WCF [WCF], getting started
- Windows Communication Foundation [WCF], getting started
- getting started [WCF]
ms.assetid: df939177-73cb-4440-bd95-092a421516a1
ms.openlocfilehash: 64d9fc09d56c417c2226d030ef6f5ee7204c4eb6
ms.sourcegitcommit: 15109844229ade1c6449f48f3834db1b26907824
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="getting-started-tutorial"></a>チュートリアル入門
このセクションに含まれているトピックはクイック露出プログラミングの経験 Windows Communication Foundation (WCF) にすることを目的としています。 これらは、このトピックに記載されているリストの順番どおりに完了するように設計されています。 このチュートリアルでの作業では、WCF サービスとクライアント アプリケーションの作成に必要な手順の概要を理解できます。 サービスは 1 つ以上のエンドポイントを公開し、それぞれのエンドポイントは 1 つ以上のサービス操作を公開します。 *エンドポイント*サービスのアドレス、サービスがある、クライアントは、サービスと機能を定義するコントラクトと通信する必要がある方法を説明する情報を含んでいるバインディングを指定します。クライアントにサービスによって提供されます。  
  
 このチュートリアルの一連のトピックを終了すると、サービスを実行し、クライアントからそのサービスを呼び出すことができるようになります。 最初の 3 つのトピックでは、サービス コントラクトを定義する方法、サービス コントラクトを実装する方法、およびサービスをホストする方法について説明します。 作成したサービスは、コンソール アプリケーション内で自己ホストされます。 また、サービスは、インターネット インフォメーション サービス (IIS) でホストすることもできます。 これを行う方法の詳細については、次を参照してください。[する方法: IIS で WCF サービスをホスト](../../../docs/framework/wcf/feature-details/how-to-host-a-wcf-service-in-iis.md)です。 サービスはコードで構成されますが、構成ファイル内で構成することもできます。 構成ファイルの使用の詳細については、次を参照してください。[構成ファイルを使用してサービスを構成する](../../../docs/framework/wcf/configuring-services-using-configuration-files.md)です。  
  
 次の 3 つのトピックでは、クライアント プロキシを作成する方法、クライアント アプリケーションを構成する方法、およびサービスが公開するサービス操作をクライアント プロキシを使って呼び出す方法について説明します。 サービスは、クライアント アプリケーションがサービスと通信するために必要な情報を定義したメタデータを公開します。 [!INCLUDE[vs_current_long](../../../includes/vs-current-long-md.md)] は、このメタデータにアクセスするプロセスを自動化し、それを使って、サービスのクライアント アプリケーションを構築および構成します。 使用していない場合[!INCLUDE[vs_current_long](../../../includes/vs-current-long-md.md)]、使用することができます、 [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)を構築し、サービスのクライアント アプリケーションを構成します。  
  
 このセクションのすべてのトピックでは、開発環境として Visual Studio 2011 を使用することを前提としています。 他の開発環境を使用している場合は、Visual Studio 具体的な指示を無視します。  
  
> [!NOTE]
>  実行する場合は[!INCLUDE[wv](../../../includes/wv-md.md)]または以降のバージョンの Windows オペレーティング システムでは、[スタート] メニューに、Visual Studio 2011 を右クリックしを選択して Visual Studio を開始する必要があります**管理者として実行**です。 ショートカットを作成のショートカットを右クリックして、プロパティを選択して、選択、管理者として Visual Studio 2011 を常に起動する、**互換性**タブをクリックし、確認、 **管理者としてこのプログラムを実行**チェック ボックスをオンします。 このショートカットで Visual Studio 2011 を起動すると、常に管理者として実行されます。  
  
 ハード_ディスクにダウンロードできますして実行する場合は、トピックを参照して、サンプル アプリケーションの[Windows Communication Foundation サンプル](http://msdn.microsoft.com/library/8ec9d192-5d81-4f64-bfd3-90c5e5858c91)です。 このトピックを参照してください、具体的には、[作業の開始](../../../docs/framework/wcf/samples/getting-started-sample.md)です。  
  
 サービスとクライアントの作成に関する詳細の詳細については、次を参照してください。[基本的な WCF プログラミング](../../../docs/framework/wcf/basic-wcf-programming.md)です。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [方法: サービス コントラクトを定義する](../../../docs/framework/wcf/how-to-define-a-wcf-service-contract.md)  
 ユーザー定義のインターフェイスを使用して WCF コントラクトを作成する方法について説明します。 コントラクトは、サービスが公開する機能を定義します。  
  
 [方法: サービス コントラクトを実装する](../../../docs/framework/wcf/how-to-implement-a-wcf-contract.md)  
 サービス コントラクトを実装する方法について説明します。 定義したコントラクトはサービスのクラスと共に実装する必要があります。  
  
 [方法: 基本的なサービスをホストおよび実行する](../../../docs/framework/wcf/how-to-host-and-run-a-basic-wcf-service.md)  
 サービスのエンドポイントをコードで構成する方法と、コンソール アプリケーションでサービスをホストする方法について説明します。 サービスをアクティブにするには、サービスをランタイム環境内で構成してホストする必要があります。 この環境によってサービスが作成され、サービスのコンテキストと有効期間が制御されます。  
  
 [方法: クライアントを作成する](../../../docs/framework/wcf/how-to-create-a-wcf-client.md)  
 WCF サービスから WCF クライアント プロキシを作成するために使用するメタデータを取得する方法について説明します。 このプロセスでは、Visual Studio 2011 の "サービス参照の追加" 機能を使用します。  
  
 [方法: クライアントを構成する](../../../docs/framework/wcf/how-to-configure-a-basic-wcf-client.md)  
 WCF クライアントの構成方法について説明します。クライアントを構成するには、クライアントがサービスへのアクセスに使用するエンドポイントを指定する必要があります。  
  
 [方法: クライアントを使用する](../../../docs/framework/wcf/how-to-use-a-wcf-client.md)  
 サービス操作を呼び出す、WCF クライアント プロキシを使用する方法について説明します。  
  
## <a name="reference"></a>参照  
 <xref:System.ServiceModel.ServiceContractAttribute>  
  
 <xref:System.ServiceModel.OperationContractAttribute>  
  
## <a name="related-sections"></a>関連項目  
 [Windows Communication Foundation サンプル](http://msdn.microsoft.com/library/8ec9d192-5d81-4f64-bfd3-90c5e5858c91)  
  
 [基本的なプログラミング ライフサイクル](../../../docs/framework/wcf/basic-programming-lifecycle.md)  
  
## <a name="see-also"></a>関連項目  
 [概念](../../../docs/framework/wcf/conceptual-overview.md)  
 [ドキュメントのガイド](../../../docs/framework/wcf/guide-to-the-documentation.md)  
 [Windows Communication Foundation とは](../../../docs/framework/wcf/whats-wcf.md)  
 [WCF 機能の詳細](../../../docs/framework/wcf/feature-details/index.md)
