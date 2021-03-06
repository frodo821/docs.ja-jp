---
title: -win32resource
ms.date: 03/13/2018
f1_keywords:
- -win32resource
- win32resource
helpviewer_keywords:
- /win32resource compiler option [Visual Basic]
- -win32resource compiler option [Visual Basic]
- win32resource compiler option [Visual Basic]
ms.assetid: e226946d-19ce-4cc9-91f5-aed24f77aa2b
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: ac495e10be2ec1534dc9d9081aef369773d93e17
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
---
# <a name="-win32resource"></a>-win32resource
出力ファイルには、Win32 リソース ファイルを挿入します。  
  
## <a name="syntax"></a>構文  
  
```  
-win32resource:filename  
```  
  
## <a name="arguments"></a>引数  
 `filename`  
 出力ファイルに追加するリソース ファイルの名前。 ファイル名を引用符で囲みます ("")、スペースが含まれている場合。  
  
## <a name="remarks"></a>コメント  
 Microsoft Windows リソース コンパイラ (RC) とは、Win32 リソース ファイルを作成できます。  
  
 Win32 リソースは、バージョンを含めることができますか、ビットマップ (アイコン) 情報を入力でアプリケーションを識別する**ファイル エクスプ ローラー**です。 指定しない場合`-win32resource`コンパイラはアセンブリのバージョンに基づくバージョン情報を生成します。 `-win32resource`と`-win32icon`オプションは相互に排他的です。  
  
 参照してください[-linkresource (Visual Basic)](../../../visual-basic/reference/command-line-compiler/linkresource.md)参照に、[!INCLUDE[dnprdnshort](~/includes/dnprdnshort-md.md)]リソース ファイル、または[-リソース (Visual Basic)](../../../visual-basic/reference/command-line-compiler/resource.md)をアタッチする、[!INCLUDE[dnprdnshort](~/includes/dnprdnshort-md.md)]リソース ファイル。  
  
> [!NOTE]
>  `-win32resource`オプションは、Visual Studio 開発環境からは利用できません; は、コマンドラインからコンパイルするときにのみ使用します。  
  
## <a name="example"></a>例  
 次のコードのコンパイル`In.vb`Win32 リソース ファイルをアタッチおよび`Rf.res`:  
  
```console  
vbc -win32resource:rf.res in.vb  
```  
  
## <a name="see-also"></a>関連項目  
 [Visual Basic のコマンド ライン コンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)  
 [コンパイル コマンド ラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
