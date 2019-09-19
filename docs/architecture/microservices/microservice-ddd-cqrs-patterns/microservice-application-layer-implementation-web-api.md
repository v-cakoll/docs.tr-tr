---
title: Web API’si kullanarak mikro hizmet uygulama katmanını uygulama
description: Kapsayıcılı .NET uygulamaları için .NET mikro hizmetleri mimarisi | Bağımlılık ekleme ve ortalama düzenlerini ve bunların uygulama ayrıntılarını Web API 'SI uygulama katmanında anlayın.
ms.date: 10/08/2018
ms.openlocfilehash: c8447cfcd3155a873d61ee9287f58774392c279d
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "70296755"
---
# <a name="implement-the-microservice-application-layer-using-the-web-api"></a><span data-ttu-id="06011-103">Web API 'sini kullanarak mikro hizmet uygulama katmanını uygulama</span><span class="sxs-lookup"><span data-stu-id="06011-103">Implement the microservice application layer using the Web API</span></span>

## <a name="use-dependency-injection-to-inject-infrastructure-objects-into-your-application-layer"></a><span data-ttu-id="06011-104">Uygulama katmanınıza altyapı nesneleri eklemek için bağımlılık ekleme 'yi kullanma</span><span class="sxs-lookup"><span data-stu-id="06011-104">Use Dependency Injection to inject infrastructure objects into your application layer</span></span>

<span data-ttu-id="06011-105">Daha önce belirtildiği gibi, uygulama katmanı, oluşturduğunuz yapıtın (derleme) bir parçası olarak (örneğin, bir Web API projesi veya bir MVC web uygulaması projesi) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="06011-105">As mentioned previously, the application layer can be implemented as part of the artifact (assembly) you are building, such as within a Web API project or an MVC web app project.</span></span> <span data-ttu-id="06011-106">ASP.NET Core ile oluşturulmuş bir mikro hizmet söz konusu olduğunda, uygulama katmanı genellikle Web API kitaplığınız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="06011-106">In the case of a microservice built with ASP.NET Core, the application layer will usually be your Web API library.</span></span> <span data-ttu-id="06011-107">Özel uygulama katmanı kodunuzda ASP.NET Core (altyapı ve Denetleyicilerinizden) nelerin geldiğini ayırmak istiyorsanız, uygulama katmanınızı ayrı bir sınıf kitaplığına yerleştirebilirsiniz, ancak bu isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="06011-107">If you want to separate what is coming from ASP.NET Core (its infrastructure plus your controllers) from your custom application layer code, you could also place your application layer in a separate class library, but that is optional.</span></span>

<span data-ttu-id="06011-108">Örneğin, sıralama mikro hizmetinin uygulama katmanı kodu, Şekil 7-23 ' de gösterildiği gibi doğrudan **sıralama. API** projesinin (bir ASP.NET Core Web API Projesi) bir parçası olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="06011-108">For instance, the application layer code of the ordering microservice is directly implemented as part of the **Ordering.API** project (an ASP.NET Core Web API project), as shown in Figure 7-23.</span></span>

![Sıralama. API mikro hizmeti 'nin, uygulama klasörü altındaki alt klasörleri gösteren Çözüm Gezgini görünümü: Davranışlar, komutlar, DomainEventHandlers, tümleştirme olayları, modeller, sorgular ve doğrulamalar.](./media/image20.png)

<span data-ttu-id="06011-110">**Şekil 7-23**.</span><span class="sxs-lookup"><span data-stu-id="06011-110">**Figure 7-23**.</span></span> <span data-ttu-id="06011-111">Sıralama. API ASP.NET Core Web API projesindeki uygulama katmanı</span><span class="sxs-lookup"><span data-stu-id="06011-111">The application layer in the Ordering.API ASP.NET Core Web API project</span></span>

<span data-ttu-id="06011-112">ASP.NET Core, varsayılan olarak Oluşturucu ekleme işlemini destekleyen basit bir [yerleşik IOC kapsayıcısı](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) (IServiceProvider arabirimi tarafından temsil edilir) içerir ve ASP.net, belırlı Hizmetleri dı üzerinden kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="06011-112">ASP.NET Core includes a simple [built-in IoC container](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) (represented by the IServiceProvider interface) that supports constructor injection by default, and ASP.NET makes certain services available through DI.</span></span> <span data-ttu-id="06011-113">ASP.NET Core, YAZMAÇ aracılığıyla eklenecek olan herhangi bir türden herhangi biri için *hizmet* koşulları 'nı kullanır.</span><span class="sxs-lookup"><span data-stu-id="06011-113">ASP.NET Core uses the term *service* for any of the types you register that will be injected through DI.</span></span> <span data-ttu-id="06011-114">Yerleşik kapsayıcının hizmetlerini uygulamanızın başlangıç sınıfındaki ConfigureServices yönteminde yapılandırırsınız.</span><span class="sxs-lookup"><span data-stu-id="06011-114">You configure the built-in container's services in the ConfigureServices method in your application's Startup class.</span></span> <span data-ttu-id="06011-115">Bağımlılıklarınız, bir tür için gereken ve IOC kapsayıcısına kaydolmanızı sağlayan hizmetlerde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="06011-115">Your dependencies are implemented in the services that a type needs and that you register in the IoC container.</span></span>

<span data-ttu-id="06011-116">Genellikle, altyapı nesneleri uygulayan bağımlılıklar eklemek istersiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-116">Typically, you want to inject dependencies that implement infrastructure objects.</span></span> <span data-ttu-id="06011-117">Ekleme için çok tipik bir bağımlılık, bir depodur.</span><span class="sxs-lookup"><span data-stu-id="06011-117">A very typical dependency to inject is a repository.</span></span> <span data-ttu-id="06011-118">Ancak sahip olduğunuz herhangi bir altyapı bağımlılığı ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-118">But you could inject any other infrastructure dependency that you may have.</span></span> <span data-ttu-id="06011-119">Daha basit uygulamalar için, DBContext aynı zamanda altyapı Kalıcılık nesnelerinizin uygulanması olduğundan, çalışma birimi nesnesi (EF DbContext nesnesi) birimini doğrudan ekleyebiliriniz.</span><span class="sxs-lookup"><span data-stu-id="06011-119">For simpler implementations, you could directly inject your Unit of Work pattern object (the EF DbContext object), because the DBContext is also the implementation of your infrastructure persistence objects.</span></span>

<span data-ttu-id="06011-120">Aşağıdaki örnekte, .NET Core 'un Oluşturucu aracılığıyla gerekli depo nesnelerini nasıl ekleme görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="06011-120">In the following example, you can see how .NET Core is injecting the required repository objects through the constructor.</span></span> <span data-ttu-id="06011-121">Sınıfı, sonraki bölümde ele alacak bir komut işleyicisidir.</span><span class="sxs-lookup"><span data-stu-id="06011-121">The class is a command handler, which we will cover in the next section.</span></span>

```csharp
public class CreateOrderCommandHandler
    : IAsyncRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;
    private readonly IIdentityService _identityService;
    private readonly IMediator _mediator;

    // Using DI to inject infrastructure persistence Repositories
    public CreateOrderCommandHandler(IMediator mediator,
                                     IOrderRepository orderRepository,
                                     IIdentityService identityService)
    {
        _orderRepository = orderRepository ??
                          throw new ArgumentNullException(nameof(orderRepository));
        _identityService = identityService ??
                          throw new ArgumentNullException(nameof(identityService));
        _mediator = mediator ??
                                 throw new ArgumentNullException(nameof(mediator));
    }

    public async Task<bool> Handle(CreateOrderCommand message)
    {
        // Create the Order AggregateRoot
        // Add child entities and value objects through the Order aggregate root
        // methods and constructor so validations, invariants, and business logic
        // make sure that consistency is preserved across the whole aggregate
        var address = new Address(message.Street, message.City, message.State,
                                  message.Country, message.ZipCode);
        var order = new Order(message.UserId, address, message.CardTypeId,
                              message.CardNumber, message.CardSecurityNumber,
                              message.CardHolderName, message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice,
                               item.Discount, item.PictureUrl, item.Units);
        }

        _orderRepository.Add(order);

        return await _orderRepository.UnitOfWork
            .SaveEntitiesAsync();
    }
}
```

<span data-ttu-id="06011-122">Sınıfı, işlemi yürütmek ve durum değişikliklerini kalıcı hale getirmek için eklenen depoları kullanır.</span><span class="sxs-lookup"><span data-stu-id="06011-122">The class uses the injected repositories to execute the transaction and persist the state changes.</span></span> <span data-ttu-id="06011-123">Bu sınıfın bir komut işleyicisi, ASP.NET Core Web API denetleyicisi yöntemi veya [ddd uygulama hizmeti](https://lostechies.com/jimmybogard/2008/08/21/services-in-domain-driven-design/)olup olmadığı konusunda bir önemi yoktur.</span><span class="sxs-lookup"><span data-stu-id="06011-123">It does not matter whether that class is a command handler, an ASP.NET Core Web API controller method, or a [DDD Application Service](https://lostechies.com/jimmybogard/2008/08/21/services-in-domain-driven-design/).</span></span> <span data-ttu-id="06011-124">Sonuç olarak, bir komut işleyicisine benzer şekilde depoları, etki alanı varlıklarını ve diğer uygulama koordinasyonunu kullanan basit bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="06011-124">It is ultimately a simple class that uses repositories, domain entities, and other application coordination in a fashion similar to a command handler.</span></span> <span data-ttu-id="06011-125">Bağımlılık ekleme, kurucuya göre dı kullanan örnekte olduğu gibi, belirtilen tüm sınıflar için aynı şekilde çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="06011-125">Dependency Injection works the same way for all the mentioned classes, as in the example using DI based on the constructor.</span></span>

### <a name="register-the-dependency-implementation-types-and-interfaces-or-abstractions"></a><span data-ttu-id="06011-126">Bağımlılık uygulama türlerini ve arabirimlerini veya soyutlamaları kaydetme</span><span class="sxs-lookup"><span data-stu-id="06011-126">Register the dependency implementation types and interfaces or abstractions</span></span>

<span data-ttu-id="06011-127">Oluşturucular aracılığıyla eklenen nesneleri kullanmadan önce, uygulama sınıflarınıza eklenen nesneleri oluşturan arabirimlerin ve sınıfların, DI aracılığıyla ne şekilde kaydedeceğinizi bilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="06011-127">Before you use the objects injected through constructors, you need to know where to register the interfaces and classes that produce the objects injected into your application classes through DI.</span></span> <span data-ttu-id="06011-128">(Daha önce gösterildiği gibi, oluşturucuyu temel alan dı gibi.)</span><span class="sxs-lookup"><span data-stu-id="06011-128">(Like DI based on the constructor, as shown previously.)</span></span>

#### <a name="use-the-built-in-ioc-container-provided-by-aspnet-core"></a><span data-ttu-id="06011-129">ASP.NET Core tarafından sunulan yerleşik IOC kapsayıcısını kullanın</span><span class="sxs-lookup"><span data-stu-id="06011-129">Use the built-in IoC container provided by ASP.NET Core</span></span>

<span data-ttu-id="06011-130">ASP.NET Core tarafından sağlanmış yerleşik IOC kapsayıcısını kullandığınızda, Startup.cs dosyasındaki ConfigureServices yöntemine eklemek istediğiniz türleri aşağıdaki kodda olduğu gibi kaydedersiniz:</span><span class="sxs-lookup"><span data-stu-id="06011-130">When you use the built-in IoC container provided by ASP.NET Core, you register the types you want to inject in the ConfigureServices method in the Startup.cs file, as in the following code:</span></span>

```csharp
// Registration of types into ASP.NET Core built-in container
public void ConfigureServices(IServiceCollection services)
{
    // Register out-of-the-box framework services.
    services.AddDbContext<CatalogContext>(c =>
    {
        c.UseSqlServer(Configuration["ConnectionString"]);
    },
    ServiceLifetime.Scoped
    );
    services.AddMvc();
    // Register custom application dependencies.
    services.AddScoped<IMyCustomRepository, MyCustomSQLRepository>();
}
```

<span data-ttu-id="06011-131">Bir IOC kapsayıcısına türlerin kaydedilmesinde en sık kullanılan desenler, bir arabirim ve ilgili uygulama sınıfı olan bir çift türü kaydetmesidir.</span><span class="sxs-lookup"><span data-stu-id="06011-131">The most common pattern when registering types in an IoC container is to register a pair of types—an interface and its related implementation class.</span></span> <span data-ttu-id="06011-132">Ardından, herhangi bir Oluşturucu aracılığıyla IOC kapsayıcısından bir nesne istediğinizde, belirli bir arabirim türünün bir nesnesini istemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="06011-132">Then when you request an object from the IoC container through any constructor, you request an object of a certain type of interface.</span></span> <span data-ttu-id="06011-133">Örneğin, önceki örnekte, kurucularınızın herhangi birinin ımıcustomrepository (arabirim veya soyutlama) üzerinde bir bağımlılığı olduğunda, IOC kapsayıcısı MyCustomSQLServerRepository uygulamasının bir örneğini ekleyecektir sınıfı.</span><span class="sxs-lookup"><span data-stu-id="06011-133">For instance, in the previous example, the last line states that when any of your constructors have a dependency on IMyCustomRepository (interface or abstraction), the IoC container will inject an instance of the MyCustomSQLServerRepository implementation class.</span></span>

#### <a name="use-the-scrutor-library-for-automatic-types-registration"></a><span data-ttu-id="06011-134">Otomatik türler kaydı için Itilen kitaplığı kullanın</span><span class="sxs-lookup"><span data-stu-id="06011-134">Use the Scrutor library for automatic types registration</span></span>

<span data-ttu-id="06011-135">.NET Core 'da dı kullanırken, bir derlemeyi tarayabilmesi ve türlerini kurala göre otomatik olarak kaydetmenizi isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-135">When using DI in .NET Core, you might want to be able to scan an assembly and automatically register its types by convention.</span></span> <span data-ttu-id="06011-136">Bu özellik şu anda ASP.NET Core ' de kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="06011-136">This feature is not currently available in ASP.NET Core.</span></span> <span data-ttu-id="06011-137">Ancak, bunun için [itilen](https://github.com/khellang/Scrutor) kitaplığı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-137">However, you can use the [Scrutor](https://github.com/khellang/Scrutor) library for that.</span></span> <span data-ttu-id="06011-138">Bu yaklaşım, IOC kapsayıcısına kaydedilmesi gereken düzinelerce türlerinizin olması durumunda kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="06011-138">This approach is convenient when you have dozens of types that need to be registered in your IoC container.</span></span>

#### <a name="additional-resources"></a><span data-ttu-id="06011-139">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="06011-139">Additional resources</span></span>

- <span data-ttu-id="06011-140">**Matthew King. Hizmetleri Itilen kayıt yaptırın** </span><span class="sxs-lookup"><span data-stu-id="06011-140">**Matthew King. Registering services with Scrutor** </span></span>\
  <https://www.mking.net/blog/registering-services-with-scrutor>

- <span data-ttu-id="06011-141">**Bir Hellang. İtilen.**</span><span class="sxs-lookup"><span data-stu-id="06011-141">**Kristian Hellang. Scrutor.**</span></span> <span data-ttu-id="06011-142">GitHub deposu.</span><span class="sxs-lookup"><span data-stu-id="06011-142">GitHub repo.</span></span> \
  <https://github.com/khellang/Scrutor>

#### <a name="use-autofac-as-an-ioc-container"></a><span data-ttu-id="06011-143">IoC kapsayıcısı olarak Autofac kullanma</span><span class="sxs-lookup"><span data-stu-id="06011-143">Use Autofac as an IoC container</span></span>

<span data-ttu-id="06011-144">Ayrıca, ek IOC kapsayıcıları kullanabilir ve bunları, [Autofac](https://autofac.org/)kullanan eShopOnContainers 'daki sıralama mikro hizmetinde olduğu gibi ASP.NET Core işlem hattına ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-144">You can also use additional IoC containers and plug them into the ASP.NET Core pipeline, as in the ordering microservice in eShopOnContainers, which uses [Autofac](https://autofac.org/).</span></span> <span data-ttu-id="06011-145">Autofac kullanırken, genellikle türleri modüller aracılığıyla kaydedersiniz; bu sayede, birden çok sınıf kitaplığı genelinde dağıtılmış uygulama türlerine sahip olduğu gibi, kayıt türlerini türlerinizin bulunduğu yere bağlı olarak birden çok Dosya arasında bölmeniz sağlanır.</span><span class="sxs-lookup"><span data-stu-id="06011-145">When using Autofac you typically register the types via modules, which allow you to split the registration types between multiple files depending on where your types are, just as you could have the application types distributed across multiple class libraries.</span></span>

<span data-ttu-id="06011-146">Örneğin, aşağıda [sıralama. API Web API 'si](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.API) projesi için eklemek istediğiniz türleri içeren [Autofac uygulama modülü](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Infrastructure/AutofacModules/ApplicationModule.cs) verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="06011-146">For example, the following is the [Autofac application module](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Infrastructure/AutofacModules/ApplicationModule.cs) for the [Ordering.API Web API](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.API) project with the types you will want to inject.</span></span>

```csharp
public class ApplicationModule : Autofac.Module
{
    public string QueriesConnectionString { get; }
    public ApplicationModule(string qconstr)
    {
        QueriesConnectionString = qconstr;
    }

    protected override void Load(ContainerBuilder builder)
    {
        builder.Register(c => new OrderQueries(QueriesConnectionString))
            .As<IOrderQueries>()
            .InstancePerLifetimeScope();
        builder.RegisterType<BuyerRepository>()
            .As<IBuyerRepository>()
            .InstancePerLifetimeScope();
        builder.RegisterType<OrderRepository>()
            .As<IOrderRepository>()
            .InstancePerLifetimeScope();
        builder.RegisterType<RequestManager>()
            .As<IRequestManager>()
            .InstancePerLifetimeScope();
   }
}
```

<span data-ttu-id="06011-147">Autofac Ayrıca [derlemeleri tarama ve türleri ad kurallarına göre kaydetme](https://autofac.readthedocs.io/en/latest/register/scanning.html)özelliği de vardır.</span><span class="sxs-lookup"><span data-stu-id="06011-147">Autofac also has a feature to [scan assemblies and register types by name conventions](https://autofac.readthedocs.io/en/latest/register/scanning.html).</span></span>

<span data-ttu-id="06011-148">Kayıt işlemi ve kavramlar, yerleşik ASP.NET Core IOC kapsayıcısına sahip türleri kaydetme yöntemine çok benzer, ancak Autofac kullanırken sözdizimi biraz farklıdır.</span><span class="sxs-lookup"><span data-stu-id="06011-148">The registration process and concepts are very similar to the way you can register types with the built-in ASP.NET Core IoC container, but the syntax when using Autofac is a bit different.</span></span>

<span data-ttu-id="06011-149">Örnek kodda, soyutlama ıorderrepository, OrderRepository uygulama sınıfı ile birlikte kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="06011-149">In the example code, the abstraction IOrderRepository is registered along with the implementation class OrderRepository.</span></span> <span data-ttu-id="06011-150">Bu, bir oluşturucunun ıorderrepository soyutlama veya arabirimi aracılığıyla bağımlılık bildirdiğinde, IOC kapsayıcısının OrderRepository sınıfının bir örneğini ekleymesidir.</span><span class="sxs-lookup"><span data-stu-id="06011-150">This means that whenever a constructor is declaring a dependency through the IOrderRepository abstraction or interface, the IoC container will inject an instance of the OrderRepository class.</span></span>

<span data-ttu-id="06011-151">Örnek kapsamı türü, bir örneğin aynı hizmet veya bağımlılık istekleri arasında nasıl paylaşılacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="06011-151">The instance scope type determines how an instance is shared between requests for the same service or dependency.</span></span> <span data-ttu-id="06011-152">Bağımlılık için bir istek yapıldığında, IOC kapsayıcısı aşağıdakileri döndürebilir:</span><span class="sxs-lookup"><span data-stu-id="06011-152">When a request is made for a dependency, the IoC container can return the following:</span></span>

- <span data-ttu-id="06011-153">Yaşam süresi kapsamı başına tek örnekli (ASP.NET Core IOC kapsayıcısında *kapsam*olarak adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="06011-153">A single instance per lifetime scope (referred to in the ASP.NET Core IoC container as *scoped*).</span></span>

- <span data-ttu-id="06011-154">Bağımlılık başına yeni bir örnek (ASP.NET Core IOC kapsayıcısında *geçici*olarak adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="06011-154">A new instance per dependency (referred to in the ASP.NET Core IoC container as *transient*).</span></span>

- <span data-ttu-id="06011-155">IFC kapsayıcısını kullanan tüm nesneler genelinde paylaşılan tek bir örnek (ASP.NET Core IOC kapsayıcısında *Singleton*olarak adlandırılır).</span><span class="sxs-lookup"><span data-stu-id="06011-155">A single instance shared across all objects using the IoC container (referred to in the ASP.NET Core IoC container as *singleton*).</span></span>

#### <a name="additional-resources"></a><span data-ttu-id="06011-156">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="06011-156">Additional resources</span></span>

- <span data-ttu-id="06011-157">**ASP.NET Core bağımlılık eklenmesine giriş** </span><span class="sxs-lookup"><span data-stu-id="06011-157">**Introduction to Dependency Injection in ASP.NET Core** </span></span>\
  [https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection](/aspnet/core/fundamentals/dependency-injection)

- <span data-ttu-id="06011-158">**Autofac.**</span><span class="sxs-lookup"><span data-stu-id="06011-158">**Autofac.**</span></span> <span data-ttu-id="06011-159">Resmi belgeler.</span><span class="sxs-lookup"><span data-stu-id="06011-159">Official documentation.</span></span> \
  <https://docs.autofac.org/en/latest/>

- <span data-ttu-id="06011-160">**Autofac IoC kapsayıcı örneği kapsamları ile ASP.NET Core IOC kapsayıcı hizmeti yaşam sürelerini karşılaştırma-Cesar de La Torre.**</span><span class="sxs-lookup"><span data-stu-id="06011-160">**Comparing ASP.NET Core IoC container service lifetimes with Autofac IoC container instance scopes - Cesar de la Torre.**</span></span> \
  <https://devblogs.microsoft.com/cesardelatorre/comparing-asp-net-core-ioc-service-life-times-and-autofac-ioc-instance-scopes/>

## <a name="implement-the-command-and-command-handler-patterns"></a><span data-ttu-id="06011-161">Komut ve komut Işleyici desenlerini uygulama</span><span class="sxs-lookup"><span data-stu-id="06011-161">Implement the Command and Command Handler patterns</span></span>

<span data-ttu-id="06011-162">Önceki bölümde gösterilen dı---Oluşturucu örneğinde, IOC kapsayıcısı bir sınıftaki Oluşturucu aracılığıyla ekleme depolandı.</span><span class="sxs-lookup"><span data-stu-id="06011-162">In the DI-through-constructor example shown in the previous section, the IoC container was injecting repositories through a constructor in a class.</span></span> <span data-ttu-id="06011-163">Ancak tam olarak nereye eklenen?</span><span class="sxs-lookup"><span data-stu-id="06011-163">But exactly where were they injected?</span></span> <span data-ttu-id="06011-164">Basit bir Web API 'sinde (örneğin, eShopOnContainers 'daki Katalog mikro hizmeti), ASP.NET Core istek ardışık düzeninin bir parçası olarak, bunları bir denetleyici oluşturucusunda MVC denetleyicileri ' düzeyinde eklersiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-164">In a simple Web API (for example, the catalog microservice in eShopOnContainers), you inject them at the MVC controllers’ level, in a controller constructor, as part of the request pipeline of ASP.NET Core.</span></span> <span data-ttu-id="06011-165">Ancak, bu bölümün ilk kodunda (eShopOnContainers 'daki sıralama. API hizmetinden [Createordercommandhandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) sınıfı), bağımlılık ekleme işlemi, belirli bir komut işleyicisinin Oluşturucusu aracılığıyla yapılır.</span><span class="sxs-lookup"><span data-stu-id="06011-165">However, in the initial code of this section (the [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) class from the Ordering.API service in eShopOnContainers), the injection of dependencies is done through the constructor of a particular command handler.</span></span> <span data-ttu-id="06011-166">Bir komut işleyicisinin ne olduğunu ve neden kullanmak istediğinizi açıklamıza izin verin.</span><span class="sxs-lookup"><span data-stu-id="06011-166">Let us explain what a command handler is and why you would want to use it.</span></span>

<span data-ttu-id="06011-167">Komut deseninin, bu kılavuzda daha önce sunulan CQRS düzeniyle ilgili doğası gereği vardır.</span><span class="sxs-lookup"><span data-stu-id="06011-167">The Command pattern is intrinsically related to the CQRS pattern that was introduced earlier in this guide.</span></span> <span data-ttu-id="06011-168">CQRS 'nin iki yüzü vardır.</span><span class="sxs-lookup"><span data-stu-id="06011-168">CQRS has two sides.</span></span> <span data-ttu-id="06011-169">İlk alan, daha önce açıklanan [paber](https://github.com/StackExchange/dapper-dot-net) mikro ORM ile basitleştirilmiş sorguları kullanarak sorgular.</span><span class="sxs-lookup"><span data-stu-id="06011-169">The first area is queries, using simplified queries with the [Dapper](https://github.com/StackExchange/dapper-dot-net) micro ORM, which was explained previously.</span></span> <span data-ttu-id="06011-170">İkinci alan, işlemler için başlangıç noktası ve hizmet dışından giriş kanalı olan komutlardır.</span><span class="sxs-lookup"><span data-stu-id="06011-170">The second area is commands, which are the starting point for transactions, and the input channel from outside the service.</span></span>

<span data-ttu-id="06011-171">Şekil 7-24 ' de gösterildiği gibi, model, istemci tarafındaki komutları kabul etmeyi, etki alanı modeli kurallarına göre işlemeyi ve son olarak işlemler ile durumları kalıcı hale getirmeyi temel alır.</span><span class="sxs-lookup"><span data-stu-id="06011-171">As shown in Figure 7-24, the pattern is based on accepting commands from the client side, processing them based on the domain model rules, and finally persisting the states with transactions.</span></span>

![CQRS içindeki yazma tarafı için üst düzey görünüm: UI uygulaması, API aracılığıyla bir komut gönderir. Bu, etki alanı modeline ve veritabanını güncelleştirme altyapısına bağlıdır.](./media/image21.png)

<span data-ttu-id="06011-173">**Şekil 7-24**.</span><span class="sxs-lookup"><span data-stu-id="06011-173">**Figure 7-24**.</span></span> <span data-ttu-id="06011-174">Bir CQRS deseninin içindeki komutların veya "işlem tarafındaki" üst düzey görünümü</span><span class="sxs-lookup"><span data-stu-id="06011-174">High-level view of the commands or “transactional side” in a CQRS pattern</span></span>

### <a name="the-command-class"></a><span data-ttu-id="06011-175">Komut sınıfı</span><span class="sxs-lookup"><span data-stu-id="06011-175">The command class</span></span>

<span data-ttu-id="06011-176">Komut, sistemin sistem durumunu değiştiren bir eylem gerçekleştirmesi için bir isteğidir.</span><span class="sxs-lookup"><span data-stu-id="06011-176">A command is a request for the system to perform an action that changes the state of the system.</span></span> <span data-ttu-id="06011-177">Komutları zorunludur ve yalnızca bir kez işlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="06011-177">Commands are imperative, and should be processed just once.</span></span>

<span data-ttu-id="06011-178">Komutlar imperatives olduğundan, genellikle kesinlik (örneğin, "Oluştur" veya "Güncelleştir") bir fiil ile adlandırılır ve CreateOrderCommand gibi toplam türü içerebilir.</span><span class="sxs-lookup"><span data-stu-id="06011-178">Since commands are imperatives, they are typically named with a verb in the imperative mood (for example, "create" or "update"), and they might include the aggregate type, such as CreateOrderCommand.</span></span> <span data-ttu-id="06011-179">Bir olaydan farklı olarak, bir komut geçmişte bir olgu değildir; yalnızca bir istekte bulunur ve bu nedenle bu durum reddedilebilir.</span><span class="sxs-lookup"><span data-stu-id="06011-179">Unlike an event, a command is not a fact from the past; it is only a request, and thus may be refused.</span></span>

<span data-ttu-id="06011-180">Komutlar, bir kullanıcının isteği başlatan bir sonucu olarak veya işlem yöneticisi bir eylemi gerçekleştirmek için bir toplama işlemi yaparken bir işlem yöneticisi 'nden kaynaklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="06011-180">Commands can originate from the UI as a result of a user initiating a request, or from a process manager when the process manager is directing an aggregate to perform an action.</span></span>

<span data-ttu-id="06011-181">Bir komutun önemli bir özelliği tek bir alıcı tarafından yalnızca bir kez işlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="06011-181">An important characteristic of a command is that it should be processed just once by a single receiver.</span></span> <span data-ttu-id="06011-182">Bunun nedeni, bir komutun uygulamada gerçekleştirmek istediğiniz tek bir eylem veya işlemdir.</span><span class="sxs-lookup"><span data-stu-id="06011-182">This is because a command is a single action or transaction you want to perform in the application.</span></span> <span data-ttu-id="06011-183">Örneğin, aynı sıra oluşturma komutu birden çok kez işlenmemelidir.</span><span class="sxs-lookup"><span data-stu-id="06011-183">For example, the same order creation command should not be processed more than once.</span></span> <span data-ttu-id="06011-184">Bu, komutlar ve olaylar arasındaki önemli bir farktır.</span><span class="sxs-lookup"><span data-stu-id="06011-184">This is an important difference between commands and events.</span></span> <span data-ttu-id="06011-185">Olaylar birden çok kez işlenebilir, çünkü birçok sistem veya mikro hizmet olayla ilgileniyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="06011-185">Events may be processed multiple times, because many systems or microservices might be interested in the event.</span></span>

<span data-ttu-id="06011-186">Ayrıca, komutun ıdempotent olmadığı durumlarda bir komutun yalnızca bir kez işlenmesi önemlidir.</span><span class="sxs-lookup"><span data-stu-id="06011-186">In addition, it is important that a command be processed only once in case the command is not idempotent.</span></span> <span data-ttu-id="06011-187">Komutun doğası veya sistemin komutu işleme biçimi nedeniyle, sonucu değiştirmeden birden çok kez yürütülebileceğini bir komut ıdempotent olur.</span><span class="sxs-lookup"><span data-stu-id="06011-187">A command is idempotent if it can be executed multiple times without changing the result, either because of the nature of the command, or because of the way the system handles the command.</span></span>

<span data-ttu-id="06011-188">Bu, kullanıcılarınızın iş kuralları ve ınvaryanslarınızın altındayken komutlarınızın ve güncelleştirmelerin ıdempotent hale gelmesini iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="06011-188">It is a good practice to make your commands and updates idempotent when it makes sense under your domain’s business rules and invariants.</span></span> <span data-ttu-id="06011-189">Örneğin, her nedenden dolayı aynı örneği kullanmak için (yeniden deneme mantığı, hacimme vb.), aynı CreateOrder komutu sisteminize birden çok kez ulaşırsa, bunu tanımlayabilir ve birden çok sipariş oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="06011-189">For instance, to use the same example, if for any reason (retry logic, hacking, etc.) the same CreateOrder command reaches your system multiple times, you should be able to identify it and ensure that you do not create multiple orders.</span></span> <span data-ttu-id="06011-190">Bunu yapmak için, işlemlere bir tür kimlik eklemeniz ve komutun veya güncelleştirmenin zaten işlenip işlenmediğini belirlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="06011-190">To do so, you need to attach some kind of identity in the operations and identify whether the command or update was already processed.</span></span>

<span data-ttu-id="06011-191">Tek bir alıcıya komut gönderirsiniz; bir komut yayımlamayın.</span><span class="sxs-lookup"><span data-stu-id="06011-191">You send a command to a single receiver; you do not publish a command.</span></span> <span data-ttu-id="06011-192">Yayımlama, bir şeyin meydana geldiğini ve Olay alıcıları için ilginç olabileceğini gösteren olaylar içindir.</span><span class="sxs-lookup"><span data-stu-id="06011-192">Publishing is for events that state a fact—that something has happened and might be interesting for event receivers.</span></span> <span data-ttu-id="06011-193">Olaylar söz konusu olduğunda, yayımcı hangi alıcıların olay veya ne yaptığını öğrenmek için bir kaygıya sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="06011-193">In the case of events, the publisher has no concerns about which receivers get the event or what they do it.</span></span> <span data-ttu-id="06011-194">Ancak, etki alanı veya tümleştirme olayları önceki bölümlerde zaten tanıtılan farklı bir hikayeye sahiptir.</span><span class="sxs-lookup"><span data-stu-id="06011-194">But domain or integration events are a different story already introduced in previous sections.</span></span>

<span data-ttu-id="06011-195">Bir komut, bu komutu yürütmek için gereken tüm bilgileri içeren veri alanları veya Koleksiyonlar içeren bir sınıf ile uygulanır.</span><span class="sxs-lookup"><span data-stu-id="06011-195">A command is implemented with a class that contains data fields or collections with all the information that is needed in order to execute that command.</span></span> <span data-ttu-id="06011-196">Bir komut, özellikle değişiklik veya işlem istemek için kullanılan özel bir Veri Aktarımı nesne türüdür (DTO).</span><span class="sxs-lookup"><span data-stu-id="06011-196">A command is a special kind of Data Transfer Object (DTO), one that is specifically used to request changes or transactions.</span></span> <span data-ttu-id="06011-197">Komutun kendisi, komutu işlemek için gereken bilgileri ve başka hiçbir şeyi temel alır.</span><span class="sxs-lookup"><span data-stu-id="06011-197">The command itself is based on exactly the information that is needed for processing the command, and nothing more.</span></span>

<span data-ttu-id="06011-198">Aşağıdaki örnek basitleştirilmiş CreateOrderCommand sınıfını gösterir.</span><span class="sxs-lookup"><span data-stu-id="06011-198">The following example shows the simplified CreateOrderCommand class.</span></span> <span data-ttu-id="06011-199">Bu, eShopOnContainers 'daki sıralama mikro hizmetinde kullanılan sabit bir komuttur.</span><span class="sxs-lookup"><span data-stu-id="06011-199">This is an immutable command that is used in the ordering microservice in eShopOnContainers.</span></span>

```csharp
// DDD and CQRS patterns comment
// Note that we recommend that you implement immutable commands
// In this case, immutability is achieved by having all the setters as private
// plus being able to update the data just once, when creating the object
// through the constructor.
// References on immutable commands:
// http://cqrs.nu/Faq
// https://docs.spine3.org/motivation/immutability.html
// http://blog.gauffin.org/2012/06/griffin-container-introducing-command-support/
// https://msdn.microsoft.com/library/bb383979.aspx
[DataContract]
public class CreateOrderCommand
    :IAsyncRequest<bool>
{
    [DataMember]
    private readonly List<OrderItemDTO> _orderItems;
    [DataMember]
    public string City { get; private set; }
    [DataMember]
    public string Street { get; private set; }
    [DataMember]
    public string State { get; private set; }
    [DataMember]
    public string Country { get; private set; }
    [DataMember]
    public string ZipCode { get; private set; }
    [DataMember]
    public string CardNumber { get; private set; }
    [DataMember]
    public string CardHolderName { get; private set; }
    [DataMember]
    public DateTime CardExpiration { get; private set; }
    [DataMember]
    public string CardSecurityNumber { get; private set; }
    [DataMember]
    public int CardTypeId { get; private set; }
    [DataMember]
    public IEnumerable<OrderItemDTO> OrderItems => _orderItems;

    public CreateOrderCommand()
    {
        _orderItems = new List<OrderItemDTO>();
    }

    public CreateOrderCommand(List<BasketItem> basketItems, string city,
        string street,
        string state, string country, string zipcode,
        string cardNumber, string cardHolderName, DateTime cardExpiration,
        string cardSecurityNumber, int cardTypeId) : this()
    {
        _orderItems = MapToOrderItems(basketItems);
        City = city;
        Street = street;
        State = state;
        Country = country;
        ZipCode = zipcode;
        CardNumber = cardNumber;
        CardHolderName = cardHolderName;
        CardSecurityNumber = cardSecurityNumber;
        CardTypeId = cardTypeId;
        CardExpiration = cardExpiration;
    }

    public class OrderItemDTO
    {
        public int ProductId { get; set; }
        public string ProductName { get; set; }
        public decimal UnitPrice { get; set; }
        public decimal Discount { get; set; }
        public int Units { get; set; }
        public string PictureUrl { get; set; }
    }
}
```

<span data-ttu-id="06011-200">Temel olarak, komut sınıfı, etki alanı model nesnelerini kullanarak bir iş işlemi gerçekleştirmek için gereken tüm verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="06011-200">Basically, the command class contains all the data you need for performing a business transaction by using the domain model objects.</span></span> <span data-ttu-id="06011-201">Bu nedenle, komutlar yalnızca salt okunurdur verileri içeren veri yapılarıdır ve hiçbir davranış yoktur.</span><span class="sxs-lookup"><span data-stu-id="06011-201">Thus, commands are simply data structures that contain read-only data, and no behavior.</span></span> <span data-ttu-id="06011-202">Komutun adı, amacını gösterir.</span><span class="sxs-lookup"><span data-stu-id="06011-202">The command’s name indicates its purpose.</span></span> <span data-ttu-id="06011-203">Gibi C#birçok dilde komutlar sınıflar olarak temsil edilir ancak gerçek nesne yönelimli anlamda doğru sınıflar değildir.</span><span class="sxs-lookup"><span data-stu-id="06011-203">In many languages like C#, commands are represented as classes, but they are not true classes in the real object-oriented sense.</span></span>

<span data-ttu-id="06011-204">Ek bir özellik olarak, beklenen kullanım doğrudan etki alanı modeli tarafından işlendiklerinden, komutlar sabittir.</span><span class="sxs-lookup"><span data-stu-id="06011-204">As an additional characteristic, commands are immutable, because the expected usage is that they are processed directly by the domain model.</span></span> <span data-ttu-id="06011-205">Bunların öngörülen yaşam süresi boyunca değiştirmeleri gerekmez.</span><span class="sxs-lookup"><span data-stu-id="06011-205">They do not need to change during their projected lifetime.</span></span> <span data-ttu-id="06011-206">Bir sınıfta C# , bir ayarlayıcıya ya da iç durumu değiştiren başka yöntemler yoksa, dengeshlik kullanılabilirliği elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="06011-206">In a C# class, immutability can be achieved by not having any setters or other methods that change internal state.</span></span>

<span data-ttu-id="06011-207">Komutları bir serileştirme/seri durumdan çıkarma sürecinde işlem yapmanız veya beklemeniz durumunda, özelliklerin özel ayarlayıcı ve `[DataMember]` (veya `[JsonProperty]`) özniteliği olması gerektiğini unutmayın, aksi takdirde seri hale getirici bunu yapamaz hedef konumda nesneyi gerekli değerlerle yeniden yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="06011-207">Bear in mind that if you intend or expect commands will be going through a serializing/deserializing process, the properties must have private setter, and the `[DataMember]` (or `[JsonProperty]`) attribute, otherwise the deserializer will not be able to reconstruct the object at destination with the required values.</span></span>

<span data-ttu-id="06011-208">Örneğin, sipariş oluşturmak için komut sınıfı büyük olasılıkla veri bakımından oluşturmak istediğiniz sıraya benzer, ancak büyük olasılıkla aynı özniteliklere gerek kalmaz.</span><span class="sxs-lookup"><span data-stu-id="06011-208">For example, the command class for creating an order is probably similar in terms of data to the order you want to create, but you probably do not need the same attributes.</span></span> <span data-ttu-id="06011-209">Örneğin, sıra henüz oluşturulmadığından CreateOrderCommand 'in bir sıra KIMLIĞI yoktur.</span><span class="sxs-lookup"><span data-stu-id="06011-209">For instance, CreateOrderCommand does not have an order ID, because the order has not been created yet.</span></span>

<span data-ttu-id="06011-210">Birçok komut sınıfı basit olabilir ve değiştirilmesi gereken bazı durumları hakkında yalnızca birkaç alan gerektirir.</span><span class="sxs-lookup"><span data-stu-id="06011-210">Many command classes can be simple, requiring only a few fields about some state that needs to be changed.</span></span> <span data-ttu-id="06011-211">Bu durum, aşağıdakine benzer bir komut kullanarak bir siparişin durumunu "işlem sürüyor" iken "ücretli" veya "sevk edildi" olarak değiştirmeniz durumunda olur:</span><span class="sxs-lookup"><span data-stu-id="06011-211">That would be the case if you are just changing the status of an order from “in process” to “paid” or “shipped” by using a command similar to the following:</span></span>

```csharp
[DataContract]
public class UpdateOrderStatusCommand
    :IAsyncRequest<bool>
{
    [DataMember]
    public string Status { get; private set; }

    [DataMember]
    public string OrderId { get; private set; }

    [DataMember]
    public string BuyerIdentityGuid { get; private set; }
}
```

<span data-ttu-id="06011-212">Bazı geliştiriciler, Kullanıcı arabirimi istek nesnelerini kendi komut DTOs dışında, ancak bu yalnızca tercihlerden ayrı hale getirir.</span><span class="sxs-lookup"><span data-stu-id="06011-212">Some developers make their UI request objects separate from their command DTOs, but that is just a matter of preference.</span></span> <span data-ttu-id="06011-213">Bu, çok fazla eklenen değer olmadan sıkıcı bir ayırdır ve nesneler neredeyse tam olarak aynı şekildir.</span><span class="sxs-lookup"><span data-stu-id="06011-213">It is a tedious separation with not much added value, and the objects are almost exactly the same shape.</span></span> <span data-ttu-id="06011-214">Örneğin, eShopOnContainers 'da, bazı komutlar doğrudan istemci tarafından gelir.</span><span class="sxs-lookup"><span data-stu-id="06011-214">For instance, in eShopOnContainers, some commands come directly from the client side.</span></span>

### <a name="the-command-handler-class"></a><span data-ttu-id="06011-215">Komut Işleyici sınıfı</span><span class="sxs-lookup"><span data-stu-id="06011-215">The Command Handler class</span></span>

<span data-ttu-id="06011-216">Her komut için belirli bir komut işleyici sınıfı uygulamalısınız.</span><span class="sxs-lookup"><span data-stu-id="06011-216">You should implement a specific command handler class for each command.</span></span> <span data-ttu-id="06011-217">Bu, modelin nasıl çalıştığı ve komut nesnesini, etki alanı nesnelerini ve altyapı deposu nesnelerini kullanacağınız yerdir.</span><span class="sxs-lookup"><span data-stu-id="06011-217">That is how the pattern works, and it is where you will use the command object, the domain objects, and the infrastructure repository objects.</span></span> <span data-ttu-id="06011-218">Komut işleyici, CQRS ve DDD bakımından uygulama katmanının temelini bulgidir.</span><span class="sxs-lookup"><span data-stu-id="06011-218">The command handler is in fact the heart of the application layer in terms of CQRS and DDD.</span></span> <span data-ttu-id="06011-219">Ancak, tüm etki alanı mantığının, toplama kökleri (kök varlıklar), alt varlıklar veya [etki alanı Hizmetleri](https://lostechies.com/jimmybogard/2008/08/21/services-in-domain-driven-design/)içindeki etki alanı sınıfları içinde olması gerekir, ancak uygulama katmanından bir sınıf olan komut işleyicisinde yer almalıdır.</span><span class="sxs-lookup"><span data-stu-id="06011-219">However, all the domain logic should be contained within the domain classes—within the aggregate roots (root entities), child entities, or [domain services](https://lostechies.com/jimmybogard/2008/08/21/services-in-domain-driven-design/), but not within the command handler, which is a class from the application layer.</span></span>

<span data-ttu-id="06011-220">Komut işleyici sınıfı, önceki bölümde bahsedilen tek sorumluluk Ilkesine (SRP) ulaşmak için bir güçlü atlama pulu sunar.</span><span class="sxs-lookup"><span data-stu-id="06011-220">The command handler class offers a strong stepping stone in the way to achieve the Single Responsibility Principle (SRP) mentioned in a previous section.</span></span>

<span data-ttu-id="06011-221">Komut işleyici bir komut alır ve kullanılan toplamanın bir sonucunu alır.</span><span class="sxs-lookup"><span data-stu-id="06011-221">A command handler receives a command and obtains a result from the aggregate that is used.</span></span> <span data-ttu-id="06011-222">Sonuç, komutun başarılı bir şekilde yürütülmesi ya da bir özel durum olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="06011-222">The result should be either successful execution of the command, or an exception.</span></span> <span data-ttu-id="06011-223">Özel durum söz konusu olduğunda sistem durumu değişmeden olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="06011-223">In the case of an exception, the system state should be unchanged.</span></span>

<span data-ttu-id="06011-224">Komut işleyici genellikle aşağıdaki adımları gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="06011-224">The command handler usually takes the following steps:</span></span>

- <span data-ttu-id="06011-225">Bir DTO ( [Mediator](https://en.wikipedia.org/wiki/Mediator_pattern) veya diğer altyapı nesnesinden) gibi komut nesnesini alır.</span><span class="sxs-lookup"><span data-stu-id="06011-225">It receives the command object, like a DTO (from the [mediator](https://en.wikipedia.org/wiki/Mediator_pattern) or other infrastructure object).</span></span>

- <span data-ttu-id="06011-226">Komutun geçerli olduğunu doğrular (Mediator tarafından doğrulanmamıştır).</span><span class="sxs-lookup"><span data-stu-id="06011-226">It validates that the command is valid (if not validated by the mediator).</span></span>

- <span data-ttu-id="06011-227">Geçerli komutun hedefi olan toplam kök örneğini başlatır.</span><span class="sxs-lookup"><span data-stu-id="06011-227">It instantiates the aggregate root instance that is the target of the current command.</span></span>

- <span data-ttu-id="06011-228">Komutu, komut dosyasından gerekli verileri alarak, toplu kök örneğinde yöntemini yürütür.</span><span class="sxs-lookup"><span data-stu-id="06011-228">It executes the method on the aggregate root instance, getting the required data from the command.</span></span>

- <span data-ttu-id="06011-229">Bu, toplamanın yeni durumunu ilgili veritabanına devam ettirir.</span><span class="sxs-lookup"><span data-stu-id="06011-229">It persists the new state of the aggregate to its related database.</span></span> <span data-ttu-id="06011-230">Bu son işlem, gerçek işlemdir.</span><span class="sxs-lookup"><span data-stu-id="06011-230">This last operation is the actual transaction.</span></span>

<span data-ttu-id="06011-231">Genellikle, bir komut işleyicisi toplam köküne (kök varlık) göre tek bir toplama ile ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="06011-231">Typically, a command handler deals with a single aggregate driven by its aggregate root (root entity).</span></span> <span data-ttu-id="06011-232">Birden çok toplamalar tek bir komutun alınması tarafından etkileniyorsa, birden fazla toplama arasında durumları veya eylemleri yaymak için etki alanı olaylarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-232">If multiple aggregates should be impacted by the reception of a single command, you could use domain events to propagate states or actions across multiple aggregates.</span></span>

<span data-ttu-id="06011-233">Buradaki önemli nokta, bir komut işlendiğinde, tüm etki alanı mantığının etki alanı modelinde (toplamalar), tam olarak kapsüllenmiş ve birim testi için hazır olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="06011-233">The important point here is that when a command is being processed, all the domain logic should be inside the domain model (the aggregates), fully encapsulated and ready for unit testing.</span></span> <span data-ttu-id="06011-234">Komut işleyici, model değiştirildiğinde altyapı katmanının (depolar) değişiklikleri kalıcı hale getirebileceği konusunda bilgi almak için, yalnızca veritabanından etki alanı modelini almanın ve son adımın bir yolu olarak davranır.</span><span class="sxs-lookup"><span data-stu-id="06011-234">The command handler just acts as a way to get the domain model from the database, and as the final step, to tell the infrastructure layer (repositories) to persist the changes when the model is changed.</span></span> <span data-ttu-id="06011-235">Bu yaklaşımın avantajı, etki alanı mantığını bir yalıtılmış, tamamen Kapsüllenen, zengin, davranış etki alanı modelinde, uygulama veya altyapı katmanlarındaki kodu değiştirmeden, tesisat seviyesi (komut işleyicileri, Web API, depolar vb.).</span><span class="sxs-lookup"><span data-stu-id="06011-235">The advantage of this approach is that you can refactor the domain logic in an isolated, fully encapsulated, rich, behavioral domain model without changing code in the application or infrastructure layers, which are the plumbing level (command handlers, Web API, repositories, etc.).</span></span>

<span data-ttu-id="06011-236">Komut işleyicileri karmaşıktır, çok fazla Logic ile, bu bir kod kokusu olabilir.</span><span class="sxs-lookup"><span data-stu-id="06011-236">When command handlers get complex, with too much logic, that can be a code smell.</span></span> <span data-ttu-id="06011-237">Bunları gözden geçirin ve etki alanı mantığını bulursanız, bu etki alanı davranışını etki alanı nesnelerinin yöntemlerine (Toplam kök ve alt varlık) taşımak için kodu yeniden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="06011-237">Review them, and if you find domain logic, refactor the code to move that domain behavior to the methods of the domain objects (the aggregate root and child entity).</span></span>

<span data-ttu-id="06011-238">Bir komut işleyici sınıfına örnek olarak, aşağıdaki kod, bu bölümün başlangıcında gördüğünüz CreateOrderCommandHandler sınıfını gösterir.</span><span class="sxs-lookup"><span data-stu-id="06011-238">As an example of a command handler class, the following code shows the same CreateOrderCommandHandler class that you saw at the beginning of this chapter.</span></span> <span data-ttu-id="06011-239">Bu durumda, tanıtıcı yöntemini ve etki alanı modeli nesneleri/toplamaları ile işlemleri vurgulamak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="06011-239">In this case, we want to highlight the Handle method and the operations with the domain model objects/aggregates.</span></span>

```csharp
public class CreateOrderCommandHandler
    : IAsyncRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;
    private readonly IIdentityService _identityService;
    private readonly IMediator _mediator;

    // Using DI to inject infrastructure persistence Repositories
    public CreateOrderCommandHandler(IMediator mediator,
                                     IOrderRepository orderRepository,
                                     IIdentityService identityService)
    {
        _orderRepository = orderRepository ??
                          throw new ArgumentNullException(nameof(orderRepository));
        _identityService = identityService ??
                          throw new ArgumentNullException(nameof(identityService));
        _mediator = mediator ??
                                 throw new ArgumentNullException(nameof(mediator));
    }

    public async Task<bool> Handle(CreateOrderCommand message)
    {
        // Create the Order AggregateRoot
        // Add child entities and value objects through the Order aggregate root
        // methods and constructor so validations, invariants, and business logic
        // make sure that consistency is preserved across the whole aggregate
        var address = new Address(message.Street, message.City, message.State,
                                  message.Country, message.ZipCode);
        var order = new Order(message.UserId, address, message.CardTypeId,
                              message.CardNumber, message.CardSecurityNumber,
                              message.CardHolderName, message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice,
                               item.Discount, item.PictureUrl, item.Units);
        }

        _orderRepository.Add(order);

        return await _orderRepository.UnitOfWork
            .SaveEntitiesAsync();
    }
}
```

<span data-ttu-id="06011-240">Bunlar bir komut işleyicisinin gerçekleşmesi gereken ek adımlardır:</span><span class="sxs-lookup"><span data-stu-id="06011-240">These are additional steps a command handler should take:</span></span>

- <span data-ttu-id="06011-241">Komut verilerini kullanarak toplam kökünün Yöntem ve davranışıyla birlikte işlem yapın.</span><span class="sxs-lookup"><span data-stu-id="06011-241">Use the command’s data to operate with the aggregate root’s methods and behavior.</span></span>

- <span data-ttu-id="06011-242">Etki alanı nesneleri içinde dahili olarak, işlem yürütüldüğü sırada etki alanı olaylarını yükseltir, ancak bu, bir komut işleyici noktasından saydamdır.</span><span class="sxs-lookup"><span data-stu-id="06011-242">Internally within the domain objects, raise domain events while the transaction is executed, but that is transparent from a command handler point of view.</span></span>

- <span data-ttu-id="06011-243">Toplamanın işlem sonucu başarılı olursa ve işlem tamamlandıktan sonra, tümleştirme olaylarını yükseltin.</span><span class="sxs-lookup"><span data-stu-id="06011-243">If the aggregate’s operation result is successful and after the transaction is finished, raise integration events.</span></span> <span data-ttu-id="06011-244">(Bunlar ayrıca depolar gibi altyapı sınıfları tarafından da oluşturulabilir.)</span><span class="sxs-lookup"><span data-stu-id="06011-244">(These might also be raised by infrastructure classes like repositories.)</span></span>

#### <a name="additional-resources"></a><span data-ttu-id="06011-245">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="06011-245">Additional resources</span></span>

- <span data-ttu-id="06011-246">**Seemann ' i işaretleyin. Sınırlar üzerinde uygulamalar nesne yönelimli değildir** </span><span class="sxs-lookup"><span data-stu-id="06011-246">**Mark Seemann. At the Boundaries, Applications are Not Object-Oriented** </span></span>\
  <https://blog.ploeh.dk/2011/05/31/AttheBoundaries,ApplicationsareNotObject-Oriented/>

- <span data-ttu-id="06011-247">**Komutlar ve olaylar** </span><span class="sxs-lookup"><span data-stu-id="06011-247">**Commands and events** </span></span>\
  <http://cqrs.nu/Faq/commands-and-events>

- <span data-ttu-id="06011-248">**Komut işleyici ne yapar?**</span><span class="sxs-lookup"><span data-stu-id="06011-248">**What does a command handler do?**</span></span> \
  <http://cqrs.nu/Faq/command-handlers>

- <span data-ttu-id="06011-249">**Jimmy Bogard. Etki alanı komut desenleri – Işleyiciler** </span><span class="sxs-lookup"><span data-stu-id="06011-249">**Jimmy Bogard. Domain Command Patterns – Handlers** </span></span>\
  <https://jimmybogard.com/domain-command-patterns-handlers/>

- <span data-ttu-id="06011-250">**Jimmy Bogard. Etki alanı komut desenleri – doğrulama** </span><span class="sxs-lookup"><span data-stu-id="06011-250">**Jimmy Bogard. Domain Command Patterns – Validation** </span></span>\
  <https://jimmybogard.com/domain-command-patterns-validation/>

## <a name="the-command-process-pipeline-how-to-trigger-a-command-handler"></a><span data-ttu-id="06011-251">Komut işlemi ardışık düzeni: komut işleyicisini tetikleme</span><span class="sxs-lookup"><span data-stu-id="06011-251">The Command process pipeline: how to trigger a command handler</span></span>

<span data-ttu-id="06011-252">Sonraki soru, bir komut işleyicisini çağırma.</span><span class="sxs-lookup"><span data-stu-id="06011-252">The next question is how to invoke a command handler.</span></span> <span data-ttu-id="06011-253">Her ilgili ASP.NET Core denetleyicisinden el ile çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-253">You could manually call it from each related ASP.NET Core controller.</span></span> <span data-ttu-id="06011-254">Ancak, bu yaklaşım çok fazla bağlanmış ve ideal değildir.</span><span class="sxs-lookup"><span data-stu-id="06011-254">However, that approach would be too coupled and is not ideal.</span></span>

<span data-ttu-id="06011-255">Önerilen Seçenekler olan diğer iki ana seçenek şunlardır:</span><span class="sxs-lookup"><span data-stu-id="06011-255">The other two main options, which are the recommended options, are:</span></span>

- <span data-ttu-id="06011-256">Bellek içi bir Mediator model yapıtı.</span><span class="sxs-lookup"><span data-stu-id="06011-256">Through an in-memory Mediator pattern artifact.</span></span>

- <span data-ttu-id="06011-257">, Denetleyiciler ve işleyiciler arasında zaman uyumsuz bir ileti kuyruğu ile.</span><span class="sxs-lookup"><span data-stu-id="06011-257">With an asynchronous message queue, in between controllers and handlers.</span></span>

### <a name="use-the-mediator-pattern-in-memory-in-the-command-pipeline"></a><span data-ttu-id="06011-258">Komut ardışık düzeninde Mediator düzenini (bellek içi) kullanın</span><span class="sxs-lookup"><span data-stu-id="06011-258">Use the Mediator pattern (in-memory) in the command pipeline</span></span>

<span data-ttu-id="06011-259">Şekil 7-25 ' de gösterildiği gibi, bir CQRS yaklaşımında, bir bellek içi veri yoluna benzer şekilde akıllı bir Mediator kullanın. Bu, bir veya daha fazla alma işlemi için doğru komut işleyicisine yönlendirilmeye yetecek kadar akıllıdır.</span><span class="sxs-lookup"><span data-stu-id="06011-259">As shown in Figure 7-25, in a CQRS approach you use an intelligent mediator, similar to an in-memory bus, which is smart enough to redirect to the right command handler based on the type of the command or DTO being received.</span></span> <span data-ttu-id="06011-260">Bileşenler arasındaki tek siyah oklar, ilgili etkileşimlerine sahip nesneler arasındaki bağımlılıkları temsil eder (yani, dı üzerinden eklenen).</span><span class="sxs-lookup"><span data-stu-id="06011-260">The single black arrows between components represent the dependencies between objects (in many cases, injected through DI) with their related interactions.</span></span>

![Önceki görüntüden yakınlaştırma: ASP.NET Core denetleyicisi komutu MediatR 'nin komut işlem hattına gönderir, bu nedenle uygun işleyiciye alırlar.](./media/image22.png)

<span data-ttu-id="06011-262">**Şekil 7-25**.</span><span class="sxs-lookup"><span data-stu-id="06011-262">**Figure 7-25**.</span></span> <span data-ttu-id="06011-263">Tek bir CQRS mikro hizmetindeki işlemdeki Mediator modelini kullanma</span><span class="sxs-lookup"><span data-stu-id="06011-263">Using the Mediator pattern in process in a single CQRS microservice</span></span>

<span data-ttu-id="06011-264">Mediator deseninin kullanılması, kurumsal uygulamalarda işleme isteklerinin karmaşık hale gelmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="06011-264">The reason that using the Mediator pattern makes sense is that in enterprise applications, the processing requests can get complicated.</span></span> <span data-ttu-id="06011-265">Günlüğe kaydetme, doğrulama, denetim ve güvenlik gibi çeşitli çapraz kesme sorunları ekleyebilmek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="06011-265">You want to be able to add an open number of cross-cutting concerns like logging, validations, audit, and security.</span></span> <span data-ttu-id="06011-266">Bu durumlarda, bu ek davranışlar veya çapraz kesme sorunları için bir yol sağlamak üzere bir Mediator işlem hattına (bkz. [Mediator düzeni](https://en.wikipedia.org/wiki/Mediator_pattern)) güvenebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-266">In these cases, you can rely on a mediator pipeline (see [Mediator pattern](https://en.wikipedia.org/wiki/Mediator_pattern)) to provide a means for these extra behaviors or cross-cutting concerns.</span></span>

<span data-ttu-id="06011-267">Bir Mediator, bu işlemin "nasıl" olduğunu kapsülleyen bir nesnedir: durumu, bir komut işleyicisinin çağrılması ya da işleyiciye sağladığınız yük olarak, yürütme şeklini temel alarak düzenler.</span><span class="sxs-lookup"><span data-stu-id="06011-267">A mediator is an object that encapsulates the “how” of this process: it coordinates execution based on state, the way a command handler is invoked, or the payload you provide to the handler.</span></span> <span data-ttu-id="06011-268">Bir Mediator bileşeni ile, dekoratörler (veya [mediaTR 3](https://www.nuget.org/packages/MediatR/3.0.0)' den beri işlem [hattı davranışları](https://github.com/jbogard/MediatR/wiki/Behaviors) ) uygulayarak merkezi ve saydam bir şekilde çapraz kesme sorunları uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-268">With a mediator component you can apply cross-cutting concerns in a centralized and transparent way by applying decorators (or [pipeline behaviors](https://github.com/jbogard/MediatR/wiki/Behaviors) since [MediatR 3](https://www.nuget.org/packages/MediatR/3.0.0)).</span></span> <span data-ttu-id="06011-269">Daha fazla bilgi için bkz. [dekoratör deseninin](https://en.wikipedia.org/wiki/Decorator_pattern).</span><span class="sxs-lookup"><span data-stu-id="06011-269">For more information, see the [Decorator pattern](https://en.wikipedia.org/wiki/Decorator_pattern).</span></span>

<span data-ttu-id="06011-270">Dekoratörler ve davranışlar, yalnızca Mediator bileşeni tarafından yönetilen belirli bir işlem ardışık düzenine uygulanan, [en boy Yönelimli Programlamaya (AOP)](https://en.wikipedia.org/wiki/Aspect-oriented_programming)benzerdir.</span><span class="sxs-lookup"><span data-stu-id="06011-270">Decorators and behaviors are similar to [Aspect Oriented Programming (AOP)](https://en.wikipedia.org/wiki/Aspect-oriented_programming), only applied to a specific process pipeline managed by the mediator component.</span></span> <span data-ttu-id="06011-271">Çapraz kesme sorunları uygulayan AOP 'nin yönleri, derleme zamanında eklenen veya nesne çağrı yakalaşmaya bağlı olan *en büyük hava* alanları temelinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="06011-271">Aspects in AOP that implement cross-cutting concerns are applied based on *aspect weavers* injected at compilation time or based on object call interception.</span></span> <span data-ttu-id="06011-272">Tipik AOP yaklaşımları bazen "Magic" gibi çalışarak, ne kadar AOP 'nin çalıştığını görmek çok kolay.</span><span class="sxs-lookup"><span data-stu-id="06011-272">Both typical AOP approaches are sometimes said to work "like magic," because it is not easy to see how AOP does its work.</span></span> <span data-ttu-id="06011-273">Ciddi sorunlar veya hatalarla ilgilenirken, AOP 'nin hata ayıklaması zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="06011-273">When dealing with serious issues or bugs, AOP can be difficult to debug.</span></span> <span data-ttu-id="06011-274">Öte yandan, bu dekoratörler/davranışlar açık ve yalnızca ortalama bağlamında uygulandı, bu nedenle hata ayıklama çok daha öngörülebilir ve kolaydır.</span><span class="sxs-lookup"><span data-stu-id="06011-274">On the other hand, these decorators/behaviors are explicit and applied only in the context of the mediator, so debugging is much more predictable and easy.</span></span>

<span data-ttu-id="06011-275">Örneğin, eShopOnContainers sıralama mikro hizmetinde, iki örnek davranış, bir [LogBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/LoggingBehavior.cs) sınıfı ve [validatorbehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/ValidatorBehavior.cs) sınıfı uyguladık.</span><span class="sxs-lookup"><span data-stu-id="06011-275">For example, in the eShopOnContainers ordering microservice, we implemented two sample behaviors, a [LogBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/LoggingBehavior.cs) class and a [ValidatorBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/ValidatorBehavior.cs) class.</span></span> <span data-ttu-id="06011-276">Davranışların uygulanması, eShopOnContainers 'ın [mediaTR 3](https://www.nuget.org/packages/MediatR/3.0.0) [davranışlarını](https://github.com/jbogard/MediatR/wiki/Behaviors)nasıl kullandığını gösteren bir sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="06011-276">The implementation of the behaviors is explained in the next section by showing how eShopOnContainers uses [MediatR 3](https://www.nuget.org/packages/MediatR/3.0.0) [behaviors](https://github.com/jbogard/MediatR/wiki/Behaviors).</span></span>

### <a name="use-message-queues-out-of-proc-in-the-commands-pipeline"></a><span data-ttu-id="06011-277">Komutun ardışık düzeninde ileti kuyruklarını (proc dışı) kullanın</span><span class="sxs-lookup"><span data-stu-id="06011-277">Use message queues (out-of-proc) in the command’s pipeline</span></span>

<span data-ttu-id="06011-278">Şekil 7-26 ' de gösterildiği gibi, aracılar veya ileti kuyrukları temelinde zaman uyumsuz iletileri kullanmak başka bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="06011-278">Another choice is to use asynchronous messages based on brokers or message queues, as shown in Figure 7-26.</span></span> <span data-ttu-id="06011-279">Bu seçenek, komut işleyicisinden önce Mediator bileşeniyle de birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="06011-279">That option could also be combined with the mediator component right before the command handler.</span></span>

![Komutun işlem hattı, komutları uygun işleyiciye teslim etmek için yüksek oranda kullanılabilir bir ileti kuyruğu tarafından da işlenebilir.](./media/image23.png)

<span data-ttu-id="06011-281">**Şekil 7-26**.</span><span class="sxs-lookup"><span data-stu-id="06011-281">**Figure 7-26**.</span></span> <span data-ttu-id="06011-282">CQRS komutlarıyla ileti kuyruklarını (işlem dışı ve işlem arası iletişim) kullanma</span><span class="sxs-lookup"><span data-stu-id="06011-282">Using message queues (out of process and inter-process communication) with CQRS commands</span></span>

<span data-ttu-id="06011-283">Komutları kabul etmek için ileti sıralarının kullanılması, büyük olasılıkla bir işlem hattını dış ileti kuyruğu aracılığıyla bağlı iki işleme bölmeniz gerekeceğinden, komutunuzun işlem hattını daha karmaşıklaştırır.</span><span class="sxs-lookup"><span data-stu-id="06011-283">Using message queues to accept the commands can further complicate your command’s pipeline, because you will probably need to split the pipeline into two processes connected through the external message queue.</span></span> <span data-ttu-id="06011-284">Hala, zaman uyumsuz mesajlaşma temelinde geliştirilmiş ölçeklenebilirlik ve performansa sahip olmanız gerekiyorsa kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="06011-284">Still, it should be used if you need to have improved scalability and performance based on asynchronous messaging.</span></span> <span data-ttu-id="06011-285">Şekil 7-26 olması durumunda denetleyicinin yalnızca komut iletisini sıraya gönderse ve döndürdüğü göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="06011-285">Consider that in the case of Figure 7-26, the controller just posts the command message into the queue and returns.</span></span> <span data-ttu-id="06011-286">Ardından komut işleyicileri iletileri kendi hızlarında işler.</span><span class="sxs-lookup"><span data-stu-id="06011-286">Then the command handlers process the messages at their own pace.</span></span> <span data-ttu-id="06011-287">Kuyrukların harika bir avantajı vardır: ileti kuyruğu, Örneğin hisse senetleri veya yüksek hacimme verileri içeren başka senaryolar gibi, Hyper ölçeklenebilirlik gerektiğinde bir arabellek görevi görebilir.</span><span class="sxs-lookup"><span data-stu-id="06011-287">That is a great benefit of queues: the message queue can act as a buffer in cases when hyper scalability is needed, such as for stocks or any other scenario with a high volume of ingress data.</span></span>

<span data-ttu-id="06011-288">Ancak, ileti sıralarının zaman uyumsuz doğası nedeniyle, komut işleminin başarısı veya başarısızlığı hakkında istemci uygulamayla nasıl iletişim kuracağınızı belirlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="06011-288">However, because of the asynchronous nature of message queues, you need to figure out how to communicate with the client application about the success or failure of the command’s process.</span></span> <span data-ttu-id="06011-289">Kural olarak, "yangın ve unut" komutlarını asla kullanmamalısınız.</span><span class="sxs-lookup"><span data-stu-id="06011-289">As a rule, you should never use “fire and forget” commands.</span></span> <span data-ttu-id="06011-290">Her iş uygulamasının, bir komutun başarıyla işlenip işlenmediğini veya en azından doğrulanıp kabul edildiğini bilmeleri gerekir.</span><span class="sxs-lookup"><span data-stu-id="06011-290">Every business application needs to know if a command was processed successfully, or at least validated and accepted.</span></span>

<span data-ttu-id="06011-291">Bu nedenle, zaman uyumsuz bir kuyruğa gönderilen bir komut iletisi doğrulandıktan sonra istemciye yanıt vermek, işlemi çalıştırdıktan sonra işlemin sonucunu döndüren işlem içi bir komut işlemiyle karşılaştırıldığında sisteminize karmaşıklık ekler.</span><span class="sxs-lookup"><span data-stu-id="06011-291">Thus, being able to respond to the client after validating a command message that was submitted to an asynchronous queue adds complexity to your system, as compared to an in-process command process that returns the operation’s result after running the transaction.</span></span> <span data-ttu-id="06011-292">Kuyrukları kullanarak, sisteminizde ek bileşenler ve özel iletişim gerektiren diğer işlem sonuç iletileri aracılığıyla komut işleminin sonucunu döndürmeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-292">Using queues, you might need to return the result of the command process through other operation result messages, which will require additional components and custom communication in your system.</span></span>

<span data-ttu-id="06011-293">Ayrıca, zaman uyumsuz komutlar tek yönlü bir komutlardır. Bu, bir [çevrimiçi konuşmada](https://groups.google.com/forum/#!msg/dddcqrs/xhJHVxDx2pM/WP9qP8ifYCwJ)Burtsev Alexey ve Greg başak arasında aşağıdaki ilginç alışverişte açıklandığı gibi, birçok durumda gerekli olmayabilir:</span><span class="sxs-lookup"><span data-stu-id="06011-293">Additionally, async commands are one-way commands, which in many cases might not be needed, as is explained in the following interesting exchange between Burtsev Alexey and Greg Young in an [online conversation](https://groups.google.com/forum/#!msg/dddcqrs/xhJHVxDx2pM/WP9qP8ifYCwJ):</span></span>

> <span data-ttu-id="06011-294">\[Burtsev Alexey\] , kişilerin zaman uyumsuz komut işleme veya tek yönlü komut mesajlaşmasını herhangi bir nedenle kullandıkları çok sayıda kod buldum (uzun bir işlem yapılmazlar, bunlar harici zaman uyumsuz kod yürütülemese de, bu uygulamalar bile çapraz uygulama değildir ileti veri yolu kullanımı sınırı).</span><span class="sxs-lookup"><span data-stu-id="06011-294">\[Burtsev Alexey\] I find lots of code where people use async command handling or one way command messaging without any reason to do so (they are not doing some long operation, they are not executing external async code, they do not even cross application boundary to be using message bus).</span></span> <span data-ttu-id="06011-295">Bu gereksiz karmaşıklığa neden tanıtılsın?</span><span class="sxs-lookup"><span data-stu-id="06011-295">Why do they introduce this unnecessary complexity?</span></span> <span data-ttu-id="06011-296">Aslında, şu ana kadar çok sayıda durumda çalışacak şekilde komut işleyicilerini engelleyen bir CQRS kod örneği görmedim.</span><span class="sxs-lookup"><span data-stu-id="06011-296">And actually, I haven't seen a CQRS code example with blocking command handlers so far, though it will work just fine in most cases.</span></span>
>
> <span data-ttu-id="06011-297">\[Greg başak\] \[... \] zaman uyumsuz bir komut yok; aslında başka bir olaydır.</span><span class="sxs-lookup"><span data-stu-id="06011-297">\[Greg Young\] \[...\] an asynchronous command doesn't exist; it's actually another event.</span></span> <span data-ttu-id="06011-298">Bana gönderdiklerinizi kabul etmem ve bir olayı daha kabul eterdiğimde, bu, bir komut \[\]değil, bir şey yapmamızı söyledim.</span><span class="sxs-lookup"><span data-stu-id="06011-298">If I must accept what you send me and raise an event if I disagree, it's no longer you telling me to do something \[that is, it’s not a command\].</span></span> <span data-ttu-id="06011-299">Bir şey yapıldığını söylemiş olursunuz.</span><span class="sxs-lookup"><span data-stu-id="06011-299">It's you telling me something has been done.</span></span> <span data-ttu-id="06011-300">Bu, ilk başta küçük bir farklılık gibi görünüyor, ancak birçok etkileri vardır.</span><span class="sxs-lookup"><span data-stu-id="06011-300">This seems like a slight difference at first, but it has many implications.</span></span>

<span data-ttu-id="06011-301">Zaman uyumsuz komutlar, hataların belirtmenin basit bir yolu olmadığından, sistemin karmaşıklığını büyük ölçüde artırır.</span><span class="sxs-lookup"><span data-stu-id="06011-301">Asynchronous commands greatly increase the complexity of a system, because there is no simple way to indicate failures.</span></span> <span data-ttu-id="06011-302">Bu nedenle, zaman uyumsuz komutlar, ölçekleme gereksinimlerinin gerekli olduğu durumlar dışında veya iç mikro hizmetleri mesajlaşma yoluyla iletişim kurarken özel durumlarda önerilmez.</span><span class="sxs-lookup"><span data-stu-id="06011-302">Therefore, asynchronous commands are not recommended other than when scaling requirements are needed or in special cases when communicating the internal microservices through messaging.</span></span> <span data-ttu-id="06011-303">Bu durumlarda, hatalara yönelik ayrı bir raporlama ve kurtarma sistemi tasarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="06011-303">In those cases, you must design a separate reporting and recovery system for failures.</span></span>

<span data-ttu-id="06011-304">EShopOnContainers 'un ilk sürümünde, HTTP isteklerinden başlatılan ve Mediator deseninin yönettiği zaman uyumlu komut işlemeyi kullanmaya karar verdik.</span><span class="sxs-lookup"><span data-stu-id="06011-304">In the initial version of eShopOnContainers, we decided to use synchronous command processing, started from HTTP requests and driven by the Mediator pattern.</span></span> <span data-ttu-id="06011-305">Bu, [Createordercommandhandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) uygulamasında olduğu gibi işlemin başarısını veya başarısızlığını kolayca döndürbırakmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="06011-305">That easily allows you to return the success or failure of the process, as in the [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) implementation.</span></span>

<span data-ttu-id="06011-306">Herhangi bir durumda bu, uygulamanızın veya mikro hizmetin iş gereksinimlerine bağlı olarak bir karar olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="06011-306">In any case, this should be a decision based on your application’s or microservice’s business requirements.</span></span>

## <a name="implement-the-command-process-pipeline-with-a-mediator-pattern-mediatr"></a><span data-ttu-id="06011-307">Komut işlem ardışık düzenini bir Mediator düzeniyle (MediatR) uygulama</span><span class="sxs-lookup"><span data-stu-id="06011-307">Implement the command process pipeline with a mediator pattern (MediatR)</span></span>

<span data-ttu-id="06011-308">Örnek bir uygulama olarak, bu kılavuz, komut alımı ve yönlendirme komutlarının, doğru komut işleyicileriyle bir şekilde kullanılması için Mediator düzenine göre işlem içi ardışık düzeni kullanmayı önerir.</span><span class="sxs-lookup"><span data-stu-id="06011-308">As a sample implementation, this guide proposes using the in-process pipeline based on the Mediator pattern to drive command ingestion and route commands, in memory, to the right command handlers.</span></span> <span data-ttu-id="06011-309">Kılavuz, ayrıca çapraz kesme sorunlarını ayırmak için [davranışları](https://github.com/jbogard/MediatR/wiki/Behaviors) uygulamayı önerir.</span><span class="sxs-lookup"><span data-stu-id="06011-309">The guide also proposes applying [behaviors](https://github.com/jbogard/MediatR/wiki/Behaviors) in order to separate cross-cutting concerns.</span></span>

<span data-ttu-id="06011-310">.NET Core 'da uygulama için, ortalama modelini uygulayan birden çok açık kaynak kitaplığı mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="06011-310">For implementation in .NET Core, there are multiple open-source libraries available that implement the Mediator pattern.</span></span> <span data-ttu-id="06011-311">Bu kılavuzda kullanılan kitaplık, [Ortaatr](https://github.com/jbogard/MediatR) açık kaynak kitaplığıdır (cemy Bogard tarafından oluşturulmuştur), ancak başka bir yaklaşım kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-311">The library used in this guide is the [MediatR](https://github.com/jbogard/MediatR) open-source library (created by Jimmy Bogard), but you could use another approach.</span></span> <span data-ttu-id="06011-312">MediatR, dekoratörler veya davranışlar uygulanırken, bir komut gibi bellek içi iletileri işlemenize olanak tanıyan küçük ve basit bir kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="06011-312">MediatR is a small and simple library that allows you to process in-memory messages like a command, while applying decorators or behaviors.</span></span>

<span data-ttu-id="06011-313">Mediator deseninin kullanılması, bağlantıyı azaltmanıza ve istenen çalışmanın kaygılarını yalıtmanıza yardımcı olur ve bu durumda, bu çalışmayı gerçekleştiren işleyiciye otomatik olarak (Bu örnekte, komut işleyicilerine) bağlantı kurar.</span><span class="sxs-lookup"><span data-stu-id="06011-313">Using the Mediator pattern helps you to reduce coupling and to isolate the concerns of the requested work, while automatically connecting to the handler that performs that work—in this case, to command handlers.</span></span>

<span data-ttu-id="06011-314">Mediator deseninin kullanılması için başka bir neden, bu kılavuzu gözden geçirirken cemy Bogard tarafından açıklanmıştı:</span><span class="sxs-lookup"><span data-stu-id="06011-314">Another good reason to use the Mediator pattern was explained by Jimmy Bogard when reviewing this guide:</span></span>

> <span data-ttu-id="06011-315">Burada test etmeyi düşünüyordum. sisteminizin davranışına yönelik iyi bir tutarlı pencere sağlar.</span><span class="sxs-lookup"><span data-stu-id="06011-315">I think it might be worth mentioning testing here – it provides a nice consistent window into the behavior of your system.</span></span> <span data-ttu-id="06011-316">İstek, yanıt verme. Bu en boy, düzenli olarak davranmakta olan testlerin oluşturulmasına oldukça değerlidir.</span><span class="sxs-lookup"><span data-stu-id="06011-316">Request-in, response-out. We’ve found that aspect quite valuable in building consistently behaving tests.</span></span>

<span data-ttu-id="06011-317">İlk olarak, yalnızca Mediator nesnesini kullanacağınız örnek bir WebAPI denetleyicisine göz atalım.</span><span class="sxs-lookup"><span data-stu-id="06011-317">First, let’s look at a sample WebAPI controller where you actually would use the mediator object.</span></span> <span data-ttu-id="06011-318">Mediator nesnesini kullanmıyorsanız, bu denetleyicinin tüm bağımlılıklarını, bir günlükçü nesnesi ve diğerleri gibi şeyleri eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="06011-318">If you were not using the mediator object, you would need to inject all the dependencies for that controller, things like a logger object and others.</span></span> <span data-ttu-id="06011-319">Bu nedenle, Oluşturucu oldukça karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="06011-319">Therefore, the constructor would be quite complicated.</span></span> <span data-ttu-id="06011-320">Diğer taraftan, Mediator nesnesini kullanırsanız, aşağıdaki örnekte olduğu gibi, her bir çapraz kesme işlemi için bir tane olmak üzere yalnızca birkaç bağımlılıkda olmak üzere denetleyicinizin Oluşturucusu çok daha basit olabilir.</span><span class="sxs-lookup"><span data-stu-id="06011-320">On the other hand, if you use the mediator object, the constructor of your controller can be a lot simpler, with just a few dependencies instead of many dependencies if you had one per cross-cutting operation, as in the following example:</span></span>

```csharp
public class MyMicroserviceController : Controller
{
    public MyMicroserviceController(IMediator mediator,
                                    IMyMicroserviceQueries microserviceQueries)
    // ...
```

<span data-ttu-id="06011-321">Mediator 'ın temiz ve yalın bir Web API denetleyici Oluşturucusu sağladığını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-321">You can see that the mediator provides a clean and lean Web API controller constructor.</span></span> <span data-ttu-id="06011-322">Ayrıca, denetleyici yöntemleri içinde Mediator nesnesine bir komut göndermek için kod neredeyse bir satırdır:</span><span class="sxs-lookup"><span data-stu-id="06011-322">In addition, within the controller methods, the code to send a command to the mediator object is almost one line:</span></span>

```csharp
[Route("new")]
[HttpPost]
public async Task<IActionResult> ExecuteBusinessOperation([FromBody]RunOpCommand
                                                               runOperationCommand)
{
    var commandResult = await _mediator.SendAsync(runOperationCommand);

    return commandResult ? (IActionResult)Ok() : (IActionResult)BadRequest();
}
```

### <a name="implement-idempotent-commands"></a><span data-ttu-id="06011-323">Idempotent komutlarını Uygula</span><span class="sxs-lookup"><span data-stu-id="06011-323">Implement idempotent Commands</span></span>

<span data-ttu-id="06011-324">**Eshoponcontainers**'da, yukarıdaki sayıdan daha gelişmiş bir örnek, sıralama mikro hizmetinden CreateOrderCommand nesnesi gönderiliyor.</span><span class="sxs-lookup"><span data-stu-id="06011-324">In **eShopOnContainers**, a more advanced example than the above is submitting a CreateOrderCommand object from the Ordering microservice.</span></span> <span data-ttu-id="06011-325">Ancak, sipariş iş süreci biraz daha karmaşık olduğundan ve bizim örneğimizde, aslında sepet mikro hizmetinde başlatıldıklarından, CreateOrderCommand nesnesini gönderme eylemi, > adlı bir tümleştirme olay işleyicisinden gerçekleştirilir. UserCheckoutAcceptedIntegrationEvent.cs] (https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/IntegrationEvents/EventHandling/UserCheckoutAcceptedIntegrationEventHandler.cs) önceki daha basit örnekte olduğu gibi, istemci uygulamasından çağrılan basit bir WebAPI denetleyicisi yerine.</span><span class="sxs-lookup"><span data-stu-id="06011-325">But since the Ordering business process is a bit more complex and, in our case, it actually starts in the Basket microservice, this action of submitting the CreateOrderCommand object is performed from an integration-event handler named >UserCheckoutAcceptedIntegrationEvent.cs](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/IntegrationEvents/EventHandling/UserCheckoutAcceptedIntegrationEventHandler.cs) instead of a simple WebAPI controller called from the client App as in the previous simpler example.</span></span>

<span data-ttu-id="06011-326">Bununla birlikte, aşağıdaki kodda gösterildiği gibi, komutunu MediatR 'ye gönderme eylemi oldukça benzerdir.</span><span class="sxs-lookup"><span data-stu-id="06011-326">Nevertheless, the action of submitting the Command to MediatR is pretty similar, as shown in the following code.</span></span>

```csharp
var createOrderCommand = new CreateOrderCommand(eventMsg.Basket.Items,
                                                eventMsg.UserId, eventMsg.City,
                                                eventMsg.Street, eventMsg.State,
                                                eventMsg.Country, eventMsg.ZipCode,
                                                eventMsg.CardNumber,
                                                eventMsg.CardHolderName,
                                                eventMsg.CardExpiration,
                                                eventMsg.CardSecurityNumber,
                                                eventMsg.CardTypeId);

var requestCreateOrder = new IdentifiedCommand<CreateOrderCommand,bool>(createOrderCommand,
                                                                        eventMsg.RequestId);
result = await _mediator.Send(requestCreateOrder);
```

<span data-ttu-id="06011-327">Bununla birlikte, Ayrıca, ıdempotent komutlarını da uygulamamız gerektiğinden, bu durum biraz daha gelişmiş bir bittir.</span><span class="sxs-lookup"><span data-stu-id="06011-327">However, this case is also a little bit more advanced because we’re also implementing idempotent commands.</span></span> <span data-ttu-id="06011-328">CreateOrderCommand işleminin ıdempotent olması gerekir, bu nedenle aynı ileti ağ üzerinden yinelenirse, yeniden denemeler gibi her nedenden dolayı aynı iş siparişi yalnızca bir kez işlenecektir.</span><span class="sxs-lookup"><span data-stu-id="06011-328">The CreateOrderCommand process should be idempotent, so if the same message comes duplicated through the network, because of any reason, like retries, the same business order will be processed just once.</span></span>

<span data-ttu-id="06011-329">Bu, iş komutu sarmalanarak (Bu durumda CreateOrderCommand) ve ıdempotent olması gereken ağ üzerinden gelen her iletinin KIMLIĞI tarafından izlenen genel bir IdentifiedCommand gömülerek uygulanır.</span><span class="sxs-lookup"><span data-stu-id="06011-329">This is implemented by wrapping the business command (in this case CreateOrderCommand) and embedding it into a generic IdentifiedCommand which is tracked by an ID of every message coming through the network that has to be idempotent.</span></span>

<span data-ttu-id="06011-330">Aşağıdaki kodda, IdentifiedCommand ile bir DTO 'ın ve KIMLIĞI artı sarmalanmış iş komut nesnesinden daha fazla şey olmadığını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-330">In the code below, you can see that the IdentifiedCommand is nothing more than a DTO with and ID plus the wrapped business command object.</span></span>

```csharp
public class IdentifiedCommand<T, R> : IRequest<R>
    where T : IRequest<R>
{
    public T Command { get; }
    public Guid Id { get; }
    public IdentifiedCommand(T command, Guid id)
    {
        Command = command;
        Id = id;
    }
}
```

<span data-ttu-id="06011-331">Daha sonra, [IdentifiedCommandHandler.cs](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/IdentifiedCommandHandler.cs) adlı IdentifiedCommand Için CommandHandler, iletinin bir parçası olarak gelen kimliğin bir tabloda zaten mevcut olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="06011-331">Then the CommandHandler for the IdentifiedCommand named [IdentifiedCommandHandler.cs](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/IdentifiedCommandHandler.cs) will basically check if the ID coming as part of the message already exists in a table.</span></span> <span data-ttu-id="06011-332">Zaten varsa, bu komut yeniden işlenmeyecek, bu nedenle bir ıdempotent komutu olarak davranır.</span><span class="sxs-lookup"><span data-stu-id="06011-332">If it already exists, that command won’t be processed again, so it behaves as an idempotent command.</span></span> <span data-ttu-id="06011-333">Bu altyapı kodu, `_requestManager.ExistAsync` aşağıdaki yöntem çağrısı tarafından gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="06011-333">That infrastructure code is performed by the `_requestManager.ExistAsync` method call below.</span></span>

```csharp
// IdentifiedCommandHandler.cs
public class IdentifiedCommandHandler<T, R> :
                                   IAsyncRequestHandler<IdentifiedCommand<T, R>, R>
                                   where T : IRequest<R>
{
    private readonly IMediator _mediator;
    private readonly IRequestManager _requestManager;

    public IdentifiedCommandHandler(IMediator mediator,
                                    IRequestManager requestManager)
    {
        _mediator = mediator;
        _requestManager = requestManager;
    }

    protected virtual R CreateResultForDuplicateRequest()
    {
        return default(R);
    }

    public async Task<R> Handle(IdentifiedCommand<T, R> message)
    {
        var alreadyExists = await _requestManager.ExistAsync(message.Id);
        if (alreadyExists)
        {
            return CreateResultForDuplicateRequest();
        }
        else
        {
            await _requestManager.CreateRequestForCommandAsync<T>(message.Id);

            // Send the embedded business command to mediator
            // so it runs its related CommandHandler
            var result = await _mediator.Send(message.Command);

            return result;
        }
    }
}
```

<span data-ttu-id="06011-334">IdentifiedCommand, bir iş komutunun zarfı gibi davrandığı için, tekrarlanmış bir kimlik olmadığından iş komutunun işlenmesi gerektiğinde, bu iç iş komutunu alır ve yukarıda gösterilen kodun en son bölümünde olduğu gibi bu iç iş komutunu yeniden gönderir. çalıştıran `_mediator.Send(message.Command)`, [IdentifiedCommandHandler.cs](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/IdentifiedCommandHandler.cs).</span><span class="sxs-lookup"><span data-stu-id="06011-334">Since the IdentifiedCommand acts like a business command’s envelope, when the business command needs to be processed because it is not a repeated Id, then it takes that inner business command and re-submits it to Mediator, as in the last part of the code shown above when running `_mediator.Send(message.Command)`, from the [IdentifiedCommandHandler.cs](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/IdentifiedCommandHandler.cs).</span></span>

<span data-ttu-id="06011-335">Bunu yaparken, aşağıdaki kodda gösterildiği gibi, işlem veritabanına karşı işlemleri çalıştıran [Createordercommandhandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) olan bu örnekte iş komut işleyicisini bağlar ve çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="06011-335">When doing that, it will link and run the business command handler, in this case, the [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) which is running transactions against the Ordering database, as shown in the following code.</span></span>

```csharp
// CreateOrderCommandHandler.cs
public class CreateOrderCommandHandler
                                   : IAsyncRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;
    private readonly IIdentityService _identityService;
    private readonly IMediator _mediator;

    // Using DI to inject infrastructure persistence Repositories
    public CreateOrderCommandHandler(IMediator mediator,
                                     IOrderRepository orderRepository,
                                     IIdentityService identityService)
    {
        _orderRepository = orderRepository ??
                          throw new ArgumentNullException(nameof(orderRepository));
        _identityService = identityService ??
                          throw new ArgumentNullException(nameof(identityService));
        _mediator = mediator ??
                                 throw new ArgumentNullException(nameof(mediator));
    }

    public async Task<bool> Handle(CreateOrderCommand message)
    {
        // Add/Update the Buyer AggregateRoot
        var address = new Address(message.Street, message.City, message.State,
                                  message.Country, message.ZipCode);
        var order = new Order(message.UserId, address, message.CardTypeId,
                              message.CardNumber, message.CardSecurityNumber,
                              message.CardHolderName, message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice,
                               item.Discount, item.PictureUrl, item.Units);
        }

        _orderRepository.Add(order);

        return await _orderRepository.UnitOfWork
            .SaveEntitiesAsync();
    }
}
```

### <a name="register-the-types-used-by-mediatr"></a><span data-ttu-id="06011-336">MediatR tarafından kullanılan türleri kaydetme</span><span class="sxs-lookup"><span data-stu-id="06011-336">Register the types used by MediatR</span></span>

<span data-ttu-id="06011-337">MediatR 'nin komut işleyici sınıflarınızın farkında olması için, IBC sınıflarını ve komut işleyici sınıflarını IOC kapsayıcısına kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="06011-337">In order for MediatR to be aware of your command handler classes, you need to register the mediator classes and the command handler classes in your IoC container.</span></span> <span data-ttu-id="06011-338">Varsayılan olarak, MediatR, IOC kapsayıcısı olarak Autofac kullanır, ancak yerleşik ASP.NET Core IOC kapsayıcısını veya MediatR tarafından desteklenen başka bir kapsayıcıyı de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-338">By default, MediatR uses Autofac as the IoC container, but you can also use the built-in ASP.NET Core IoC container or any other container supported by MediatR.</span></span>

<span data-ttu-id="06011-339">Aşağıdaki kod, Autofac modülleri kullanılırken Mediator 'ın türlerin ve komutlarının nasıl kaydedileceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="06011-339">The following code shows how to register Mediator’s types and commands when using Autofac modules.</span></span>

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterAssemblyTypes(typeof(IMediator).GetTypeInfo().Assembly)
            .AsImplementedInterfaces();

        // Register all the Command classes (they implement IAsyncRequestHandler)
        // in assembly holding the Commands
        builder.RegisterAssemblyTypes(
                              typeof(CreateOrderCommand).GetTypeInfo().Assembly).
                                   AsClosedTypesOf(typeof(IAsyncRequestHandler<,>));
        // Other types registration
        //...
    }
}
```

<span data-ttu-id="06011-340">Bu, MediatR ile "sihirli olur" dır.</span><span class="sxs-lookup"><span data-stu-id="06011-340">This is where “the magic happens” with MediatR.</span></span>

<span data-ttu-id="06011-341">Her komut işleyicisi `IAsyncRequestHandler<T>` genel arabirimi uyguladığı için, derlemeleri kaydederken kod, `CommandHandlers` ile `Commands`ilişkili olduğu şekilde `IAsyncRequestHandler` işaretlenmiş `RegisteredAssemblyTypes` tüm türlerle kaydedilir, teşekkürler Aşağıdaki örnekte olduğu gibi, `CommandHandler` sınıfında belirtilen ilişkiye:</span><span class="sxs-lookup"><span data-stu-id="06011-341">Because each command handler implements the generic `IAsyncRequestHandler<T>` interface, when registering the assemblies, the code registers with `RegisteredAssemblyTypes` all the types marked as `IAsyncRequestHandler` while relating the `CommandHandlers` with their `Commands`, thanks to the relationship stated at the `CommandHandler` class, as in the following example:</span></span>

```csharp
public class CreateOrderCommandHandler
  : IAsyncRequestHandler<CreateOrderCommand, bool>
{
```

<span data-ttu-id="06011-342">Komutları komut işleyicileriyle ilişkilendiren koddur.</span><span class="sxs-lookup"><span data-stu-id="06011-342">That is the code that correlates commands with command handlers.</span></span> <span data-ttu-id="06011-343">İşleyici sadece basit bir sınıftır, ancak öğesinden `RequestHandler<T>`devralır; burada T, komut türüdür ve mediaTR 'nin doğru yük (komut) ile çağrıldığından emin olur.</span><span class="sxs-lookup"><span data-stu-id="06011-343">The handler is just a simple class, but it inherits from `RequestHandler<T>`, where T is the command type, and MediatR makes sure it is invoked with the correct payload (the command).</span></span>

## <a name="apply-cross-cutting-concerns-when-processing-commands-with-the-behaviors-in-mediatr"></a><span data-ttu-id="06011-344">MediatR 'deki davranışlar ile komutları işlerken çapraz kesme sorunları uygulayın</span><span class="sxs-lookup"><span data-stu-id="06011-344">Apply cross-cutting concerns when processing commands with the Behaviors in MediatR</span></span>

<span data-ttu-id="06011-345">Bir şey daha vardır: ortalama işlem hattına çapraz kesme sorunları uygulayabiliyor.</span><span class="sxs-lookup"><span data-stu-id="06011-345">There is one more thing: being able to apply cross-cutting concerns to the mediator pipeline.</span></span> <span data-ttu-id="06011-346">Ayrıca, Autofac kayıt modülü kodunun sonunda, özel bir LoggingBehavior sınıfı ve bir ValidatorBehavior sınıfı olarak bir davranış türü nasıl kaydedeceğini de görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-346">You can also see at the end of the Autofac registration module code how it registers a behavior type, specifically, a custom LoggingBehavior class and a ValidatorBehavior class.</span></span> <span data-ttu-id="06011-347">Ancak başka özel davranışlar da ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-347">But you could add other custom behaviors, too.</span></span>

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterAssemblyTypes(typeof(IMediator).GetTypeInfo().Assembly)
            .AsImplementedInterfaces();

        // Register all the Command classes (they implement IAsyncRequestHandler)
        // in assembly holding the Commands
        builder.RegisterAssemblyTypes(
                              typeof(CreateOrderCommand).GetTypeInfo().Assembly).
                                   AsClosedTypesOf(typeof(IAsyncRequestHandler<,>));
        // Other types registration
        //...
        builder.RegisterGeneric(typeof(LoggingBehavior<,>)).
                                                   As(typeof(IPipelineBehavior<,>));
        builder.RegisterGeneric(typeof(ValidatorBehavior<,>)).
                                                   As(typeof(IPipelineBehavior<,>));
    }
}
```

<span data-ttu-id="06011-348">Bu [LoggingBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/LoggingBehavior.cs) sınıfı, yürütülen komut işleyicisiyle ilgili bilgileri ve başarılı olup olmadığını günlüğe kaydeden aşağıdaki kod olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="06011-348">That [LoggingBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/LoggingBehavior.cs) class can be implemented as the following code, which logs information about the command handler being executed and whether it was successful or not.</span></span>

```csharp
public class LoggingBehavior<TRequest, TResponse>
         : IPipelineBehavior<TRequest, TResponse>
{
    private readonly ILogger<LoggingBehavior<TRequest, TResponse>> _logger;
    public LoggingBehavior(ILogger<LoggingBehavior<TRequest, TResponse>> logger) =>
                                                                  _logger = logger;

    public async Task<TResponse> Handle(TRequest request,
                                        RequestHandlerDelegate<TResponse> next)
    {
        _logger.LogInformation($"Handling {typeof(TRequest).Name}");
        var response = await next();
        _logger.LogInformation($"Handled {typeof(TResponse).Name}");
        return response;
    }
}
```

<span data-ttu-id="06011-349">Yalnızca bu davranış sınıfını uygulayarak ve bunu işlem hattına kaydederek (yukarıdaki MediatorModule), MediatR aracılığıyla işlenen tüm komutlar yürütme hakkında bilgi günlüğe alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="06011-349">Just by implementing this behavior class and by registering it in the pipeline (in the MediatorModule above), all the commands processed through MediatR will be logging information about the execution.</span></span>

<span data-ttu-id="06011-350">EShopOnContainers sıralama mikro hizmeti Ayrıca, aşağıdaki kodda gösterildiği gibi, [akıcı bir doğrulama](https://github.com/JeremySkinner/FluentValidation) kitaplığına dayanan [validatorbehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/ValidatorBehavior.cs) sınıfı temel doğrulamalar için ikinci bir davranış uygular:</span><span class="sxs-lookup"><span data-stu-id="06011-350">The eShopOnContainers ordering microservice also applies a second behavior for basic validations, the [ValidatorBehavior](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/Behaviors/ValidatorBehavior.cs) class that relies on the [FluentValidation](https://github.com/JeremySkinner/FluentValidation) library, as shown in the following code:</span></span>

```csharp
public class ValidatorBehavior<TRequest, TResponse>
         : IPipelineBehavior<TRequest, TResponse>
{
    private readonly IValidator<TRequest>[] _validators;
    public ValidatorBehavior(IValidator<TRequest>[] validators) =>
                                                         _validators = validators;

    public async Task<TResponse> Handle(TRequest request,
                                        RequestHandlerDelegate<TResponse> next)
    {
        var failures = _validators
            .Select(v => v.Validate(request))
            .SelectMany(result => result.Errors)
            .Where(error => error != null)
            .ToList();

        if (failures.Any())
        {
            throw new OrderingDomainException(
                $"Command Validation Errors for type {typeof(TRequest).Name}",
                        new ValidationException("Validation exception", failures));
        }

        var response = await next();
        return response;
    }
}
```

<span data-ttu-id="06011-351">Buradaki davranış, doğrulama başarısız olursa bir özel durum ortaya koyar, ancak başarılı olursa komut sonucunu içeren bir sonuç nesnesi de döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="06011-351">The behavior here is raising an exception if validation fails, but you could also return a result object, containing the command result if it succeeded or the validation messages in case it didn’t.</span></span> <span data-ttu-id="06011-352">Bu, büyük olasılıkla kullanıcıya doğrulama sonuçlarının görüntülenmesini kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="06011-352">This would probably make it easier to display validation results to the user.</span></span>

<span data-ttu-id="06011-353">Daha sonra, akıcı bir [doğrulama](https://github.com/JeremySkinner/FluentValidation) kitaplığına bağlı olarak, aşağıdaki kodda olduğu gibi CreateOrderCommand ile geçirilen veriler için doğrulama oluşturduk:</span><span class="sxs-lookup"><span data-stu-id="06011-353">Then, based on the [FluentValidation](https://github.com/JeremySkinner/FluentValidation) library, we created validation for the data passed with CreateOrderCommand, as in the following code:</span></span>

```csharp
public class CreateOrderCommandValidator : AbstractValidator<CreateOrderCommand>
{
    public CreateOrderCommandValidator()
    {
        RuleFor(command => command.City).NotEmpty();
        RuleFor(command => command.Street).NotEmpty();
        RuleFor(command => command.State).NotEmpty();
        RuleFor(command => command.Country).NotEmpty();
        RuleFor(command => command.ZipCode).NotEmpty();
        RuleFor(command => command.CardNumber).NotEmpty().Length(12, 19);
        RuleFor(command => command.CardHolderName).NotEmpty();
        RuleFor(command => command.CardExpiration).NotEmpty().Must(BeValidExpirationDate).WithMessage("Please specify a valid card expiration date");
        RuleFor(command => command.CardSecurityNumber).NotEmpty().Length(3);
        RuleFor(command => command.CardTypeId).NotEmpty();
        RuleFor(command => command.OrderItems).Must(ContainOrderItems).WithMessage("No order items found");
    }

    private bool BeValidExpirationDate(DateTime dateTime)
    {
        return dateTime >= DateTime.UtcNow;
    }

    private bool ContainOrderItems(IEnumerable<OrderItemDTO> orderItems)
    {
        return orderItems.Any();
    }
}

```

<span data-ttu-id="06011-354">Ek doğrulamalar oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-354">You could create additional validations.</span></span> <span data-ttu-id="06011-355">Bu, komut doğrulamalarınızı uygulamak için çok temiz ve zarif bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="06011-355">This is a very clean and elegant way to implement your command validations.</span></span>

<span data-ttu-id="06011-356">Benzer bir şekilde, bunları işlerken komutlara uygulamak istediğiniz ek yönleri veya çapraz kesme sorunları için diğer davranışları uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06011-356">In a similar way, you could implement other behaviors for additional aspects or cross-cutting concerns that you want to apply to commands when handling them.</span></span>

#### <a name="additional-resources"></a><span data-ttu-id="06011-357">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="06011-357">Additional resources</span></span>

##### <a name="the-mediator-pattern"></a><span data-ttu-id="06011-358">Mediator deseninin</span><span class="sxs-lookup"><span data-stu-id="06011-358">The mediator pattern</span></span>

- <span data-ttu-id="06011-359">**Mediator stili** </span><span class="sxs-lookup"><span data-stu-id="06011-359">**Mediator pattern** </span></span>\
  [https://en.wikipedia.org/wiki/Mediator\_pattern](https://en.wikipedia.org/wiki/Mediator_pattern)

##### <a name="the-decorator-pattern"></a><span data-ttu-id="06011-360">Dekoratör deseninin</span><span class="sxs-lookup"><span data-stu-id="06011-360">The decorator pattern</span></span>

- <span data-ttu-id="06011-361">**Dekoratör stili** </span><span class="sxs-lookup"><span data-stu-id="06011-361">**Decorator pattern** </span></span>\
  [https://en.wikipedia.org/wiki/Decorator\_pattern](https://en.wikipedia.org/wiki/Decorator_pattern)

##### <a name="mediatr-jimmy-bogard"></a><span data-ttu-id="06011-362">MediatR (Jimmy Bogard)</span><span class="sxs-lookup"><span data-stu-id="06011-362">MediatR (Jimmy Bogard)</span></span>

- <span data-ttu-id="06011-363">**MediatR.**</span><span class="sxs-lookup"><span data-stu-id="06011-363">**MediatR.**</span></span> <span data-ttu-id="06011-364">GitHub deposu.</span><span class="sxs-lookup"><span data-stu-id="06011-364">GitHub repo.</span></span> \
  <https://github.com/jbogard/MediatR>

- <span data-ttu-id="06011-365">**MediatR ve Automaber ile CQRS** </span><span class="sxs-lookup"><span data-stu-id="06011-365">**CQRS with MediatR and AutoMapper** </span></span>\
  <https://lostechies.com/jimmybogard/2015/05/05/cqrs-with-mediatr-and-automapper/>

- <span data-ttu-id="06011-366">**Denetleyicilerinizi bir diet 'e yerleştirin: Gönderi ve komutlar.**</span><span class="sxs-lookup"><span data-stu-id="06011-366">**Put your controllers on a diet: POSTs and commands.**</span></span> \
  <https://lostechies.com/jimmybogard/2013/12/19/put-your-controllers-on-a-diet-posts-and-commands/>

- <span data-ttu-id="06011-367">**Bir Mediator işlem hattı ile çıkış çapraz kesme sorunları** </span><span class="sxs-lookup"><span data-stu-id="06011-367">**Tackling cross-cutting concerns with a mediator pipeline** </span></span>\
  <https://lostechies.com/jimmybogard/2014/09/09/tackling-cross-cutting-concerns-with-a-mediator-pipeline/>

- <span data-ttu-id="06011-368">**CQRS ve REST: kusursuz eşleşme** </span><span class="sxs-lookup"><span data-stu-id="06011-368">**CQRS and REST: the perfect match** </span></span>\
  <https://lostechies.com/jimmybogard/2016/06/01/cqrs-and-rest-the-perfect-match/>

- <span data-ttu-id="06011-369">**MediatR işlem hattı örnekleri** </span><span class="sxs-lookup"><span data-stu-id="06011-369">**MediatR Pipeline Examples** </span></span>\
  <https://lostechies.com/jimmybogard/2016/10/13/mediatr-pipeline-examples/>

- <span data-ttu-id="06011-370">**MediatR ve ASP.NET Core için dikey dilim test armatürleri** </span><span class="sxs-lookup"><span data-stu-id="06011-370">**Vertical Slice Test Fixtures for MediatR and ASP.NET Core** </span></span>\
  <https://lostechies.com/jimmybogard/2016/10/24/vertical-slice-test-fixtures-for-mediatr-and-asp-net-core/>

- <span data-ttu-id="06011-371">**Microsoft 'un bağımlılığı ekleme için MediatR uzantıları yayınlandı** </span><span class="sxs-lookup"><span data-stu-id="06011-371">**MediatR Extensions for Microsoft Dependency Injection Released** </span></span>\
  <https://lostechies.com/jimmybogard/2016/07/19/mediatr-extensions-for-microsoft-dependency-injection-released/>

##### <a name="fluent-validation"></a><span data-ttu-id="06011-372">Akıcı doğrulama</span><span class="sxs-lookup"><span data-stu-id="06011-372">Fluent validation</span></span>

- <span data-ttu-id="06011-373">**Jeremy SKINNER. Floentvalidation.**</span><span class="sxs-lookup"><span data-stu-id="06011-373">**Jeremy Skinner. FluentValidation.**</span></span> <span data-ttu-id="06011-374">GitHub deposu.</span><span class="sxs-lookup"><span data-stu-id="06011-374">GitHub repo.</span></span> \
  <https://github.com/JeremySkinner/FluentValidation>

> [!div class="step-by-step"]
> <span data-ttu-id="06011-375">[Önceki](microservice-application-layer-web-api-design.md)İleri
> [](../implement-resilient-applications/index.md)</span><span class="sxs-lookup"><span data-stu-id="06011-375">[Previous](microservice-application-layer-web-api-design.md)
[Next](../implement-resilient-applications/index.md)</span></span>