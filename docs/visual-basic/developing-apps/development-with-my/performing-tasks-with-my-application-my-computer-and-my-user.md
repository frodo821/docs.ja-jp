---
title: My.Application、My.Computer、および My.User でのタスクの実行 (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- My.Application object [Visual Basic], developing applications
- rapid application development (RAD), My.Application
- rapid application development (RAD), My.Computer
- rapid application development (RAD), My.User
- My.Computer object [Visual Basic], developing applications
- My.User object [Visual Basic], developing applications
ms.assetid: c8af61bd-4dd3-4a0f-9af5-795b594b240b
ms.openlocfilehash: bc2b0521598c00cdb398d936d61d65874a9cf274
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
---
# <a name="performing-tasks-with-myapplication-mycomputer-and-myuser-visual-basic"></a>My.Application、My.Computer、および My.User でのタスクの実行 (Visual Basic)
次の 3 つの中央`My`へのアクセスについてよく使用される機能を提供するオブジェクトは`My.Application`(<xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>)、 `My.Computer` (<xref:Microsoft.VisualBasic.Devices.Computer>)、および`My.User`(<xref:Microsoft.VisualBasic.ApplicationServices.User>)。 これらのオブジェクトを使用して、それぞれ、現在のアプリケーション、アプリケーションにインストールされているコンピューターまたはアプリケーションの現在のユーザーに関連する情報にアクセスすることができます。  
  
## <a name="myapplication-mycomputer-and-myuser"></a>My.Application、My.Computer、および My.User  
 次の例では、情報をする方法を示しますを使用して取得`My`です。  
  
 [!code-vb[VbVbcnMy#1](../../../visual-basic/developing-apps/development-with-my/codesnippet/VisualBasic/performing-tasks-with-my-application-my-computer-and-my-user_1.vb)]  
  
 [!code-vb[VbVbcnMy#2](../../../visual-basic/developing-apps/development-with-my/codesnippet/VisualBasic/performing-tasks-with-my-application-my-computer-and-my-user_2.vb)]  
  
 情報を取得するだけでなくこれら 3 つのオブジェクトを介して公開されるメンバーも、そのオブジェクトに関連するメソッドを実行できます。 インスタンスのさまざまなファイルを操作または経由でのレジストリを更新する方法を表示できます`My.Computer`です。  
  
 ファイル I/O は簡単かつ短時間では大幅に`My`さまざまなファイル、ディレクトリ、およびドライブを操作のメソッドとプロパティが含まれます。 <xref:Microsoft.VisualBasic.FileIO.TextFieldParser>オブジェクトは、大きな構造化されたファイルが区切られたまたは固定幅フィールドから読み取ることができます。 この例を開いて、`TextFieldParser``reader`からの読み取りを使用して`C:\TestFolder1\test1.txt`です。  
  
 [!code-vb[VbVbalrTextFieldParser#23](../../../visual-basic/developing-apps/development-with-my/codesnippet/VisualBasic/performing-tasks-with-my-application-my-computer-and-my-user_3.vb)]  
  
 `My.Application` アプリケーションのカルチャを変更できます。 次の例では、このメソッドを呼び出す方法を示します。  
  
 [!code-vb[VbVbcnMy#3](../../../visual-basic/developing-apps/development-with-my/codesnippet/VisualBasic/performing-tasks-with-my-application-my-computer-and-my-user_4.vb)]  
  
## <a name="see-also"></a>関連項目  
 <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>  
 <xref:Microsoft.VisualBasic.Devices.Computer>  
 <xref:Microsoft.VisualBasic.ApplicationServices.User>  
 [プロジェクトの種類に応じた My の機能](../../../visual-basic/developing-apps/development-with-my/how-my-depends-on-project-type.md)
