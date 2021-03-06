---
title: 配列
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- class library design guidelines [.NET Framework], arrays
- arrays [.NET Framework], usage guidelines
- empty arrays
ms.assetid: 66a1b3d8-6f3f-4715-b235-e1ff95e32d8e
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 2945ead7c22b759ce88f6585e2254e9bc540a7ef
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
---
# <a name="arrays"></a>配列
**✓ しないで**パブリック Api で配列にコレクションの使用を優先します。 [コレクション](../../../docs/standard/design-guidelines/guidelines-for-collections.md)コレクションと配列から選択する方法の詳細についても説明します。  
  
 **X しないで**読み取り専用配列フィールドを使用します。 フィールド自体は読み取り専用と変更できない配列内の要素を変更することができます。  
  
 **✓ を検討してください**多次元配列ではなくジャグ配列を使用します。  
  
 ジャグ配列は、要素も配列を含む配列です。 さまざまなサイズは小さい無駄な空間データ (たとえば、スパース マトリックス) の一部のセットと比較して多次元配列の要素を構成する配列ができます。 さらに、いくつかのシナリオで実行時パフォーマンスの向上を発生する可能性がありますので、CLR は、ジャグ配列のインデックス操作を最適化します。  
  
 *部分 © 2005、2009 Microsoft Corporation します。All rights reserved.*  
  
 *ピアソン教育, Inc. からのアクセス許可によって検出[Framework デザイン ガイドライン: 規則、表現方法、および再利用可能な .NET ライブラリを第 2 版パターン](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)は Cwalina と Brad Abrams、2008 年 10 月 22 日で発行されました。Microsoft Windows 開発シリーズの一部として、Addison-wesley Professional。*  
  
## <a name="see-also"></a>関連項目  
 <xref:System.Array>  
 [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)  
 [使用方法のガイドライン](../../../docs/standard/design-guidelines/usage-guidelines.md)
