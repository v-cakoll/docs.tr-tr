---
title: .NET Core ile bir mikro hizmet etki alanı modeli uygulama
description: Kapsayıcılı .NET uygulamaları için .NET mikro hizmetleri mimarisi | DDD-odaklı bir etki alanı modelinin uygulama ayrıntılarına ulaşın.
ms.date: 10/08/2018
ms.openlocfilehash: b2ad62c2a16dd3993b9624ec14f0070e934ac2de
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "70296758"
---
# <a name="implement-a-microservice-domain-model-with-net-core"></a><span data-ttu-id="8c20a-103">.NET Core ile bir mikro hizmet etki alanı modeli uygulama</span><span class="sxs-lookup"><span data-stu-id="8c20a-103">Implement a microservice domain model with .NET Core</span></span>

<span data-ttu-id="8c20a-104">Önceki bölümde, bir etki alanı modeli tasarlamaya yönelik temel tasarım ilkeleri ve desenleri açıklanmıştı.</span><span class="sxs-lookup"><span data-stu-id="8c20a-104">In the previous section, the fundamental design principles and patterns for designing a domain model were explained.</span></span> <span data-ttu-id="8c20a-105">Artık, .NET Core (düz C\# kodu) ve EF Core kullanarak etki alanı modelini uygulamak için olası yolları keşfetmeye yönelik bir zaman vardır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-105">Now it is time to explore possible ways to implement the domain model by using .NET Core (plain C\# code) and EF Core.</span></span> <span data-ttu-id="8c20a-106">Etki alanı modelinizin yalnızca kodunuzun oluşyacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="8c20a-106">Note that your domain model will be composed simply of your code.</span></span> <span data-ttu-id="8c20a-107">Yalnızca EF Core model gereksinimlerine sahip olur ancak EF üzerinde gerçek bağımlılıklara sahip olmaz.</span><span class="sxs-lookup"><span data-stu-id="8c20a-107">It will have just the EF Core model requirements, but not real dependencies on EF.</span></span> <span data-ttu-id="8c20a-108">Etki alanı modelinizdeki EF Core veya başka bir ORM için sabit bağımlılıklara veya başvuru içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-108">You should not have hard dependencies or references to EF Core or any other ORM in your domain model.</span></span>

## <a name="domain-model-structure-in-a-custom-net-standard-library"></a><span data-ttu-id="8c20a-109">Özel bir .NET Standard kitaplığındaki etki alanı model yapısı</span><span class="sxs-lookup"><span data-stu-id="8c20a-109">Domain model structure in a custom .NET Standard Library</span></span>

<span data-ttu-id="8c20a-110">EShopOnContainers başvuru uygulaması için kullanılan klasör organizasyonu, uygulamanın DDD modelini gösterir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-110">The folder organization used for the eShopOnContainers reference application demonstrates the DDD model for the application.</span></span> <span data-ttu-id="8c20a-111">Farklı bir klasör kuruluşun, uygulamanız için yapılan tasarım seçimlerini daha net bir şekilde iletişim kuracağını fark edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c20a-111">You might find that a different folder organization more clearly communicates the design choices made for your application.</span></span> <span data-ttu-id="8c20a-112">Şekil 7-10 ' de görebileceğiniz gibi, sıralama etki alanı modelinde, sipariş toplama ve alıcı toplama olmak üzere iki toplama vardır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-112">As you can see in Figure 7-10, in the ordering domain model there are two aggregates, the order aggregate and the buyer aggregate.</span></span> <span data-ttu-id="8c20a-113">Her toplama, bir etki alanı varlıkları ve değer nesneleri grubudur, ancak tek bir etki alanı varlığından (Toplam kök veya kök varlık) oluşan bir toplama işlemi de olabilir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-113">Each aggregate is a group of domain entities and value objects, although you could have an aggregate composed of a single domain entity (the aggregate root or root entity) as well.</span></span>

![<span data-ttu-id="8c20a-114">BuyerAggregate ve OrderAggregate klasörlerini içeren AggregatesModel klasörünü gösteren sıralama. Domain projesi için Çözüm Gezgini görünümü, her biri varlık sınıflarını, değer nesne dosyalarını ve bu şekilde devam eder.</span><span class="sxs-lookup"><span data-stu-id="8c20a-114">The Solution Explorer view for the Ordering.Domain project, showing the AggregatesModel folder containing the BuyerAggregate and OrderAggregate folders, each one containing it's entity classes, value object files and so on.</span></span> ](./media/image11.png)

<span data-ttu-id="8c20a-115">**Şekil 7-10**.</span><span class="sxs-lookup"><span data-stu-id="8c20a-115">**Figure 7-10**.</span></span> <span data-ttu-id="8c20a-116">EShopOnContainers 'da sıralama mikro hizmeti için etki alanı model yapısı</span><span class="sxs-lookup"><span data-stu-id="8c20a-116">Domain model structure for the ordering microservice in eShopOnContainers</span></span>

<span data-ttu-id="8c20a-117">Ayrıca, etki alanı modeli katmanı, etki alanı modelinizin altyapı gereksinimleri olan depo sözleşmelerini (arabirimler) içerir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-117">Additionally, the domain model layer includes the repository contracts (interfaces) that are the infrastructure requirements of your domain model.</span></span> <span data-ttu-id="8c20a-118">Diğer bir deyişle, bu arabirimler hangi depoların ve altyapı katmanının uygulamanız gereken yöntemleri ifade ediyor.</span><span class="sxs-lookup"><span data-stu-id="8c20a-118">In other words, these interfaces express what repositories and the methods the infrastructure layer must implement.</span></span> <span data-ttu-id="8c20a-119">Depoların uygulanması, altyapı katmanı kitaplığındaki etki alanı modeli katmanının dışına yerleştirilmesi açısından önemli bir öneme sahiptir. bu nedenle, etki alanı model katmanı, Entity Framework gibi altyapı teknolojilerinde API veya sınıflar tarafından "ayrılmış" değildir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-119">It is critical that the implementation of the repositories be placed outside of the domain model layer, in the infrastructure layer library, so the domain model layer is not “contaminated” by API or classes from infrastructure technologies, like Entity Framework.</span></span>

<span data-ttu-id="8c20a-120">Ayrıca, etki alanı varlıklarınız ve değer nesneleri için temel olarak kullanabileceğiniz özel temel sınıflar içeren bir [Seedwork](https://martinfowler.com/bliki/Seedwork.html) klasörü görebilirsiniz, bu nedenle her bir etki alanının nesne sınıfında gereksiz kodunuz yoktur.</span><span class="sxs-lookup"><span data-stu-id="8c20a-120">You can also see a [SeedWork](https://martinfowler.com/bliki/Seedwork.html) folder that contains custom base classes that you can use as a base for your domain entities and value objects, so you do not have redundant code in each domain’s object class.</span></span>

## <a name="structure-aggregates-in-a-custom-net-standard-library"></a><span data-ttu-id="8c20a-121">Özel bir .NET Standard kitaplığındaki yapı toplamaları</span><span class="sxs-lookup"><span data-stu-id="8c20a-121">Structure aggregates in a custom .NET Standard library</span></span>

<span data-ttu-id="8c20a-122">Bir toplama, işlem tutarlılığını eşleştirmek için birlikte gruplanmış bir etki alanı nesneleri kümesini ifade eder.</span><span class="sxs-lookup"><span data-stu-id="8c20a-122">An aggregate refers to a cluster of domain objects grouped together to match transactional consistency.</span></span> <span data-ttu-id="8c20a-123">Bu nesneler, varlıkların örnekleri (biri toplam kök veya kök varlık) ve ek değer nesneleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-123">Those objects could be instances of entities (one of which is the aggregate root or root entity) plus any additional value objects.</span></span>

<span data-ttu-id="8c20a-124">İşlemsel tutarlılık, bir toplamanın bir iş eyleminin sonunda tutarlı ve güncel olmasını garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-124">Transactional consistency means that an aggregate is guaranteed to be consistent and up to date at the end of a business action.</span></span> <span data-ttu-id="8c20a-125">Örneğin, eShopOnContainers sıralama mikro hizmet etki alanı modelinden sıra toplaması, Şekil 7-11 ' de gösterildiği gibi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8c20a-125">For example, the order aggregate from the eShopOnContainers ordering microservice domain model is composed as shown in Figure 7-11.</span></span>

![OrderAggregate klasörünün ayrıntılı bir görünümü: Address.cs bir değer nesnesidir, ıorderrepository bir depo arabirimidir, Order.cs bir toplama köküdür, OrderItem.cs ise bir alt varlıktır ve OrderStatus.cs bir numaralandırma sınıfıdır.](./media/image12.png)

<span data-ttu-id="8c20a-127">**Şekil 7-11**.</span><span class="sxs-lookup"><span data-stu-id="8c20a-127">**Figure 7-11**.</span></span> <span data-ttu-id="8c20a-128">Visual Studio çözümünde sıra toplaması</span><span class="sxs-lookup"><span data-stu-id="8c20a-128">The order aggregate in Visual Studio solution</span></span>

<span data-ttu-id="8c20a-129">Bir toplama klasöründeki dosyalardan herhangi birini açarsanız, [Seedwork](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.Domain/SeedWork) klasöründe uygulandığı şekilde varlık veya değer nesnesi gibi özel bir temel sınıf veya arabirim olarak nasıl işaretlendiğini görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c20a-129">If you open any of the files in an aggregate folder, you can see how it is marked as either a custom base class or interface, like entity or value object, as implemented in the [SeedWork](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.Domain/SeedWork) folder.</span></span>

## <a name="implement-domain-entities-as-poco-classes"></a><span data-ttu-id="8c20a-130">POCO sınıfları olarak etki alanı varlıklarını uygulama</span><span class="sxs-lookup"><span data-stu-id="8c20a-130">Implement domain entities as POCO classes</span></span>

<span data-ttu-id="8c20a-131">Etki alanı varlıklarınızı uygulayan POCO sınıfları oluşturarak .NET içinde bir etki alanı modeli uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c20a-131">You implement a domain model in .NET by creating POCO classes that implement your domain entities.</span></span> <span data-ttu-id="8c20a-132">Aşağıdaki örnekte, Order sınıfı bir varlık olarak ve ayrıca bir toplam kök olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-132">In the following example, the Order class is defined as an entity and also as an aggregate root.</span></span> <span data-ttu-id="8c20a-133">Order sınıfı varlık temel sınıfından türetildiğinden, varlıklarla ilgili ortak kodu yeniden kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-133">Because the Order class derives from the Entity base class, it can reuse common code related to entities.</span></span> <span data-ttu-id="8c20a-134">Bu temel sınıfların ve arabirimlerin, etki alanı modeli projesinde sizin tarafınızdan tanımlandığını göz önünde bulundurun. bu nedenle, f gibi bir ORM 'den altyapı kodu değil kodunuz olur.</span><span class="sxs-lookup"><span data-stu-id="8c20a-134">Bear in mind that these base classes and interfaces are defined by you in the domain model project, so it is your code, not infrastructure code from an ORM like EF.</span></span>

```csharp
// COMPATIBLE WITH ENTITY FRAMEWORK CORE 2.0
// Entity is a custom base class with the ID
public class Order : Entity, IAggregateRoot
{
    private DateTime _orderDate;
    public Address Address { get; private set; }
    private int? _buyerId;

    public OrderStatus OrderStatus { get; private set; }
    private int _orderStatusId;

    private string _description;
    private int? _paymentMethodId;

    private readonly List<OrderItem> _orderItems;
    public IReadOnlyCollection<OrderItem> OrderItems => _orderItems;

    public Order(string userId, Address address, int cardTypeId, string cardNumber, string cardSecurityNumber,
            string cardHolderName, DateTime cardExpiration, int? buyerId = null, int? paymentMethodId = null)
    {
        _orderItems = new List<OrderItem>();
        _buyerId = buyerId;
        _paymentMethodId = paymentMethodId;
        _orderStatusId = OrderStatus.Submitted.Id;
        _orderDate = DateTime.UtcNow;
        Address = address;

        // ...Additional code ...
    }

    public void AddOrderItem(int productId, string productName,
                            decimal unitPrice, decimal discount,
                            string pictureUrl, int units = 1)
    {
        //...
        // Domain rules/logic for adding the OrderItem to the order
        // ...

        var orderItem = new OrderItem(productId, productName, unitPrice, discount, pictureUrl, units);

        _orderItems.Add(orderItem);

    }
    // ...
    // Additional methods with domain rules/logic related to the Order aggregate
    // ...
}
```

<span data-ttu-id="8c20a-135">Bu, POCO sınıfı olarak uygulanan bir etki alanı varlığı olduğunu unutmamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-135">It is important to note that this is a domain entity implemented as a POCO class.</span></span> <span data-ttu-id="8c20a-136">Entity Framework Core veya başka bir altyapı çerçevesine doğrudan bağımlılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="8c20a-136">It does not have any direct dependency on Entity Framework Core or any other infrastructure framework.</span></span> <span data-ttu-id="8c20a-137">Bu uygulama, ddd, yalnızca bir etki alanı modeli uygulayan C\# koduna sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-137">This implementation is as it should be in DDD, just C\# code implementing a domain model.</span></span>

<span data-ttu-id="8c20a-138">Ayrıca, sınıfı IAggregateRoot adlı bir arabirimle birlikte tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-138">In addition, the class is decorated with an interface named IAggregateRoot.</span></span> <span data-ttu-id="8c20a-139">Bu arabirim, bazen yalnızca bu varlık sınıfının de bir toplam kök olduğunu göstermek için kullanılan *boş bir arabirimdir*.</span><span class="sxs-lookup"><span data-stu-id="8c20a-139">That interface is an empty interface, sometimes called a *marker interface*, that is used just to indicate that this entity class is also an aggregate root.</span></span>

<span data-ttu-id="8c20a-140">İşaretleyici arabirimi bazen bir anti-model olarak kabul edilir; Ancak, özellikle de bu arabirim gelişiyor olabileceği gibi, bir sınıfı işaretlemek için de temiz bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="8c20a-140">A marker interface is sometimes considered as an anti-pattern; however, it is also a clean way to mark a class, especially when that interface might be evolving.</span></span> <span data-ttu-id="8c20a-141">Bir öznitelik, işaretleyici için diğer seçim olabilir, ancak sınıfın üzerine bir toplama özniteliği işareti koymak yerine ıaggregate arabiriminin yanındaki temel sınıfı (varlık) görmek daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-141">An attribute could be the other choice for the marker, but it is quicker to see the base class (Entity) next to the IAggregate interface instead of putting an Aggregate attribute marker above the class.</span></span> <span data-ttu-id="8c20a-142">Bu, herhangi bir durumda tercihlerden bağımsız olur.</span><span class="sxs-lookup"><span data-stu-id="8c20a-142">It is a matter of preferences, in any case.</span></span>

<span data-ttu-id="8c20a-143">Toplam bir köke sahip olmak, toplamanın varlıkların tutarlılık ve iş kurallarıyla ilgili kodların büyük bir kısmının sıra toplama kök sınıfında (örneğin, toplama için bir OrderItem nesnesi eklenirken Addorderıtem) Yöntemler olarak uygulanmasıdır. .</span><span class="sxs-lookup"><span data-stu-id="8c20a-143">Having an aggregate root means that most of the code related to consistency and business rules of the aggregate’s entities should be implemented as methods in the Order aggregate root class (for example, AddOrderItem when adding an OrderItem object to the aggregate).</span></span> <span data-ttu-id="8c20a-144">OrderItems nesnelerini bağımsız olarak veya doğrudan oluşturmanız veya güncelleştirmemelisiniz; AggregateRoot sınıfı, alt varlıklara karşı herhangi bir güncelleştirme işleminin denetimini ve tutarlılığını tutmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-144">You should not create or update OrderItems objects independently or directly; the AggregateRoot class must keep control and consistency of any update operation against its child entities.</span></span>

## <a name="encapsulate-data-in-the-domain-entities"></a><span data-ttu-id="8c20a-145">Etki alanı varlıklarındaki verileri yalıtma</span><span class="sxs-lookup"><span data-stu-id="8c20a-145">Encapsulate data in the Domain Entities</span></span>

<span data-ttu-id="8c20a-146">Varlık modellerindeki yaygın bir sorun, koleksiyon gezinti özelliklerini herkese açık olarak erişilebilen liste türleri olarak kullanıma sunmasıdır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-146">A common problem in entity models is that they expose collection navigation properties as publicly accessible list types.</span></span> <span data-ttu-id="8c20a-147">Bu, tüm ortak çalışan geliştiricisinin bu koleksiyon türlerinin içeriğini işlemesini sağlar. Bu, koleksiyonda ilgili önemli iş kurallarını atlayabilir ve bu da nesneyi geçersiz bir durumda bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-147">This allows any collaborator developer to manipulate the contents of these collection types, which may bypass important business rules related to the collection, possibly leaving the object in an invalid state.</span></span> <span data-ttu-id="8c20a-148">Bunun çözümü, ilgili koleksiyonlara salt okuma erişimi sunmak ve istemcilerin bunları işleyebilecekleri yolları tanımlayan yöntemleri açıkça sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-148">The solution to this is to expose read-only access to related collections and explicitly provide methods that define ways in which clients can manipulate them.</span></span>

<span data-ttu-id="8c20a-149">Önceki kodda, pek çok özniteliğin salt okunurdur veya Private olduğunu ve yalnızca sınıf yöntemleri tarafından güncelleştirilebilir olduğunu, bu nedenle tüm güncelleştirmeler, iş etki alanı iç türevlerini ve sınıf yöntemleri içinde belirtilen mantığı kabul eder.</span><span class="sxs-lookup"><span data-stu-id="8c20a-149">In the previous code, note that many attributes are read-only or private and are only updatable by the class methods, so any update considers business domain invariants and logic specified within the class methods.</span></span>

<span data-ttu-id="8c20a-150">Örneğin, DDD desenlerinin ardından, herhangi bir komut işleyici yönteminden veya uygulama katmanı sınıfından **şunları yapmanız**  gerekmez (Aslında bunun sizin için imkansız olması gerekir):</span><span class="sxs-lookup"><span data-stu-id="8c20a-150">For example, following DDD patterns, **you should *not* do the following** from any command handler method or application layer class (actually, it should be impossible for you to do so):</span></span>

```csharp
// WRONG ACCORDING TO DDD PATTERNS – CODE AT THE APPLICATION LAYER OR
// COMMAND HANDLERS
// Code in command handler methods or Web API controllers
//... (WRONG) Some code with business logic out of the domain classes ...
OrderItem myNewOrderItem = new OrderItem(orderId, productId, productName,
    pictureUrl, unitPrice, discount, units);

//... (WRONG) Accessing the OrderItems collection directly from the application layer // or command handlers
myOrder.OrderItems.Add(myNewOrderItem);
//...
```

<span data-ttu-id="8c20a-151">Bu durumda, Add yöntemi yalnızca OrderItems koleksiyonuna doğrudan erişim ile veri eklemek için bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-151">In this case, the Add method is purely an operation to add data, with direct access to the OrderItems collection.</span></span> <span data-ttu-id="8c20a-152">Bu nedenle, alt varlıklar ile ilgili etki alanı mantığının, kuralların veya bu işlemle ilgili doğrulamaların çoğu uygulama katmanında (komut işleyicileri ve Web API denetleyicileri) yayalınacaktır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-152">Therefore, most of the domain logic, rules, or validations related to that operation with the child entities will be spread across the application layer (command handlers and Web API controllers).</span></span>

<span data-ttu-id="8c20a-153">Toplama kökünün çevresine giderseniz, toplam kök, onun geçerliliğini, geçerliliğini veya tutarlılığını garanti edemez.</span><span class="sxs-lookup"><span data-stu-id="8c20a-153">If you go around the aggregate root, the aggregate root cannot guarantee its invariants, its validity, or its consistency.</span></span> <span data-ttu-id="8c20a-154">Sonuç olarak, istenmeyen bir kod ya da işlemsel betik kodunuz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-154">Eventually you will have spaghetti code or transactional script code.</span></span>

<span data-ttu-id="8c20a-155">DDD desenlerini izlemek için, varlıkların herhangi bir varlık özelliğinde ortak ayarlayıcılarına sahip olmaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-155">To follow DDD patterns, entities must not have public setters in any entity property.</span></span> <span data-ttu-id="8c20a-156">Bir varlıktaki değişiklikler, varlıkta gerçekleştirdikleri değişiklik hakkında açık olan açık yöntemlerle birlikte kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-156">Changes in an entity should be driven by explicit methods with explicit ubiquitous language about the change they are performing in the entity.</span></span>

<span data-ttu-id="8c20a-157">Ayrıca, varlık içindeki Koleksiyonlar (Order Items gibi) salt okunur özellikler olmalıdır (daha sonra açıklanan AsReadOnly yöntemi).</span><span class="sxs-lookup"><span data-stu-id="8c20a-157">Furthermore, collections within the entity (like the order items) should be read-only properties (the AsReadOnly method explained later).</span></span> <span data-ttu-id="8c20a-158">Yalnızca toplam kök sınıf yöntemleri veya alt varlık yöntemlerinden içinden güncelleştirme yapabilmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="8c20a-158">You should be able to update it only from within the aggregate root class methods or the child entity methods.</span></span>

<span data-ttu-id="8c20a-159">Sipariş toplama kökünün kodunda görebileceğiniz gibi, tüm ayarlayıcılar özel veya en azından Salt okunabilir olmalıdır, böylece varlığın verilerine veya alt varlıklarına karşı herhangi bir işlem varlık sınıfındaki yöntemlerle gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-159">As you can see in the code for the Order aggregate root, all setters should be private or at least read-only externally, so that any operation against the entity’s data or its child entities has to be performed through methods in the entity class.</span></span> <span data-ttu-id="8c20a-160">Bu işlem, işlemsel betik kodu uygulamak yerine denetimli ve nesne odaklı bir şekilde tutarlılık sağlar.</span><span class="sxs-lookup"><span data-stu-id="8c20a-160">This maintains consistency in a controlled and object-oriented way instead of implementing transactional script code.</span></span>

<span data-ttu-id="8c20a-161">Aşağıdaki kod parçacığı, sıra toplamasına bir OrderItem nesnesi ekleme görevini kodun doğru yolunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-161">The following code snippet shows the proper way to code the task of adding an OrderItem object to the Order aggregate.</span></span>

```csharp
// RIGHT ACCORDING TO DDD--CODE AT THE APPLICATION LAYER OR COMMAND HANDLERS
// The code in command handlers or WebAPI controllers, related only to application stuff
// There is NO code here related to OrderItem object’s business logic
myOrder.AddOrderItem(productId, productName, pictureUrl, unitPrice, discount, units);

// The code related to OrderItem params validations or domain rules should
// be WITHIN the AddOrderItem method.

//...
```

<span data-ttu-id="8c20a-162">Bu kod parçacığında, bir OrderItem nesnesinin oluşturulmasıyla ilgili doğrulamaları veya mantığın çoğu, Addorderıtem yönteminde, özellikle de toplamadaki diğer öğelerle ilgili doğrulamalar ve mantık olan sıra toplama kökünün denetimi altında olacaktır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-162">In this snippet, most of the validations or logic related to the creation of an OrderItem object will be under the control of the Order aggregate root—in the AddOrderItem method—especially validations and logic related to other elements in the aggregate.</span></span> <span data-ttu-id="8c20a-163">Örneğin, birden çok Addorderıtem çağrısının sonucuyla aynı ürün öğesini elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c20a-163">For instance, you might get the same product item as the result of multiple calls to AddOrderItem.</span></span> <span data-ttu-id="8c20a-164">Bu yöntemde, ürün öğelerini inceleyebilir ve aynı ürün öğelerini birçok birim ile tek bir OrderItem nesnesi halinde birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c20a-164">In that method, you could examine the product items and consolidate the same product items into a single OrderItem object with several units.</span></span> <span data-ttu-id="8c20a-165">Ayrıca, farklı indirim miktarları varsa ancak ürün KIMLIĞI aynıysa, büyük olasılıkla daha yüksek iskontoyu uygularsınız.</span><span class="sxs-lookup"><span data-stu-id="8c20a-165">Additionally, if there are different discount amounts but the product ID is the same, you would likely apply the higher discount.</span></span> <span data-ttu-id="8c20a-166">Bu ilke, OrderItem nesnesinin diğer tüm etki alanı mantığına yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-166">This principle applies to any other domain logic for the OrderItem object.</span></span>

<span data-ttu-id="8c20a-167">Ayrıca, yeni OrderItem (params) işlemi de düzen toplama kökünden Addorderıtem yöntemi tarafından denetlenir ve gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-167">In addition, the new OrderItem(params) operation will also be controlled and performed by the AddOrderItem method from the Order aggregate root.</span></span> <span data-ttu-id="8c20a-168">Bu nedenle, bu işlemle ilgili mantık veya doğrulamaların çoğu (özellikle diğer alt varlıklar arasındaki tutarlılığı etkileyen her şey), toplam kökte tek bir yerde olacaktır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-168">Therefore, most of the logic or validations related to that operation (especially anything that impacts the consistency between other child entities) will be in a single place within the aggregate root.</span></span> <span data-ttu-id="8c20a-169">Bu, toplam kök deseninin nihai amacı olur.</span><span class="sxs-lookup"><span data-stu-id="8c20a-169">That is the ultimate purpose of the aggregate root pattern.</span></span>

<span data-ttu-id="8c20a-170">Entity Framework Core 1,1 veya sonraki bir sürümü kullandığınızda, özelliklere ek olarak [alanlarla eşleştirmeye](https://docs.microsoft.com/ef/core/modeling/backing-field) ızın verdiğinden ddd varlığı daha iyi ifade edilebilir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-170">When you use Entity Framework Core 1.1 or later, a DDD entity can be better expressed because it allows [mapping to fields](https://docs.microsoft.com/ef/core/modeling/backing-field) in addition to properties.</span></span> <span data-ttu-id="8c20a-171">Bu, alt varlıkların veya değer nesnelerinin koleksiyonlarını koruurken yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="8c20a-171">This is useful when protecting collections of child entities or value objects.</span></span> <span data-ttu-id="8c20a-172">Bu geliştirmelerden bazıları özellikler yerine basit özel alanları kullanabilir ve genel yöntemlerde alan koleksiyonuna herhangi bir güncelleştirmeyi uygulayabilir ve AsReadOnly yöntemi aracılığıyla salt okunur erişim sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c20a-172">With this enhancement, you can use simple private fields instead of properties and you can implement any update to the field collection in public methods and provide read-only access through the AsReadOnly method.</span></span>

<span data-ttu-id="8c20a-173">DDD 'da, verilerin herhangi bir kısmını ve tutarlılığını denetlemek için yalnızca varlıktaki yöntemler aracılığıyla varlığı (veya Oluşturucu) güncellemek isteyebilirsiniz, böylece özellikler yalnızca bir get erişimcisi ile tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-173">In DDD you want to update the entity only through methods in the entity (or the constructor) in order to control any invariant and the consistency of the data, so properties are defined only with a get accessor.</span></span> <span data-ttu-id="8c20a-174">Özellikler özel alanlarla desteklenir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-174">The properties are backed by private fields.</span></span> <span data-ttu-id="8c20a-175">Özel üyelere yalnızca sınıfının içinden erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-175">Private members can only be accessed from within the class.</span></span> <span data-ttu-id="8c20a-176">Ancak, bir özel durum vardır: EF Core bu alanları da ayarlaması gerekir (Bu nedenle nesneyi doğru değerlerle döndürebilir).</span><span class="sxs-lookup"><span data-stu-id="8c20a-176">However, there one exception: EF Core needs to set these fields as well (so it can return the object with the proper values).</span></span>

### <a name="map-properties-with-only-get-accessors-to-the-fields-in-the-database-table"></a><span data-ttu-id="8c20a-177">Özellikleri veritabanı tablosundaki alanlara yalnızca get erişimcileri ile eşleyin</span><span class="sxs-lookup"><span data-stu-id="8c20a-177">Map properties with only get accessors to the fields in the database table</span></span>

<span data-ttu-id="8c20a-178">Özellikleri veritabanı tablosu sütunlarına eşleme, altyapı ve kalıcılık katmanının bir parçası olarak bir etki alanı sorumluluğu değildir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-178">Mapping properties to database table columns is not a domain responsibility but part of the infrastructure and persistence layer.</span></span> <span data-ttu-id="8c20a-179">Burada, varlıkların nasıl modellemeytireceğiniz ile ilgili EF Core 1,1 veya sonraki sürümlerde yeni yetenekler hakkında farkında olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-179">We mention this here just so you are aware of the new capabilities in EF Core 1.1 or later related to how you can model entities.</span></span> <span data-ttu-id="8c20a-180">Bu konuyla ilgili ek ayrıntılar altyapı ve kalıcılık bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-180">Additional details on this topic are explained in the infrastructure and persistence section.</span></span>

<span data-ttu-id="8c20a-181">EF Core 1,0 veya sonraki bir sürümü kullandığınızda, DbContext içinde, yalnızca alıcıları ile tanımlanan özellikleri veritabanı tablosundaki gerçek alanlarla eşlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-181">When you use EF Core 1.0 or later, within the DbContext you need to map the properties that are defined only with getters to the actual fields in the database table.</span></span> <span data-ttu-id="8c20a-182">Bu, PropertyBuilder sınıfının HasField yöntemiyle yapılır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-182">This is done with the HasField method of the PropertyBuilder class.</span></span>

### <a name="map-fields-without-properties"></a><span data-ttu-id="8c20a-183">Özellikleri olmayan harita alanları</span><span class="sxs-lookup"><span data-stu-id="8c20a-183">Map fields without properties</span></span>

<span data-ttu-id="8c20a-184">Sütunları alanlarla eşlemek için EF Core 1,1 veya sonraki bir sürüme sahip bir özellik ile, özellikler de kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="8c20a-184">With the feature in EF Core 1.1 or later to map columns to fields, it is also possible to not use properties.</span></span> <span data-ttu-id="8c20a-185">Bunun yerine, sütunları yalnızca bir tablodan alanlarla eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8c20a-185">Instead, you can just map columns from a table to fields.</span></span> <span data-ttu-id="8c20a-186">Bunun için ortak kullanım örneği, varlığın dışından erişilmesi gerekmeyen iç durum için özel alanlardır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-186">A common use case for this is private fields for an internal state that does not need to be accessed from outside the entity.</span></span>

<span data-ttu-id="8c20a-187">Örneğin, önceki orderaggregate kod örneğinde, bir ayarlayıcı veya alıcı için ilgili özelliği olmayan, `_paymentMethodId` alanı gibi birkaç özel alan vardır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-187">For example, in the preceding OrderAggregate code example, there are several private fields, like the  `_paymentMethodId` field, that have no related property for either a setter or getter.</span></span> <span data-ttu-id="8c20a-188">Bu alan Ayrıca, siparişin iş mantığı dahilinde hesaplanabilecek ve siparişin yöntemlerinden, ancak veritabanında kalıcı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8c20a-188">That field could also be calculated within the order’s business logic and used from the order’s methods, but it needs to be persisted in the database as well.</span></span> <span data-ttu-id="8c20a-189">Bu nedenle EF Core (v 1.1 ' den itibaren), ilgili bir özellik olmadan bir alanı veritabanındaki bir sütuna eşlemek için bir yol vardır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-189">So in EF Core (since v1.1) there is a way to map a field without a related property to a column in the database.</span></span> <span data-ttu-id="8c20a-190">Bu, bu kılavuzun [altyapı katmanı](ddd-oriented-microservice.md#the-infrastructure-layer) bölümünde de açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="8c20a-190">This is also explained in the [Infrastructure layer](ddd-oriented-microservice.md#the-infrastructure-layer) section of this guide.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="8c20a-191">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8c20a-191">Additional resources</span></span>

- <span data-ttu-id="8c20a-192">**Vaughn versuz. DDD ve Entity Framework ile toplamalar Modellendirme.**</span><span class="sxs-lookup"><span data-stu-id="8c20a-192">**Vaughn Vernon. Modeling Aggregates with DDD and Entity Framework.**</span></span> <span data-ttu-id="8c20a-193">Bunun *Entity Framework Core olmadığına* unutmayın.</span><span class="sxs-lookup"><span data-stu-id="8c20a-193">Note that this is *not* Entity Framework Core.</span></span> \
  <https://kalele.io/blog-posts/modeling-aggregates-with-ddd-and-entity-framework/>

- <span data-ttu-id="8c20a-194">**Julie Lerman. Veri noktaları-etki alanı odaklı tasarım için kodlama: Veri odaklı Devs ipuçları** </span><span class="sxs-lookup"><span data-stu-id="8c20a-194">**Julie Lerman. Data Points - Coding for Domain-Driven Design: Tips for Data-Focused Devs** </span></span>\
  <https://msdn.microsoft.com/magazine/dn342868.aspx>

- <span data-ttu-id="8c20a-195">**UDI Dahan. Tamamen kapsüllenmiş etki alanı modelleri oluşturma** </span><span class="sxs-lookup"><span data-stu-id="8c20a-195">**Udi Dahan. How to create fully encapsulated Domain Models** </span></span>\
  <http://udidahan.com/2008/02/29/how-to-create-fully-encapsulated-domain-models/>

> [!div class="step-by-step"]
> <span data-ttu-id="8c20a-196">[Önceki](microservice-domain-model.md)İleri
> [](seedwork-domain-model-base-classes-interfaces.md)</span><span class="sxs-lookup"><span data-stu-id="8c20a-196">[Previous](microservice-domain-model.md)
[Next](seedwork-domain-model-base-classes-interfaces.md)</span></span>