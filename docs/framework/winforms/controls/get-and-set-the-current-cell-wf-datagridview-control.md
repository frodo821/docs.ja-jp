---
title: '方法 : Windows フォーム DataGridView コントロールの現在のセルを取得および設定する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGridView control [Windows Forms], getting current cell
- DataGridView control [Windows Forms], setting current cell
- cells [Windows Forms], getting and setting current
ms.assetid: b0e41e57-493a-4bd0-9376-a6f76723540c
ms.openlocfilehash: 0a3c8a891bf3f8424158a7266c3752edc33e8805
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
ms.locfileid: "33527548"
---
# <a name="how-to-get-and-set-the-current-cell-in-the-windows-forms-datagridview-control"></a>方法 : Windows フォーム DataGridView コントロールの現在のセルを取得および設定する
対話、<xref:System.Windows.Forms.DataGridView>多くの場合、ことをプログラムによって検出されるセルが現在アクティブなが必要です。 また、現在のセルを変更する必要があります。 これらのタスクを行うことができます、<xref:System.Windows.Forms.DataGridView.CurrentCell%2A>プロパティです。  
  
> [!NOTE]
>  行または列を持つ現在のセルを設定することはできません、<xref:System.Windows.Forms.DataGridViewBand.Visible%2A>プロパティに設定`false`です。  
  
 によって、<xref:System.Windows.Forms.DataGridView>現在のセルを変更するコントロールの選択モードが選択を変更できます。 詳細については、次を参照してください。 [Windows フォーム DataGridView コントロールで選択モード](../../../../docs/framework/winforms/controls/selection-modes-in-the-windows-forms-datagridview-control.md)です。  
  
### <a name="to-get-the-current-cell-programmatically"></a>現在のセルをプログラムで取得するには  
  
-   使用して、<xref:System.Windows.Forms.DataGridView>コントロールの<xref:System.Windows.Forms.DataGridView.CurrentCell%2A>プロパティです。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#080](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#080)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#080](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#080)]  
  
### <a name="to-set-the-current-cell-programmatically"></a>現在のセルをコードから設定するには  
  
-   設定、<xref:System.Windows.Forms.DataGridView.CurrentCell%2A>のプロパティ、<xref:System.Windows.Forms.DataGridView>コントロール。 次のコード例では、現在のセルは行の 0、1 列目に設定されます。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#085](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#085)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#085](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#085)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
-   <xref:System.Windows.Forms.Button> という名前のコントロール`getCurrentCellButton`と`setCurrentCellButton`です。 Visual c# では、割り当てる必要がある、<xref:System.Windows.Forms.Control.Click>のコード例に関連付けられているイベント ハンドラーには、各ボタンのイベントです。  
  
-   `dataGridView1` という名前の <xref:System.Windows.Forms.DataGridView> コントロール。  
  
-   <xref:System?displayProperty=nameWithType> アセンブリおよび <xref:System.Windows.Forms?displayProperty=nameWithType> アセンブリへの参照。  
  
## <a name="see-also"></a>関連項目  
 <xref:System.Windows.Forms.DataGridView>  
 <xref:System.Windows.Forms.DataGridView.CurrentCell%2A?displayProperty=nameWithType>  
 [Windows フォーム DataGridView コントロールでの列、行、およびセルの基本機能](../../../../docs/framework/winforms/controls/basic-column-row-and-cell-features-wf-datagridview-control.md)  
 [Windows フォーム DataGridView コントロールの選択モード](../../../../docs/framework/winforms/controls/selection-modes-in-the-windows-forms-datagridview-control.md)
