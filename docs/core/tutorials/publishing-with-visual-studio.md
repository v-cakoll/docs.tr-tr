---
title: Visual Studio kullanarak bir .NET Core konsol uygulaması yayımlama
description: Yayımlama, .NET Core uygulamasını çalıştırmak için gereken dosya kümesini oluşturur.
ms.date: 06/08/2020
dev_langs:
- csharp
- vb
ms.custom: vs-dotnet
ms.openlocfilehash: 44646a307d230db395b55b9dec5acfd168605940
ms.sourcegitcommit: 1cbd77da54405ea7dba343ac0334fb03237d25d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84701290"
---
# <a name="tutorial-publish-a-net-core-console-application-using-visual-studio"></a>Öğretici: Visual Studio kullanarak bir .NET Core konsol uygulaması yayımlama

Bu öğreticide, diğer kullanıcıların çalışması için bir konsol uygulamasının nasıl yayımlanacağı gösterilmektedir. Yayımlama, uygulamanızı çalıştırmak için gereken dosya kümesini oluşturur. Dosyaları dağıtmak için, onları hedef makineye kopyalayın.

## <a name="prerequisites"></a>Önkoşullar

- Bu öğretici, [Visual Studio 2019 ' de .NET Core konsol uygulaması oluşturma](with-visual-studio.md)bölümünde oluşturduğunuz konsol uygulamasıyla birlikte kullanılır.

## <a name="publish-the-app"></a>Uygulamayı yayımlama

1. Visual Studio’yu çalıştırın.

1. [Visual Studio 'da .NET Core konsol uygulaması oluşturma](with-visual-studio.md)bölümünde oluşturduğunuz *HelloWorld* projesini açın.

1. Visual Studio 'Nun yayın derleme yapılandırması ' nı kullandığınızdan emin olun. Gerekirse, araç çubuğundaki derleme yapılandırma ayarını **Hata Ayıkla** 'dan **Release**olarak değiştirin.

   ![Yayın derlemesi seçiliyken Visual Studio araç çubuğu](media/publishing-with-visual-studio/visual-studio-toolbar-release.png)

1. **HelloWorld** projesine (HelloWorld çözümüne değil) sağ tıklayın ve menüden **Yayımla** ' yı seçin.

   ![Visual Studio Yayımla bağlam menüsü](media/publishing-with-visual-studio/publish-context-menu.png)

1. **Yayımla** sayfasının **hedef** sekmesinde **klasör**' i seçin ve ardından **İleri**' yi seçin.

   ![Visual Studio 'da bir yayımlama hedefi seçin](media/publishing-with-visual-studio/pick-publish-target.png)

1. **Yayımla** sayfasının **konum** sekmesinde **son**' u seçin.

   ![Visual Studio yayımlama sayfası konum sekmesi](media/publishing-with-visual-studio/publish-page-loc-tab.png)

1. **Yayımla** penceresinin **Yayımla** sekmesinde **Yayımla**' yı seçin.

   ![Visual Studio Yayımla penceresi](media/publishing-with-visual-studio/publish-page.png)

## <a name="inspect-the-files"></a>Dosyaları inceleyin

Varsayılan olarak, yayımlama işlemi, yayımlanan uygulamanın .NET Core çalışma zamanının yüklü olduğu makinede çalıştığı bir dağıtım türü olan çerçeveye bağlı bir dağıtım oluşturur. Kullanıcılar, çalıştırılabilir dosyayı çift tıklayarak veya komut isteminden komutu vererek, yayımlanan uygulamayı çalıştırabilir `dotnet HelloWorld.dll` .

Aşağıdaki adımlarda, yayımlama işlemi tarafından oluşturulan dosyalara bakacaksınız.

1. **Çözüm Gezgini**, **tüm dosyaları göster**' i seçin.

1. Proje klasöründe *bin/Release/netcoreapp 3.1/Yayımla*' yı genişletin.

   :::image type="content" source="media/publishing-with-visual-studio/published-files-output.png" alt-text="Yayımlanan dosyaları gösterme Çözüm Gezgini":::

   Görüntüde gösterildiği gibi, yayımlanan çıktı aşağıdaki dosyaları içerir:

   * *ÜzerindeHelloWorld.deps.js*

      Bu, uygulamanın çalışma zamanı bağımlılıkları dosyasıdır. Uygulamayı çalıştırmak için gereken .NET Core bileşenlerini ve kitaplıklarını (uygulamanızı içeren dinamik bağlantı kitaplığı dahil) tanımlar. Daha fazla bilgi için bkz. [çalışma zamanı yapılandırma dosyaları](https://github.com/dotnet/cli/blob/85ca206d84633d658d7363894c4ea9d59e515c1a/Documentation/specs/runtime-configuration-file.md).

   * *HelloWorld.dll*

      Bu, uygulamanın [çerçeveye bağımlı dağıtım](../deploying/deploy-with-cli.md#framework-dependent-deployment) sürümüdür. Bu dinamik bağlantı kitaplığını yürütmek için `dotnet HelloWorld.dll` bir komut istemine girin. Uygulamayı çalıştırma yöntemi, .NET Core çalışma zamanının yüklü olduğu tüm platformlarda çalışır.

   * *HelloWorld.exe*

      Bu, uygulamanın [çerçeveye bağımlı yürütülebilir](../deploying/deploy-with-cli.md#framework-dependent-executable) sürümüdür. Çalıştırmak için `HelloWorld.exe` bir komut istemine girin. Dosya işletim sistemine özgüdür.

   * *HelloWorld. pdb* (dağıtım için isteğe bağlı)

      Bu, hata ayıklama sembolleri dosyasıdır. Bu dosyayı uygulamanızla birlikte dağıtmanız gerekmez, ancak uygulamanızın yayımlanan sürümünde hata ayıklaması yapmanız gereken bir olaya kaydetmeniz gerekir.

   * *ÜzerindeHelloWorld.runtimeconfig.js*

      Bu, uygulamanın çalışma zamanı yapılandırma dosyasıdır. Uygulamanızın üzerinde çalışmak üzere oluşturulduğu .NET Core sürümünü tanımlar. Ayrıca, buna yapılandırma seçenekleri de ekleyebilirsiniz. Daha fazla bilgi için bkz. [.NET Core çalışma zamanı yapılandırma ayarları](../run-time-config/index.md#runtimeconfigjson).

## <a name="run-the-published-app"></a>Yayımlanan uygulamayı çalıştırma

1. **Çözüm Gezgini**, *Yayımla* klasörüne sağ tıklayın ve **tam yolu Kopyala**' yı seçin.

1. Bir komut istemi açın ve *Yayımla* klasörüne gidin. Bunu yapmak için `cd` tam yolu girin ve ardından yapıştırın. Örnek:

   ```
   cd C:\Projects\HelloWorld\bin\Release\netcoreapp3.1\publish\
   ```

1. Yürütülebilir dosyayı kullanarak uygulamayı çalıştırın:

   1. Yazın `HelloWorld.exe` ve ENTER <kbd>Enter</kbd>tuşuna basın.

   1. İstemine yanıt olarak bir ad girin ve çıkmak için herhangi bir tuşa basın.

1. Şu komutu kullanarak uygulamayı çalıştırın `dotnet` :

   1. Yazın `dotnet HelloWorld.dll` ve ENTER <kbd>Enter</kbd>tuşuna basın.

   1. İstemine yanıt olarak bir ad girin ve çıkmak için herhangi bir tuşa basın.

## <a name="additional-resources"></a>Ek kaynaklar

- [.NET Core uygulama dağıtımı](../deploying/index.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir konsol uygulaması yayımladınız. Sonraki öğreticide, bir sınıf kitaplığı oluşturursunuz.

> [!div class="nextstepaction"]
> [Visual Studio’da bir .NET Standard kitaplığı oluşturma](library-with-visual-studio.md)
