---
title: Docker について
description: コンテナー化された .NET アプリケーションの .NET マイクロサービス アーキテクチャ | Docker について
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 05/26/2017
ms.openlocfilehash: fadd2611283f0a7dadbf1734fe48f7d1a13096ef
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
---
# <a name="what-is-docker"></a>Docker について

[Docker](https://www.docker.com/) は、クラウドまたはオンプレミスで実行できる移植可能な自己完結型のコンテナーとしてアプリの展開を自動化する[オープン ソース プロジェクト](https://github.com/docker/docker)です。 Docker は、このテクノロジを促進および進化させる[会社](https://www.docker.com/)でもあります。 Docker は、クラウド、Linux、Microsoft を含む Windows のベンダーと協力しています。

![](./media/image2.png)

**図 2-2** Docker は、ハイブリッド クラウドのすべてのレイヤーでコンテナーを展開します。

Docker イメージ コンテナーは、Linux と Windows 上でネイティブに実行されます。 Windows イメージは、Windows ホストでのみ実行され、Linux イメージは Linux ホストでのみ実行されます。 ホストは、サーバーまたは VM です。

Windows、Linux、または macOS 上で開発することができます。 開発用コンピューターでは、アプリとその依存関係を含む、Docker イメージが展開されている Docker ホストを実行します。 Linux または macOS では、Linux ベースの Docker ホストを使用し、Linux コンテナー専用のイメージを作成できます (Macos では、コードを編集するか Docker CLI を実行できますが、この記事の執筆時点で、コンテナーは macOS 上で直接実行されません)。Windows では、Linux または Windows コンテナーのいずれかのイメージを作成できます。

Windows または macOS 上で、[Docker Community Edition (CE)](https://www.docker.com/community-edition) が、開発環境でコンテナーをホストし、その他の開発者ツールを提供します。 [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition) は、大規模な基幹業務アプリケーションをビルド、出荷、および実行する IT チームによって使用されます。 どちらの製品も、コンテナーをホストするために必要な VM (Docker ホスト) をインストールします。 

[Windows コンテナー](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/about/about_overview)は、2 種類のランタイムと連携します。

-   Windows Server コンテナーは、プロセスと名前空間の分離テクノロジを使用してアプリケーションの分離を提供します。 Windows Server コンテナーは、コンテナー ホストおよびホストで実行されているすべてのコンテナーとカーネルを共有します。

-   HYPER-V コンテナーは、各コンテナーを高度に最適化された仮想マシンで実行することで、Windows Server コンテナーによって提供される分離を拡張します。 この構成では、コンテナー ホストのカーネルは、Hyper-V コンテナーと共有されず、分離性を提供します。 HYPER-V コンテナーは、信頼されていない*悪意のあるマルチ テナント* アプリケーションの同じホストでの実行を許可します。 HYPER-V コンテナーは、Windows Server コンテナーよりもスタートアップ時間の効率と密度が低下します。

これらのコンテナーのイメージは、同じように作成され、機能します。 コンテナーの作成方法が異なります。 詳細については、「[Hyper-V コンテナー](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/about/about_overview)」を参照してください。

## <a name="comparing-docker-containers-with-virtual-machines"></a>Docker コンテナーと仮想マシンの比較

図 2-3 は、VM と Docker コンテナーの比較を示しています。

  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **仮想マシン**                                                                                                                                                                  **Docker コンテナー**
                                                                                                                                                                                        
  ![](./media/image3.png)                                                                                                                                ![](./media/image4.png)
                                                                                                                                                                                        
  仮想マシンには、アプリケーション、必要なライブラリまたはバイナリ、および完全なゲスト オペレーティング システムが含まれます。 完全な仮想化では、コンテナー詰めより多くのリソースが必要です。 コンテナーには、アプリケーションとそのすべての依存関係が含まれます。 ただし、コンテナーは、他のコンテナーと OS カーネルを共有します。 コンテナーは、ホスト オペレーティング システムでのユーザー領域で分離されたプロセスとして実行されます。 コンテナーごとに特別な仮想マシン内で実行される HYPER-V コンテナーは例外です。
  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**図 2-3** 従来の仮想マシンと Docker コンテナーの比較

コンテナーは必要なリソースが少ないので (たとえば、完全な OS は必要ありません)、高速に起動し、簡単に展開できます。 低いリソース使用率により、高い密度が可能になります。 同じハードウェア単位でより多くのサービスを実行し、コストを削減できます。

同じカーネルで実行される結果として、VM よりも緩やかな分離が提供されます。

イメージの主な目的は、異なる展開間で同じ環境 (依存関係) にすることです。 自分のコンピューターでデバッグし、同じ環境が保証されている別のコンピューターに展開できます。

コンテナー イメージは、アプリやサービスをパッケージ化し、信頼性が高く、再現可能な方法で展開する方法です。 Docker はテクノロジであるだけでなく、哲学やプロセスとも言うことができます。

Docker 開発者は、「私のコンピューターで機能するのだから、実稼働環境でも機能する」とは言いません。 彼らは、「Docker で実行される」と言います。 Docker にパッケージ化されたアプリは、サポートされている任意の Docker 環境で実行できます。 Docker にパッケージ化されたアプリは、すべて展開ターゲット (開発、QA、ステージング、実稼働) で一貫して実行されます。

>[!div class="step-by-step"]
[Previous] (index.md) [Next] (docker-terminology.md)
