---
title: Expressions2
ms.date: 03/30/2017
ms.assetid: 43a85905-77b5-4893-bb38-1cb9b293d69d
ms.openlocfilehash: 2f08e114729d9fd00a51fe0c4182862257e8c330
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
---
# <a name="expressions"></a>式
このサンプルでは、基本的な式をワークフローで使用する方法を示します。 このサンプルは、架空の会社の 2 人の従業員の基本給を計算するワークフローで構成されます。 `Employee` および `SalaryStats` という 2 つのクラスが Employee.cs と SalaryStats.cs で定義されています。 これらのクラスをワークフローで使用して、複合型の変数のプロパティに対する単純な数値演算と文字列操作を実行する方法を示します。  
  
 給与計算ワークフローは、2 つの作成スタイルを示すために XAML と C# の両方で定義されています。 XAML バージョンは SalaryCalculation.xaml にあり、ワークフロー デザイナーで表示および編集できます。 C# バージョンは Program.cs にあります。 XAML で使用されている式は Visual Basic 構文に準拠しており、<xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601> 式アクティビティと <xref:Microsoft.VisualBasic.Activities.VisualBasicReference%601> 式アクティビティを使用して実行されます。 詳細については、Visual Basic 式「 [Visual Basic 式](http://go.microsoft.com/fwlink/?LinkId=165912)です。 一方、C# の式はラムダ式で記述されており、<xref:System.Activities.Expressions.LambdaValue%601> 式アクティビティと <xref:System.Activities.Expressions.LambdaReference%601> 式アクティビティが使用されます。 ラムダ式で記述されているため、C# コンパイラで構文の強調表示や静的な検証を行うことができます。 C# でのラムダ式の詳細については、次を参照してください。[ラムダ式 (c# プログラミング ガイド)](http://go.microsoft.com/fwlink/?LinkId=182082)です。 ワークフローの作成時に Visual Basic でコードを記述すると、Visual Basic ラムダ式が使用されます。 Visual Basic のラムダ式の詳細については、次を参照してください。[ラムダ式 (Visual Basic)](http://go.microsoft.com/fwlink/?LinkId=152437)です。 コードを使用してワークフローの作成に関する詳細については、次を参照してください。[オーサリング ワークフロー、アクティビティ、および命令型コードを使用して式](../../../../docs/framework/windows-workflow-foundation/authoring-workflows-activities-and-expressions-using-imperative-code.md)です。  
  
#### <a name="to-run-the-sample"></a>サンプルを実行するには  
  
1.  [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)] で Expressions.sln ソリューションを開きます。  
  
2.  ソリューションをビルドまたは選択するには CTRL + SHIFT + B キーを押して**ソリューションのビルド**から、**ビルド**メニュー。  
  
    > [!NOTE]
    >  Visual Studio デザイナーで SalaryCalculation.xaml を開くには、先にプロジェクトをコンパイルして、`Employee` クラスと `SalaryStats` クラスをデザイナーで使用できるようにする必要があります。  
  
3.  F5 キーを押してビルドが成功した後、または選択**デバッグの開始**から、**デバッグ**メニュー。 または ctrl キーを押しながら f5 キーを押してまたは選択**デバッグなしで開始**から、**デバッグ**] メニューの [デバッグなしで実行します。  
  
> [!IMPORTANT]
>  サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  このディレクトリが存在しない場合に、 [Windows Communication Foundation (WCF) および .NET Framework 4 向けの Windows Workflow Foundation (WF) サンプル](http://go.microsoft.com/fwlink/?LinkId=150780)すべて Windows Communication Foundation (WCF) をダウンロードして[!INCLUDE[wf1](../../../../includes/wf1-md.md)]サンプルです。 このサンプルは、次のディレクトリに格納されます。  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Expressions`