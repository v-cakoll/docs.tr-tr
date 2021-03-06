---
title: StatusBar Denetiminde Hangi Panelin Tıklandığını Belirleyin
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- status bars [Windows Forms], determining panel clicked
- panels [Windows Forms], determining clicked
- StatusBar control [Windows Forms], coding panel click events
- StatusBar control [Windows Forms], determining panel clicked
- PanelClick event [Windows Forms], determining panel clicked
- Panel control [Windows Forms], determining click
ms.assetid: d14c6092-04b2-4a07-8ddf-0dd11277ff5f
ms.openlocfilehash: eb3b10d515ba5b62236594e063ca7f060b34b73e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79182358"
---
# <a name="how-to-determine-which-panel-in-the-windows-forms-statusbar-control-was-clicked"></a>Nasıl yapılır: Windows Forms StatusBar Denetiminde Hangi Panele Tıklandığını Belirleme
> [!IMPORTANT]
> Ve <xref:System.Windows.Forms.StatusStrip> <xref:System.Windows.Forms.ToolStripStatusLabel> denetimleri değiştirin ve <xref:System.Windows.Forms.StatusBar> <xref:System.Windows.Forms.StatusBarPanel> işlevsellik ekleyin ve denetimleri; ancak, <xref:System.Windows.Forms.StatusBar> ve <xref:System.Windows.Forms.StatusBarPanel> denetimler, isterseniz, hem geriye dönük uyumluluk hem de gelecekteki kullanım için korunur.  
  
 Kullanıcı tıklamalarına yanıt vermek için [StatusBar Control](statusbar-control-windows-forms.md) denetimini <xref:System.Windows.Forms.StatusBar.PanelClick> programlamak için olay içinde bir servis talebi bildirimi kullanın. Olay, tıklatılana <xref:System.Windows.Forms.StatusBarPanel>bir başvuru içeren bir bağımsız değişken (panel bağımsız değişkeni) içerir. Bu başvuruyu kullanarak, tıklanan panelin dizinini belirleyebilir ve buna göre programlayabilirsiniz.  
  
> [!NOTE]
> Denetimin <xref:System.Windows.Forms.StatusBar> <xref:System.Windows.Forms.StatusBar.ShowPanels%2A> özelliğinin '' olarak `true`ayarlandığından emin olun  
  
### <a name="to-determine-which-panel-was-clicked"></a>Hangi panelin tıklandığını belirlemek için  
  
1. Olay <xref:System.Windows.Forms.StatusBar.PanelClick> işleyicisinde, olay `Select Case` bağımsız değişkenlerinde `switch case` tıklanan panelin dizinini inceleyerek hangi panelin tıklandığını belirlemek için (Visual Basic'te) veya (Visual C# veya Visual C++) deyimini kullanın.  
  
     Aşağıdaki kod örneği, <xref:System.Windows.Forms.StatusBar> bir denetimin `StatusBar1`ve iki <xref:System.Windows.Forms.StatusBarPanel> nesnenin formdaki `StatusBarPanel1` varlığını `StatusBarPanel2`ve .  
  
    ```vb  
    Private Sub StatusBar1_PanelClick(ByVal sender As System.Object, ByVal e As System.Windows.Forms.StatusBarPanelClickEventArgs) Handles StatusBar1.PanelClick  
       Select Case StatusBar1.Panels.IndexOf(e.StatusBarPanel)  
         Case 0  
           MessageBox.Show("You have clicked Panel One.")  
         Case 1  
           MessageBox.Show("You have clicked Panel Two.")  
       End Select  
    End Sub  
    ```  
  
    ```csharp  
    private void statusBar1_PanelClick(object sender,
    System.Windows.Forms.StatusBarPanelClickEventArgs e)  
    {  
       switch (statusBar1.Panels.IndexOf(e.StatusBarPanel))  
       {  
          case 0 :  
             MessageBox.Show("You have clicked Panel One.");  
             break;  
          case 1 :  
             MessageBox.Show("You have clicked Panel Two.");  
             break;  
       }  
    }  
    ```  
  
    ```cpp  
    private:  
       void statusBar1_PanelClick(System::Object ^  sender,  
          System::Windows::Forms::StatusBarPanelClickEventArgs ^  e)  
       {  
          switch (statusBar1->Panels->IndexOf(e->StatusBarPanel))  
          {  
             case 0 :  
                MessageBox::Show("You have clicked Panel One.");  
                break;  
             case 1 :  
                MessageBox::Show("You have clicked Panel Two.");  
                break;  
          }  
       }  
    ```  
  
     (Görsel C#, Görsel C++) Olay işleyicisini kaydetmek için aşağıdaki kodu formun oluşturucusuna yerleştirin.  
  
    ```csharp  
    this.statusBar1.PanelClick += new
       System.Windows.Forms.StatusBarPanelClickEventHandler
       (this.statusBar1_PanelClick);  
    ```  
  
    ```cpp  
    this->statusBar1->PanelClick += gcnew  
       System::Windows::Forms::StatusBarPanelClickEventHandler  
       (this, &Form1::statusBar1_PanelClick);  
    ```  
  
## <a name="see-also"></a>Ayrıca bkz.

- <xref:System.Windows.Forms.StatusBar>
- <xref:System.Windows.Forms.ToolStripStatusLabel>
- [Nasıl yapılır: Durum Çubuğu Panellerinin Boyutunu Ayarlama](how-to-set-the-size-of-status-bar-panels.md)
- [İzlenecek yol: Çalışma Zamanında Durum Çubuğu Bilgilerini Güncelleştirme](walkthrough-updating-status-bar-information-at-run-time.md)
- [StatusBar Denetimine Genel Bakış](statusbar-control-overview-windows-forms.md)
