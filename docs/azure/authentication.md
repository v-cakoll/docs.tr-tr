---
title: .NET için Azure kitaplıklarında kimlik doğrulamasını anlama
description: .NET için Azure SDK ile kimlik doğrulamanın farklı yollarını açıklar.
ms.date: 06/19/2020
ms.custom: azure-sdk-dotnet
ms.openlocfilehash: 727842b34faa37558220a3035ac5228fae196201
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301625"
---
# <a name="authenticate-with-the-azure-sdk-for-net"></a>.NET için Azure SDK ile kimlik doğrulama

## <a name="recommended-azureidentity"></a>Önerilen: Azure. Identity

.NET için Azure SDK 'sindeki en son paketler, kimlik doğrulaması için ortak bir kimlik doğrulama paketi kullanır `Azure.Identity` . `Azure.Identity`Bu belgenin ilerleyen kısımlarında açıklanan diğer kimlik doğrulama mekanizmaları üzerinde kullanılması önerilir. Tarafından sunulan kimlik bilgilerini destekleyen paketlerin `Azure.Identity` Azure ile başlayan paket tanımlayıcıları vardır *.* [Daha fazla bilgi için bkz. .net Için Azure SDK 'da en son yayınlar](https://azure.github.io/azure-sdk/releases/latest/index.html#net).

Projenizde kullanmayla ilgili tüm yönergeler için `Azure.Identity` bkz. [.net Için Azure Identity Client](/dotnet/api/overview/azure/identity-readme)belgeleri.

> [!TIP]
> Azure kaynaklarını yönetmek ve bunlara erişmek için Azure kimliği kullanma örnekleri için bkz. Azure [kimliği, kaynak yönetimi ve depolama örneği](/samples/dotnet/samples/azure-identity-resource-management-storage/) .

Azure. Identity ' ı desteklemeyen kitaplıklarda kimlik doğrulaması yapmak için bu konunun geri kalanına bakın.

## <a name="access-azure-resources"></a>Azure kaynaklarına erişin

Key Vault bir gizli dizi alma veya depolama alanında BLOB depolama gibi Azure kaynaklarıyla etkileşim kurmak için, birçok Azure hizmet kitaplığı, kimlik doğrulaması için bir bağlantı dizesi veya anahtar gerektirir. Örneğin, SQL veritabanı [Standart BIR SQL bağlantı dizesi](https://docs.microsoft.com/azure/azure-sql/database/connect-query-dotnet-core)kullanır. Hizmet bağlantı dizeleri [Cosmosdb](/azure/cosmos-db/), [redin için Azure Cache](/azure/azure-cache-for-redis/cache-dotnet-how-to-use-azure-redis-cache)ve [Service Bus](/azure/service-bus-messaging/service-bus-dotnet-get-started-with-queues)gibi diğer Azure hizmetlerinde kullanılır. Bu dizeleri Azure portal, CLı veya PowerShell kullanarak edinebilirsiniz. Kodunuzda bağlantı dizeleri oluşturmak üzere kaynakları sorgulamak için .NET için Azure Yönetim kitaplıklarını da kullanabilirsiniz.

Bir bağlantı dizesi kullanma yöntemleri ürüne göre farklılık gösterir. [Azure ürününüzün belgelerine bakın](/azure/?product=featured).

## <a name="manage-azure-resources"></a>Azure kaynaklarını yönetme

[!include[Create service principal](includes/create-sp.md)]

Hizmet sorumlusu oluşturulduktan sonra, kaynakları oluşturmak ve yönetmek için hizmet sorumlusunun kimliğini doğrulamak üzere iki seçenek mevcuttur.

Her iki seçenek için de aşağıdaki NuGet paketlerini projenize eklemeniz gerekir.

```powershell
Install-Package Microsoft.Azure.Management.Fluent
Install-Package Microsoft.Azure.Management.ResourceManager.Fluent
```

### <a name="authenticate-with-token-credentials"></a>Belirteç kimlik bilgileriyle kimlik doğrulama

İlk yöntem, koddaki Token Credential nesnesini oluşturmak için kullanılır. Kimlik bilgilerini bir yapılandırma dosyasında, kayıt defterinde veya Azure Keykasasında güvenli bir şekilde depolamanız gerekir.

```csharp
var credentials = SdkContext.AzureCredentialsFactory
    .FromServicePrincipal(clientId,
        clientSecret,
        tenantId,
        AzureEnvironment.AzureGlobalCloud);
```

Hizmet sorumlusunu oluştururken JSON çıktısından *ClientID*, *ClientSecret*ve *tenantıd* değerlerini kullanın.

Ardından, `Azure` API ile çalışmaya başlamak için giriş noktası nesnesini oluşturun:

```csharp
var azure = Microsoft.Azure.Management.Fluent.Azure
    .Configure()
    .Authenticate(credentials)
    .WithDefaultSubscription();
```

JSON çıktısından nesnesine açıkça *abonelik* sağlamanız önerilir `Azure` :

```csharp
var azure = Microsoft.Azure.Management.Fluent.Azure
    .Configure()
    .Authenticate(credentials)
    .WithSubscription(subscriptionId);
```

### <a name="file-based-authentication"></a><a name="mgmt-file"></a>Dosya tabanlı kimlik doğrulaması

Dosya tabanlı kimlik doğrulaması, hizmet sorumlusu kimlik bilgilerini bir düz metin dosyasına yerleştirip dosya sistemi içinde güvenli hale getirmeye olanak tanır.

[!include[File-based authentication](includes/file-based-auth.md)]

API ile çalışmaya başlamak için dosyanın içeriğini okuyun ve giriş noktası `Azure` nesnesini oluşturun:

```csharp
// pull in the location of the authentication properties file from the environment
var credentials = SdkContext.AzureCredentialsFactory
    .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

var azure = Microsoft.Azure.Management.Fluent.Azure
    .Configure()
    .Authenticate(credentials)
    .WithDefaultSubscription();
```
