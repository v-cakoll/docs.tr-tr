---
title: .NET Core 'a Windows Forms uygulaması bağlantı noktası
description: Bir .NET Framework Windows Forms uygulamasının Windows için .NET Core 'a nasıl bağlantı noktası alınacağını öğretir.
author: Thraka
ms.author: adegeo
ms.date: 01/24/2020
ms.openlocfilehash: efa73428c816eddc00c62c2275d3457c92284388
ms.sourcegitcommit: 488aced39b5f374bc0a139a4993616a54d15baf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83206129"
---
# <a name="how-to-port-a-windows-forms-desktop-app-to-net-core"></a>Windows Forms masaüstü uygulamasının .NET Core 'a bağlantı noktası oluşturma

Bu makalede Windows Forms tabanlı masaüstü uygulamanızın .NET Framework .NET Core 3,0 veya sonraki bir sürüme nasıl bağlantı kurulacağı açıklanmaktadır. .NET Core 3,0 SDK Windows Forms uygulamaları için destek içerir. Windows Forms hala yalnızca Windows Framework ' ü ve Windows üzerinde çalışır. Bu örnek, projenizi oluşturmak ve yönetmek için .NET Core SDK CLı kullanır.

Bu makalede, geçiş için kullanılan dosya türlerini tanımlamak için çeşitli adlar kullanılır. Projeniz geçirilirken dosyalarınız farklı şekilde adlandırılır, bu sayede bunları aşağıda listelenen adlarla eşleştirin:

| Dosya | Açıklama |
| ---- | ----------- |
| **Uygps. sln** | Çözüm dosyasının adı. |
| **MyForms. csproj** | .NET Framework Windows Forms projenin bağlantı noktasına adı. |
| **MyFormsCore. csproj** | Oluşturduğunuz yeni .NET Core projesinin adı. |
| **MyAppCore. exe** | .NET Core Windows Forms uygulaması çalıştırılabilir. |

## <a name="prerequisites"></a>Ön koşullar

- Yapmak istediğiniz tüm tasarımcı çalışmaları için [Visual Studio 2019 16,5 Preview 1](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=community&ch=pre&rel=16) veya üzeri. [Visual Studio 'nun en son önizleme sürümüne](https://visualstudio.microsoft.com/vs/preview/)güncelleştirmeniz önerilir.

  Aşağıdaki Visual Studio iş yüklerini yükler:
  
  - .NET masaüstü geliştirme
  - .NET Core çoklu platform geliştirme

- Sorun olmadan oluşturulup çalışan bir çözümde çalışan bir Windows Forms projesi.
- C# dilinde kodlanmış bir proje.

> [!NOTE]
> .NET Core 3,0 projeleri yalnızca **Visual Studio 2019** veya sonraki bir sürümde desteklenir. **Visual Studio 2019 sürüm 16,5 Preview 1**' den başlayarak, .net Core Windows Forms Tasarımcısı da desteklenir.
>
> Tasarımcıyı etkinleştirmek için **Araçlar**  >  **Seçenekler**  >  **ortam**  >  **Önizleme özellikleri** ' ne gidin ve **.NET Core uygulamaları için Önizleme Windows Forms Tasarımcısını kullan** seçeneğini belirleyin.

### <a name="consider"></a>Seçmeyi

Bir .NET Framework Windows Forms uygulamasının taşıma sırasında göz önünde bulundurmanız gereken birkaç nokta vardır.

01. Uygulamanızın geçiş için iyi bir aday olduğunu kontrol edin.

    Projenizin .NET Core 3,0 ' e geçiş olacağını öğrenmek için [.net taşınabilirlik Çözümleyicisi](../../standard/analyzers/portability-analyzer.md) ' ni kullanın. Projenizde .NET Core 3,0 ile ilgili sorunlar varsa, çözümleyici bu sorunları belirlemenize yardımcı olur.

01. Farklı bir Windows Forms sürümü kullanıyorsunuz.

    .NET Core 3,0 Preview 1 yayınlandığında, Windows Forms GitHub 'da açık kaynak olarak gitti. .NET Core Windows Forms kodu, .NET Framework Windows Forms kod temelinin çatalından oluşur. Bazı farklılıklar olabilir ve uygulamanızın bağlantı noktası olmayacaktır.

01. [Windows Uyumluluk Paketi][compat-pack] , geçiş yapmanıza yardımcı olabilir.

    .NET Framework ' de kullanılabilen bazı API 'Ler .NET Core 3,0 ' de kullanılamaz. [Windows Uyumluluk Paketi][compat-pack] , bu API 'lerin çoğunu ekler ve Windows Forms uygulamanızın .NET Core ile uyumlu hale gelmesine yardımcı olabilir.

01. Projeniz tarafından kullanılan NuGet paketlerini güncelleştirin.

    Herhangi bir geçişten önce NuGet paketlerinin en son sürümlerini kullanmak her zaman iyi bir uygulamadır. Uygulamanız herhangi bir NuGet paketine başvuruyorsa, bunları en son sürüme güncelleştirin. Uygulamanızın başarıyla derlemediğinden emin olun. Yükseltmeden sonra, herhangi bir paket hatası varsa, paketi, kodunuzu kesen en son sürüme indirgeymelisiniz.

## <a name="create-a-new-sdk-project"></a>Yeni bir SDK projesi oluştur

Oluşturduğunuz yeni .NET Core 3,0 projesi .NET Framework projenizden farklı bir dizinde olmalıdır. İkisi de aynı dizinde ise, **obj** dizininde oluşturulan dosyalarla çakışmalarıyla karşılaşabilirsiniz. Bu örnekte, **SolutionFolder** dizininde **Myformsappcore** adlı bir dizin oluşturacağız:

```
SolutionFolder
├───MyApps.sln
├───MyFormsApp
│   └───MyForms.csproj
└───MyFormsAppCore      <--- New folder for core project
```

Sonra, **Myformscore. csproj** projesini **Myformsappcore** dizininde oluşturmanız gerekir. Bu dosyayı tercih ettiğiniz metin düzenleyicisini kullanarak el ile oluşturabilirsiniz. Aşağıdaki XML 'e yapıştırın:

```xml
<Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>netcoreapp3.0</TargetFramework>
    <UseWindowsForms>true</UseWindowsForms>
  </PropertyGroup>

</Project>
```

Proje dosyasını el ile oluşturmak istemiyorsanız, projeyi oluşturmak için Visual Studio 'Yu veya .NET Core SDK kullanabilirsiniz. Ancak proje dosyası hariç proje şablonu tarafından oluşturulan diğer tüm dosyaları silmeniz gerekir. SDK 'yı kullanmak için **SolutionFolder** dizininden aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet new winforms -o MyFormsAppCore -n MyFormsCore
```

**Myformscore. csproj**öğesini oluşturduktan sonra, dizin yapınız aşağıdaki gibi görünmelidir:

```
SolutionFolder
├───MyApps.sln
├───MyFormsApp
│   └───MyForms.csproj
└───MyFormsAppCore
    └───MyFormsCore.csproj
```

Visual Studio ya da **SolutionFolder** dizininden .NET Core CLI **myformscore. csproj** projesini **uygulamaps. sln** ' ye ekleyin:

```dotnetcli
dotnet sln add .\MyFormsAppCore\MyFormsCore.csproj
```

## <a name="fix-assembly-info-generation"></a>Derleme bilgileri oluşturmayı çözme

.NET Framework ile oluşturulan Windows Forms projeler `AssemblyInfo.cs` , oluşturulacak derlemenin sürümü gibi derleme özniteliklerini içeren bir dosya içerir. SDK stilindeki projeler, SDK proje dosyasını temel alarak bu bilgileri sizin için otomatik olarak oluşturur. Her iki tür "derleme bilgisi" de bir çakışma oluşturur. Otomatik oluşturmayı devre dışı bırakarak bu sorunu çözün. Bu, projeyi mevcut dosyanızı kullanmaya zorlar `AssemblyInfo.cs` .

Ana düğüme eklemenin üç ayarı vardır `<PropertyGroup>` .

- **Generateassemblyınfo**\
Bu özelliği ' a ayarladığınızda `false` , derleme özniteliklerini oluşturmaz. Bu, .NET Framework projesinden mevcut dosya ile çakışmayı önler `AssemblyInfo.cs` .

- **AssemblyName**\
Bu özelliğin değeri, derlerken oluşturulan çıkış ikilisi olur. Ada eklenen bir uzantıya gerek yoktur. Örneğin, kullanılarak `MyCoreApp` üretir `MyCoreApp.exe` .

- **RootNamespace**\
Projeniz tarafından kullanılan varsayılan ad alanı. Bu, .NET Framework projesinin varsayılan ad alanıyla eşleşmelidir.

Bu üç öğeyi `<PropertyGroup>` dosyadaki düğümüne ekleyin `MyFormsCore.csproj` :

```xml
<Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>netcoreapp3.0</TargetFramework>
    <UseWindowsForms>true</UseWindowsForms>

    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    <AssemblyName>MyCoreApp</AssemblyName>
    <RootNamespace>WindowsFormsApp1</RootNamespace>
  </PropertyGroup>

</Project>
```

## <a name="add-source-code"></a>Kaynak kodu ekle

Şu anda **Myformscore. csproj** projesi hiçbir kodu derlemez. Varsayılan olarak, .NET Core projeleri geçerli dizine ve tüm alt dizinlere tüm kaynak kodlarını otomatik olarak ekler. Projeyi, .NET Framework projesinden bir göreli yol kullanarak kod içerecek şekilde yapılandırmanız gerekir. .NET Framework projeniz, formlarınızın simgeleri ve kaynakları için **. resx** dosyalarını kullandıysanız, bunları da eklemeniz gerekir.

Aşağıdaki `<ItemGroup>` düğümü projenize ekleyin. Her bir bildirimde alt dizinler içeren bir dosya glob kalıbı bulunur.

```xml
  <ItemGroup>
    <Compile Include="..\MyFormsApp\**\*.cs" />
    <EmbeddedResource Include="..\MyFormsApp\**\*.resx" />
  </ItemGroup>
```

Alternatif olarak, `<Compile>` `<EmbeddedResource>` .NET Framework projenizdeki her dosya için bir veya girişi oluşturabilirsiniz.

## <a name="add-nuget-packages"></a>NuGet paketlerini ekleme

.NET Framework projesi tarafından başvurulan her bir NuGet paketini .NET Core projesine ekleyin.

Büyük olasılıkla .NET Framework Windows Forms uygulamanızın, projeniz tarafından başvurulan tüm NuGet paketlerinin listesini içeren bir **Packages. config** dosyası vardır. .NET Core projesine hangi NuGet paketlerinin ekleneceğini öğrenmek için bu listeye bakabilirsiniz. Örneğin, .NET Framework projesi `MetroFramework` ,, `MetroFramework.Design` ve NuGet paketlerine başvuruyorsa, `MetroFramework.Fonts` her birini Visual Studio ya da **SolutionFolder** dizininden .NET Core CLI ile projeye ekleyin:

```dotnetcli
dotnet add .\MyFormsAppCore\MyFormsCore.csproj package MetroFramework
dotnet add .\MyFormsAppCore\MyFormsCore.csproj package MetroFramework.Design
dotnet add .\MyFormsAppCore\MyFormsCore.csproj package MetroFramework.Fonts
```

Önceki komutlar **Myformscore. csproj** projesine aşağıdaki NuGet başvurularını ekler:

```xml
  <ItemGroup>
    <PackageReference Include="MetroFramework" Version="1.2.0.3" />
    <PackageReference Include="MetroFramework.Design" Version="1.2.0.3" />
    <PackageReference Include="MetroFramework.Fonts" Version="1.2.0.3" />
  </ItemGroup>
```

## <a name="port-control-libraries"></a>Bağlantı noktası denetim kitaplıkları

Bağlantı noktası için bir Windows Forms denetimleri kitaplığı projeniz varsa, yönergeler birkaç ayar haricinde bir .NET Framework Windows Forms uygulama projesinin taşıma ile aynıdır. Bir yürütülebilir dosyaya derlemek yerine bir kitaplığa derleyebilirsiniz. Kaynak kodunuzu içeren genelleştirmeler dosya yollarının yanı sıra, yürütülebilir proje ve kitaplık projesi arasındaki fark en düşük düzeydedir.

Önceki adımın örneğini kullanarak, üzerinde çalıştığımız proje ve dosyaları genişletmenizi sağlar.

| Dosya | Açıklama |
| ---- | ----------- |
| **Uygps. sln** | Çözüm dosyasının adı. |
| **MyControls. csproj** | .NET Framework Windows Forms adı, projeyi bağlantı noktasına denetler. |
| **MyControlsCore. csproj** | Oluşturduğunuz yeni .NET Core kitaplığı projesinin adı. |
| **MyCoreControls. dll** | .NET Core Windows Forms denetimleri kitaplığı. |

```
SolutionFolder
├───MyApps.sln
├───MyFormsApp
│   └───MyForms.csproj
├───MyFormsAppCore
│   └───MyFormsCore.csproj
│
├───MyFormsControls
│   └───MyControls.csproj
└───MyFormsControlsCore
    └───MyControlsCore.csproj   <--- New project for core controls
```

`MyControlsCore.csproj`Proje ve daha önce oluşturulan proje arasındaki farkları göz önünde bulundurun `MyFormsCore.csproj` .

```diff
 <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

   <PropertyGroup>
-    <OutputType>WinExe</OutputType>
     <TargetFramework>netcoreapp3.0</TargetFramework>
     <UseWindowsForms>true</UseWindowsForms>

     <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
-    <AssemblyName>MyCoreApp</AssemblyName>
-    <RootNamespace>WindowsFormsApp1</RootNamespace>
+    <AssemblyName>MyControlsCore</AssemblyName>
+    <RootNamespace>WindowsFormsControlLibrary1</RootNamespace>
   </PropertyGroup>

   <ItemGroup>
-    <Compile Include="..\MyFormsApp\**\*.cs" />
-    <EmbeddedResource Include="..\MyFormsApp\**\*.resx" />
+    <Compile Include="..\MyFormsControls\**\*.cs" />
+    <EmbeddedResource Include="..\MyFormsControls\**\*.resx" />
   </ItemGroup>

 </Project>
```

.NET Core Windows Forms denetimleri kitaplığı proje dosyasının nasıl görüneceğine ilişkin bir örnek aşağıda verilmiştir:

```xml
<Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

  <PropertyGroup>

    <TargetFramework>netcoreapp3.0</TargetFramework>
    <UseWindowsForms>true</UseWindowsForms>

    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    <AssemblyName>MyCoreControls</AssemblyName>
    <RootNamespace>WindowsFormsControlLibrary1</RootNamespace>
  </PropertyGroup>

  <ItemGroup>
    <Compile Include="..\MyFormsControls\**\*.cs" />
    <EmbeddedResource Include="..\MyFormsControls\**\*.resx" />
  </ItemGroup>

</Project>
```

Görebileceğiniz gibi, `<OutputType>` düğüm kaldırılmıştır ve bu, derleyicinin çalıştırılabilir yerine bir kitaplık oluşturmasını sağlar. `<AssemblyName>`Ve `<RootNamespace>` değiştirildi. Özellikle, `<RootNamespace>` taşıma yaptığınız Windows Forms denetimleri kitaplığı ad alanıyla eşleşmelidir. Son olarak, `<Compile>` ve `<EmbeddedResource>` düğümleri, taşıma yaptığınız Windows Forms denetimleri kitaplığının klasörünü işaret etmek üzere ayarlanmıştı.

Ardından, ana .NET Core **Myformscore. csproj** projesinde, yeni .net Core Windows Forms denetim kitaplığına bir başvuru ekleyin. Visual Studio ya da **SolutionFolder** dizininden .NET Core CLI bir başvuru ekleyin:

```dotnetcli
dotnet add .\MyFormsAppCore\MyFormsCore.csproj reference .\MyFormsControlsCore\MyControlsCore.csproj
```

Önceki komut **Myformscore. csproj** projesine aşağıdakini ekler:

```xml
  <ItemGroup>
    <ProjectReference Include="..\MyFormsControlsCore\MyControlsCore.csproj" />
  </ItemGroup>
```

## <a name="compilation-problems"></a>Derleme sorunları

Projelerinizi derlerken sorunlarla karşılaşırsanız, .NET Core 'da .NET Framework, ancak kullanılamayan yalnızca Windows salt Windows API 'Lerini kullanıyor olabilirsiniz. [Windows Uyumluluk Paketi][compat-pack] NuGet paketini projenize eklemeyi deneyebilirsiniz. Bu paket yalnızca Windows üzerinde çalışır ve .NET Core ve .NET Standard projelerine 20.000 Windows API 'Leri ekler.

```dotnetcli
dotnet add .\MyFormsAppCore\MyFormsCore.csproj package Microsoft.Windows.Compatibility
```

Önceki komut **Myformscore. csproj** projesine aşağıdakini ekler:

```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.Windows.Compatibility" Version="3.1.0" />
  </ItemGroup>
```

## <a name="windows-forms-designer"></a>Windows Form Tasarımcısı

Bu makalede açıklandığı gibi, Visual Studio 2019 yalnızca .NET Framework projelerinde form tasarımcısını destekler. Yan yana .NET Core projesi oluşturarak, formları tasarlamak için .NET Framework projesi kullanırken projenizi .NET Core ile test edebilirsiniz. Çözüm dosyanız hem .NET Framework hem de .NET Core projelerini içerir. .NET Framework projesindeki formlarınızı ve denetimlerinizi ekleyin ve tasarlayın ve .NET Core projelerine eklediğimiz dosya glob desenlerine bağlı olarak, .NET Core projelerine yeni veya değiştirilmiş dosyalar otomatik olarak dahil edilir.

Visual Studio 2019 Windows Form Tasarımcısı desteklediğinde, .NET Core proje dosyanızın içeriğini kopyalayabilir/.NET Framework proje dosyasına yapıştırabilirsiniz. Sonra ve öğeleriyle eklenen glob düzenlerini silin `<Source>` `<EmbeddedResource>` . Uygulamanız tarafından kullanılan herhangi bir proje başvurusunun yolunu düzeltin. Bu, .NET Framework projesini bir .NET Core projesine etkin bir şekilde yükseltir.

## <a name="next-steps"></a>Sonraki adımlar

- [.NET Framework 'den .NET Core 'a son değişiklikler](../compatibility/fx-core.md)hakkında bilgi edinin.
- [Windows Uyumluluk Paketi][compat-pack]hakkında daha fazla bilgi edinin.
- .NET Framework Windows Forms projenizi .NET Core 'a [taşıma hakkında bir video](https://www.youtube.com/watch?v=upVQEUc_KwU) izleyin.

[compat-pack]: windows-compat-pack.md
