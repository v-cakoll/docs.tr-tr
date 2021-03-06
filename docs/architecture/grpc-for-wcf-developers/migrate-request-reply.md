---
title: WCF geliştiricileri için bir WCF isteği-yanıt hizmetini gRPC-gRPC 'ye geçirme
description: WCF 'den gRPC 'ye basit bir istek-yanıt hizmeti geçirmeyi öğrenin.
ms.date: 09/02/2019
ms.openlocfilehash: 018aa94a15cdcb1e0f559afb7b3a88cd4f915398
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628559"
---
# <a name="migrate-a-wcf-request-reply-service-to-a-grpc-unary-rpc"></a>WCF isteği yanıt verme hizmetini gRPC birli RPC 'ye geçirme

Bu bölümde, WCF 'de temel bir istek-yanıt hizmetinin ASP.NET Core gRPC 'deki birli RPC hizmetine geçirilmesi ele alınmaktadır. Bu hizmetler hem Windows Communication Foundation (WCF) hem de gRPC içindeki en basit hizmet türleridir. bu nedenle başlamak için mükemmel bir yerdir. Hizmeti geçirdikten sonra, bir .NET istemci uygulamasından hizmeti kullanmak için aynı `.proto` dosyasından bir istemci kitaplığı oluşturmayı öğreneceksiniz.

## <a name="the-wcf-solution"></a>WCF çözümü

[PortfoliosSample çözümü](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/PortfoliosSample/wcf/TraderSys) , belirli bir eğitime için tek bir portföy ya da tüm portföyleri indirmek üzere basit bir istek-yanıt portföyü hizmeti içerir. Hizmet, arabirim `IPortfolioService` `ServiceContract` özniteliğiyle tanımlanmıştır:

```csharp
[ServiceContract]
public interface IPortfolioService
{
    [OperationContract]
    Task<Portfolio> Get(Guid traderId, int portfolioId);

    [OperationContract]
    Task<List<Portfolio>> GetAll(Guid traderId);
}
```

`Portfolio` modeli, [DataContract](xref:System.Runtime.Serialization.DataContractAttribute) ile işaretlenmiş C# ve `PortfolioItem` nesnelerinin bir listesini içeren basit bir sınıftır. Bu modeller `TraderSys.PortfolioData` projesinde, bir veri erişimi soyutlamasını temsil eden bir depo sınıfıyla birlikte tanımlanmıştır.

```csharp
[DataContract]
public class Portfolio
{
    [DataMember]
    public int Id { get; set; }

    [DataMember]
    public Guid TraderId { get; set; }

    [DataMember]
    public List<PortfolioItem> Items { get; set; }
}

[DataContract]
public class PortfolioItem
{
    [DataMember]
    public int Id { get; set; }

    [DataMember]
    public int ShareId { get; set; }

    [DataMember]
    public int Holding { get; set; }

    [DataMember]
    public decimal Cost { get; set; }
}
```

`ServiceContract` uygulama, `DataContract` türlerinin örneklerini döndüren bağımlılık ekleme yoluyla sağlanmış bir depo sınıfı kullanır:

```csharp
public class PortfolioService : IPortfolioService
{
    private readonly IPortfolioRepository _repository;

    public PortfolioService(IPortfolioRepository repository)
    {
        _repository = repository;
    }

    public async Task<Portfolio> Get(Guid traderId, int portfolioId)
    {
        return await _repository.GetAsync(traderId, portfolioId);
    }

    public async Task<List<Portfolio>> GetAll(Guid traderId)
    {
        return await _repository.GetAllAsync(traderId);
    }
}
```

## <a name="the-portfoliosproto-file"></a>Portföyler. proto dosyası

Önceki bölümde yer alan yönergeleri izlediyseniz, şuna benzer bir `portfolios.proto` dosyası içeren bir gRPC projeniz olmalıdır:

```protobuf
syntax = "proto3";

option csharp_namespace = "TraderSys.Portfolios.Protos";

package PortfolioServer;

service Portfolios {
  // RPCs will go here
}
```

İlk adım `DataContract` sınıflarını prototip eşdeğerlerine geçirmeyecektir.

## <a name="convert-the-datacontract-classes-to-grpc-messages"></a>DataContract sınıflarını gRPC iletilerine Dönüştür

`PortfolioItem` sınıfı ilk olarak bir Prototipme iletisine dönüştürülür, çünkü `Portfolio` sınıfı buna bağlıdır. Sınıfı basittir ve özelliklerden üçü doğrudan gRPC veri türlerine eşlenir. Satın alımlardaki paylaşımlar için ödenen fiyatı temsil eden `Cost` özelliği bir `decimal` alanıdır. gRPC yalnızca `float` veya `double`, para birimi için uygun olmayan gerçek numaralar için destekler. Paylaşma fiyatları en az bir sent 'a göre farklılık gösterdiğinden, maliyet, ilal `int32` olarak ifade edilebilir.

> [!NOTE]
> `.proto` dosyanızdaki alan adları için camelCase kullanmayı unutmayın. C# Kod Oluşturucu, sizin Için PascalCase 'e dönüştürecek ve diğer dillerin kullanıcıları farklı kodlama standartlarını daha iyi bir şekilde tahmin ettiğiniz için teşekkür eder.

```protobuf
message PortfolioItem {
    int32 id = 1;
    int32 share_id = 2;
    int32 holding = 3;
    int32 cost_cents = 4;
}
```

`Portfolio` sınıfı biraz daha karmaşıktır. WCF kodunda, geliştirici `TraderId` özelliği için bir `Guid` kullanıyordu ve bir `List<PortfolioItem>`içerir. Birinci sınıf `UUID` türüne sahip olmayan prototipte, `traderId` alanı için bir `string` kullanmalı ve kendi kodunuzda ayrıştırmalısınız. Öğelerin listesi için alanındaki `repeated` anahtar sözcüğünü kullanın.

```protobuf
message Portfolio {
    int32 id = 1;
    string trader_id = 2;
    repeated PortfolioItem items = 3;
}
```

Artık veri iletilerine sahip olduğunuza göre, hizmet RPC uç noktalarını bildirebilirsiniz.

## <a name="convert-servicecontract-to-a-grpc-service"></a>ServiceContract 'i gRPC hizmetine dönüştürme

WCF `Get` yöntemi iki parametre alır: `Guid traderId` ve `int portfolioId`. gRPC hizmeti yöntemleri yalnızca tek bir parametre alabilir, bu nedenle iki değeri tutacak bir ileti oluşturmanız gerekir. Bu istek nesnelerini, ardından sonek `Request`ile aynı ada sahip olacak şekilde adlandırmak yaygın bir uygulamadır. `string`, `traderId` alanı için `Guid`yerine kullanılıyor.

Hizmet yalnızca `Portfolio` bir ileti döndürebilir, ancak bu, gelecekte geriye dönük uyumluluğu etkileyebilir. Bir hizmette bulunan her yöntem için ayrı `Request` ve `Response` iletileri tanımlamak iyi bir uygulamadır. Bu, çoğu durumda aynı anda aynı olsa bile. Bu nedenle, tek bir `Portfolio` alanla `GetResponse` bir ileti bildirin.

Bu örnek, `GetRequest` iletisiyle gRPC hizmeti yönteminin bildirimini gösterir:

```protobuf
message GetRequest {
    string trader_id = 1;
    int32 portfolio_id = 2;
}

message GetResponse {
    Portfolio portfolio = 1;
}

service Portfolios {
    rpc Get(GetRequest) returns (GetResponse);
}
```

WCF `GetAll` yöntemi, `traderId`yalnızca tek bir parametre alır, bu yüzden parametre türü olarak `string` belirtebileceğiniz görünebilir. Ancak gRPC tanımlı bir ileti türü gerektirir. Bu gereksinim, ileri doğru uyumluluk için tüm girişler ve çıkışlar için özel iletiler kullanma uygulamasını zorlamaya yardımcı olur.

WCF Yöntemi ayrıca bir `List<Portfolio>`döndürür, ancak basit parametre türlerine izin vermediği için gRPC, dönüş türü olarak `repeated Portfolio` izin vermez. Bunun yerine, listeyi kaydırmak için bir `GetAllResponse` türü oluşturun.

> [!WARNING]
> `PortfolioList` bir ileti veya benzer bir durum oluşturabilir ve bunu birden çok hizmet yöntemi genelinde kullanabilirsiniz, ancak bu geçici yeniden ölçeklendirmelisiniz. Bir hizmette çeşitli yöntemlerin nasıl gelişeceğimizi bilmek olanaksızdır, bu sayede iletilerini özel ve düzgün bir şekilde ayrılmış olarak tutun.

```protobuf
message GetAllRequest {
    string trader_id = 1;
}

message PortfolioList {
    repeated Portfolio portfolios = 1;
}

service Portfolios {
    rpc Get(GetRequest) returns (Portfolio);
    rpc GetAll(GetAllRequest) returns (GetAllResponse);
}
```

Projenizi bu değişikliklerle kaydederseniz, gRPC derleme hedefi arka planda çalışır ve tüm prototip ileti türlerini ve hizmeti uygulamak için kalıtımla oluşturabileceğiniz bir temel sınıfı oluşturur.

`Services/GreeterService.cs` sınıfını açın ve örnek kodu silin. Artık portföy hizmeti uygulamasını ekleyebilirsiniz. Oluşturulan temel sınıf `Protos` ad alanında olur ve iç içe geçmiş bir sınıf olarak oluşturulur. gRPC, `.proto` dosyasında hizmetle aynı ada sahip bir statik sınıf ve bu statik sınıfın içinde `Base` sonekine sahip bir temel sınıf oluşturur. bu nedenle, temel tür için tam tanımlayıcı `TraderSys.Portfolios.Protos.Portfolios.PortfoliosBase`olur.

```csharp
namespace TraderSys.Portfolios.Services
{
    public class PortfolioService : Protos.Portfolios.PortfoliosBase
    {
    }
}
```

Temel sınıf, hizmeti uygulamak için geçersiz kılınabilen `Get` ve `GetAll` için `virtual` Yöntemler bildirir. Yöntemler, `abstract` yerine `virtual`. bu sayede, hizmet, normal C# kodda bir `NotImplementedException` oluşturabilmeniz gibi açık bir grpc `Unimplemented` durum kodu döndürebilir.

ASP.NET Core içindeki tüm gRPC birli hizmet yöntemleri için imza tutarlıdır. İki parametre vardır: ilki `.proto` dosyasında bildirildiği ileti türüdür ve ikincisi ASP.NET Core `HttpContext` benzer şekilde çalışır `ServerCallContext`. Aslında, temel `HttpContext`almak için kullanabileceğiniz `ServerCallContext` sınıfında `GetHttpContext` adlı bir genişletme yöntemi vardır, ancak bunu sık kullanmanıza gerek kalmaz. Bu bölümde daha sonra `ServerCallContext` ve ayrıca, kimlik doğrulamasını ele alan bölümde de bir göz atalım.

Yöntemin dönüş türü bir `Task<T>`, burada `T` yanıt iletisi türüdür. Tüm gRPC hizmet yöntemleri zaman uyumsuzdur.

## <a name="migrate-the-portfoliodata-library-to-net-core"></a>PortfolioData kitaplığını .NET Core 'a geçirme

Bu noktada, projenin, WCF çözümünde `TraderSys.PortfolioData` sınıf kitaplığında bulunan portföy deposuna ve modellerine ihtiyacı vardır. Bunu yapmanın en kolay yolu, sınıf kitaplığı (.NET Standard) şablonuyla Visual Studio **Yeni proje** iletişim kutusunu kullanarak ya da .NET Core CLI kullanarak komut satırından, `TraderSys.sln` dosyasını içeren dizinden bu komutları çalıştırarak yeni bir sınıf kitaplığı oluşturmaktır:

```dotnetcli
dotnet new classlib -o src/TraderSys.PortfolioData
dotnet sln add src/TraderSys.PortfolioData
```

Kitaplığı oluşturup çözüme ekledikten sonra, oluşturulan `Class1.cs` dosyasını silin ve dosyaları WCF çözümünün kitaplığından yeni sınıf kitaplığının klasörüne kopyalayın ve klasör yapısını saklayın:

```
Models
  Portfolio.cs
  PortfolioItem.cs
IPortfolioRepository.cs
PortfolioRepository.cs
```

SDK stili .NET projeleri otomatik olarak kendi dizinine veya altına `.cs` dosyaları dahil eder, bu nedenle bunları projeye açıkça eklemeniz gerekmez. Kalan tek adım, `DataContract` ve `DataMember` özniteliklerinin `Portfolio` ve `PortfolioItem` sınıflarının salt eski C# sınıflar olması için kaldırmasıdır:

```csharp
public class Portfolio
{
    public int Id { get; set; }
    public Guid TraderId { get; set; }
    public List<PortfolioItem> Items { get; set; }
}

public class PortfolioItem
{
    public int Id { get; set; }
    public int ShareId { get; set; }
    public int Holding { get; set; }
    public decimal Cost { get; set; }
}
```

## <a name="use-aspnet-core-dependency-injection"></a>ASP.NET Core bağımlılığı ekleme kullanma

Artık gRPC uygulama projesine bu kitaplığa bir başvuru ekleyebilir ve gRPC hizmeti uygulamasına bağımlılık ekleme işlemini kullanarak `PortfolioRepository` sınıfını kullanabilirsiniz. WCF uygulamasında, bağımlılık ekleme Autofac IoC kapsayıcısı tarafından sağlandı. ASP.NET Core içinde bağımlılık ekleme ' ye sahiptir. Depoyu `Startup` sınıfındaki `ConfigureServices` yöntemine kaydedebilirsiniz:

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        // Register the repository class as a scoped service (instance per request)
        services.AddScoped<IPortfolioRepository, PortfolioRepository>();

        services.AddGrpc();
    }

    // ...
}
```

`IPortfolioRepository` uygulama artık aşağıdaki gibi `PortfolioService` sınıfında bir oluşturucu parametresi olarak belirtilebilir:

```csharp
public class PortfolioService : Protos.Portfolios.PortfoliosBase
{
    private readonly IPortfolioRepository _repository;

    public PortfolioService(IPortfolioRepository repository)
    {
        _repository = repository;
    }
}
```

## <a name="implement-the-grpc-service"></a>GRPC hizmetini uygulama

İletilerinizi ve hizmetinizi `portfolios.proto` dosyasında bildirdiğine göre, hizmet yöntemlerini gRPC tarafından oluşturulan `Portfolios.PortfoliosBase` sınıfından devralan `PortfolioService` sınıfında uygulamanız gerekir. Yöntemler, temel sınıfta `virtual` olarak bildirilmiştir. Bunları geçersiz kılamazsanız, varsayılan olarak bir gRPC "uygulanmamış" durum kodu döndürür.

`Get` yöntemi uygulayarak başlayın. Varsayılan geçersiz kılma Şu örneğe benzer şekilde görünür:

```csharp
public override Task<GetResponse> Get(GetRequest request, ServerCallContext context)
{
    return base.Get(request, context);
}
```

İlk sorun `request.TraderId` bir dizedir ve hizmet bir `Guid`gerektirir. Dize için beklenen biçim `UUID`olsa da, kod çağıranın geçersiz bir değer gönderdiği ve uygun şekilde yanıt verdiği olasılığa karşı uğraşmak zorunda olur. Hizmet, `RpcException` oluşturarak ve sorunu ifade etmek için standart `InvalidArgument` durum kodunu kullanarak hatalarla yanıt verebilir:

```csharp
public override Task<GetResponse> Get(GetRequest request, ServerCallContext context)
{
    if (!Guid.TryParse(request.TraderId, out var traderId))
    {
        throw new RpcException(new Status(StatusCode.InvalidArgument, "traderId must be a UUID"));
    }

    return base.Get(request, context);
}
```

`traderId`için uygun bir `Guid` değeri olduktan sonra, portföyü almak ve istemciye döndürmek için depoyu kullanabilirsiniz:

```csharp
    var response = new GetResponse
    {
        Portfolio = await _repository.GetAsync(request.TraderId, request.PortfolioId)
    };
```

### <a name="map-internal-models-to-grpc-messages"></a>İç modelleri gRPC iletilerine eşleme

Depo kendi POCO model `Portfolio`döndürürken, ancak gRPC 'nin kendi prototipli ileti `Portfolio`ihtiyacı olduğundan, önceki kod aslında çalışmaz. Entity Framework türlerini veri aktarım türleriyle eşleştirdiğinizde, en iyi çözüm iki arasında dönüştürme sağlamaktır. Bu dönüştürme için kodu yerleştirmek için iyi bir yer, `partial` sınıf olarak belirtilen Prototipsiz oluşturulan sınıfta yer alabilir ve bu nedenle Genişletilebilir:

```csharp
namespace TraderSys.Portfolios.Protos
{
    public partial class PortfolioItem
    {
        public static PortfolioItem FromRepositoryModel(PortfolioData.Models.PortfolioItem source)
        {
            if (source is null) return null;

            return new PortfolioItem
            {
                Id = source.Id,
                ShareId = source.ShareId,
                Holding = source.Holding,
                CostCents = (int)(source.Cost * 100)
            };
        }
    }

    public partial class Portfolio
    {
        public static Portfolio FromRepositoryModel(PortfolioData.Models.Portfolio source)
        {
            if (source is null) return null;

            var target = new Portfolio
            {
                Id = source.Id,
                TraderId = source.TraderId.ToString(),
            };

            target.Items.AddRange(source.Items.Select(PortfolioItem.FromRepositoryModel));

            return target;
        }
    }
}
```

> [!NOTE]
> `string`/`Guid` veya `decimal`/`double` ve liste eşlemesi gibi alt düzey tür dönüştürmelerini yapılandırdığınız sürece, iç model sınıflarından bu dönüştürmeyi prototip türlerine işlemek için [Automaber](https://automapper.org/) gibi bir kitaplık kullanabilirsiniz.

Artık dönüştürme koduna sahip olduğunuza göre `Get` yöntemi uygulamasını tamamlayabilirsiniz:

```csharp
public override async Task<GetResponse> Get(GetRequest request, ServerCallContext context)
{
    if (!Guid.TryParse(request.TraderId, out var traderId))
    {
        throw new RpcException(new Status(StatusCode.InvalidArgument, "traderId must be a UUID"));
    }

    var portfolio = await _repository.GetAsync(traderId, request.PortfolioId);

    return new GetResponse
    {
        Portfolio = Portfolio.FromRepositoryModel(portfolio)
    };
}

```

`GetAll` yönteminin uygulanması benzerdir. Prototipteki iletilerde `repeated` alanlarının `RepeatedField<T>`türünde `readonly` özellikler olarak oluşturulduğunu unutmayın, bu nedenle, bu örnekte olduğu gibi `AddRange` yöntemini kullanarak bunlara öğe eklemeniz gerekir:

```csharp
public override async Task<GetAllResponse> GetAll(GetAllRequest request, ServerCallContext context)
{
    if (!Guid.TryParse(request.TraderId, out var traderId))
    {
        throw new RpcException(new Status(StatusCode.InvalidArgument, "traderId must be a UUID"));
    }

    var portfolios = await _repository.GetAllAsync(traderId);

    var response = new GetAllResponse();
    response.Portfolios.AddRange(portfolios.Select(Portfolio.FromRepositoryModel));

    return response;
}
```

WCF istek-yanıt hizmetini başarıyla gRPC 'ye geçirdiyseniz, `.proto` dosyasından bir istemci oluşturmaya bakalım.

## <a name="generate-client-code"></a>İstemci kodu oluştur

İstemcisini içermesi için aynı çözümde bir .NET Standard sınıf kitaplığı oluşturun. Bu öncelikle istemci kodu oluşturma örneğidir, ancak bu tür bir kitaplığı NuGet kullanarak paketleyebilir ve diğer .NET ekibinin kullanması için bir iç depoya dağıtabilirsiniz. Devam edin ve çözüme `TraderSys.Portfolios.Client` adlı yeni bir .NET Standard sınıf kitaplığı ekleyin ve `Class1.cs` dosyasını silin.

> [!CAUTION]
> [GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet paketi .net Core 3,0 (veya başka bir .NET Standard 2,1 uyumlu çalışma zamanı) gerektirir. .NET Framework ve .NET Core 'un önceki sürümleri [GRPC. Core](https://www.nuget.org/packages/Grpc.Core) NuGet paketi tarafından desteklenir.

Visual Studio 2019 ' de, Visual Studio 'nun önceki sürümlerindeki WCF projelerine hizmet başvuruları ekleme yöntemine benzer bir şekilde gRPC hizmetlerine başvurular ekleyebilirsiniz. Hizmet başvuruları ve bağlı hizmetler artık aynı kullanıcı arabirimi altında yönetilir. Çözüm Gezgini ' de `TraderSys.Portfolios.Client` projesindeki **Bağımlılıklar** düğümüne sağ tıklayıp **bağlı hizmet ekle**' yi seçerek kullanıcı arabirimine erişebilirsiniz. Görüntülenen araç penceresinde, **hizmet başvuruları** bölümünü seçin ve ardından **Yeni GRPC hizmet başvurusu Ekle**' yi seçin:

![Visual Studio 2019 'de bağlı hizmetler kullanıcı arabirimi](media/migrate-request-reply/add-connected-service.png)

`TraderSys.Portfolios` projesindeki `portfolios.proto` dosyasına gidin, **istemciye** , **oluşturulacak sınıf türünü seçin**altında bırakın ve **Tamam**' ı seçin:

![Visual Studio 2019 ' de yeni gRPC hizmet başvurusu Ekle iletişim kutusu](media/migrate-request-reply/add-new-grpc-service-reference.png)

> [!TIP]
> Bu iletişim kutusunun Ayrıca bir URL alanı sağladığını unutmayın. Kuruluşunuz, `.proto` dosyaları için Web erişimli bir dizin koruyorsa, bu URL adresini ayarlayarak istemcileri oluşturabilirsiniz.

Visual Studio **bağlı hizmet ekle** özelliğini kullandığınızda `portfolios.proto` dosyası, kopyalamak yerine *bağlantılı bir dosya* olarak sınıf kitaplığı projesine eklenir, bu nedenle hizmet projesindeki dosyada yapılan değişiklikler istemci projesinde otomatik olarak uygulanır. `csproj` dosyasındaki `<Protobuf>` öğesi şöyle görünür:

```xml
<Protobuf Include="..\TraderSys.Portfolios\Protos\portfolios.proto" GrpcServices="Client">
  <Link>Protos\portfolios.proto</Link>
</Protobuf>
```

> [!TIP]
> Visual Studio 'Yu kullanmıyorsanız veya komut satırından çalışmayı tercih ediyorsanız, bir .NET gRPC projesindeki prototipsel başvuruları yönetmek için `dotnet-grpc` genel aracını kullanabilirsiniz. Daha fazla bilgi için [`dotnet-grpc` belgelerine](/aspnet/core/grpc/dotnet-grpc)bakın.

### <a name="use-the-portfolios-service-from-a-client-application"></a>Bir istemci uygulamasından portföyde hizmetini kullanma

Aşağıdaki kod, bir konsol uygulamasında oluşturulan istemciyi nasıl kullanacağınızı gösteren kısa bir örnektir. GRPC istemci kodunun daha ayrıntılı bir araştırması, bu bölümün sonunda bulunur.

```csharp
public class Program
{
    public async Task Main(string[] args)
    {
        GetResponse response;

        using (var channel = GrpcChannel.ForAddress("https://localhost:5001"))
        {
            var client = new Protos.Portfolios.PortfoliosClient(channel);

            response = await client.GetAsync(new GetRequest
            {
                TraderId = args[0],
                PortfolioId = int.Parse(args[1])
            });
        }

        foreach (var item in response.Portfolio.Items)
        {
            Console.WriteLine($"Holding {item.Holding} of Share ID {item.ShareId}.");
        }
    }
}
```

Artık temel bir WCF uygulamasını ASP.NET Core gRPC hizmetine geçirdiniz ve hizmeti bir .NET uygulamasından tüketmek için bir istemci oluşturdunuz. Sonraki bölümde, ilgili çift yönlü hizmetler ele alınacaktır.

>[!div class="step-by-step"]
>[Önceki](create-project.md)
>[İleri](migrate-duplex-services.md)
