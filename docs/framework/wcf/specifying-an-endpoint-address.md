---
title: Bir Uç Noktası Adresi Belirtme
description: WCF 'de ServiceEndpoint 'in bir parçası olan bir uç nokta adresi hakkında bilgi edinin. WCF hizmeti olan tüm iletişimler uç noktaları aracılığıyla oluşur.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- endpoints [WCF], addressing
ms.assetid: ac24f5ad-9558-4298-b168-c473c68e819b
ms.openlocfilehash: e1bd9e5a27d1bc86d2d3e04ee82221a27a4e1fa8
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245992"
---
# <a name="specifying-an-endpoint-address"></a>Bir Uç Noktası Adresi Belirtme

Bir Windows Communication Foundation (WCF) hizmeti ile tüm iletişim uç noktaları aracılığıyla oluşur. Her biri <xref:System.ServiceModel.Description.ServiceEndpoint> <xref:System.ServiceModel.Description.ServiceEndpoint.Address%2A> , bir <xref:System.ServiceModel.Description.ServiceEndpoint.Binding%2A> ve içerir <xref:System.ServiceModel.Description.ServiceEndpoint.Contract%2A> . Sözleşme hangi işlemlerin kullanılabilir olduğunu belirtir. Bağlama, hizmetle nasıl iletişim kurabileceğinizi belirtir ve adres hizmetin nerede bulunacağını belirtir. Her uç noktanın benzersiz bir adresi olmalıdır. Uç nokta adresi, <xref:System.ServiceModel.EndpointAddress> hizmetin adresini temsil eden bir Tekdüzen Kaynak tanımlayıcısı (URI) içeren sınıf tarafından temsil edilir. Bu, <xref:System.ServiceModel.EndpointAddress.Identity%2A> hizmetin güvenlik kimliğini ve isteğe bağlı bir koleksiyonu temsil eder <xref:System.ServiceModel.EndpointAddress.Headers%2A> . İsteğe bağlı üstbilgiler, uç noktayı tanımlamak veya ile etkileşimde bulunmak için daha ayrıntılı adresleme bilgileri sağlar. Örneğin, üst bilgilerin bir yanıt iletisi gönderebilmesi veya birden çok örnek kullanılabilir olduğunda belirli bir kullanıcıdan gelen bir iletiyi işlemek için kullanılacak bir hizmet örneği olan gelen bir iletiyi nasıl işleyeceğini belirtebilir.

## <a name="definition-of-an-endpoint-address"></a>Bir uç nokta adresinin tanımı

WCF 'de, <xref:System.ServiceModel.EndpointAddress> ws-Addressing standardında tanımlanan bir uç nokta başvurusu (EPR) modeller.

Çoğu taşımanın adres URI 'sinin dört bölümü vardır. Örneğin, bu URI `http://www.fabrikam.com:322/mathservice.svc/secureEndpoint` aşağıdaki dört parçaya sahiptir:

- Düzen: http:

- Makin`www.fabrikam.com`

- Seçim Bağlantı noktası: 322

- Yol:/mathservice.svc/secureEndpoint

EPR modelinin bir parçası olan her bir uç nokta başvurusunun, ek tanımlayıcı bilgi ekleyen bazı başvuru parametrelerini taşıyacağından emin olur. WCF 'de, bu başvuru parametreleri sınıfının örnekleri olarak modellenir <xref:System.ServiceModel.Channels.AddressHeader> .

Bir hizmet için uç nokta adresi, kod kullanılarak veya bildirimli olarak yapılandırma yoluyla imperatively belirtilebilir. Dağıtılmış bir hizmetin bağlamaları ve adresleri genellikle hizmet geliştirildiğinde kullanılanlardan farklı olduğundan, koddaki uç noktaların tanımlanması genellikle pratik değildir. Genellikle, kod yerine yapılandırma kullanarak hizmet uç noktaları tanımlamak daha pratik bir yapılandırmadır. Bağlama ve adresleme bilgilerinin koddan tutulması, uygulamayı yeniden derlemek ve yeniden dağıtmak zorunda kalmadan değiştirilmesine izin verir. Kodda veya yapılandırmada hiçbir uç nokta belirtilmemişse, çalışma zamanı, hizmet tarafından uygulanan her bir sözleşmenin her bir temel adresine bir varsayılan uç nokta ekler.

WCF 'de bir hizmet için uç nokta adreslerini belirtmenin iki yolu vardır. Hizmetle ilişkili her bir uç nokta için mutlak bir adres belirtebilir veya bir hizmetin temel adresini belirtebilir <xref:System.ServiceModel.ServiceHost> ve bu hizmetle ilişkili olan ve bu temel adrese göre tanımlanan her bir uç nokta için bir adres belirtebilirsiniz. Yapılandırma veya koddaki bir hizmetin uç nokta adreslerini belirtmek için bu yordamların her birini kullanabilirsiniz. Göreli bir adres belirtmezseniz, hizmet temel adresi kullanır. Ayrıca, bir hizmet için birden fazla temel adrese sahip olabilirsiniz, ancak her bir hizmet için her bir aktarım için yalnızca bir temel adrese izin verilir. Her biri farklı bir bağlama ile yapılandırılan birden fazla uç noktalarınız varsa, bunların adresleri benzersiz olmalıdır. Aynı bağlamayı kullanan uç noktalar, ancak farklı sözleşmeler aynı adresi kullanabilir.

IIS ile barındırırken, <xref:System.ServiceModel.ServiceHost> örneği kendiniz yönetmeyin. Temel adres, IIS 'de barındırırken hizmetin. svc dosyasında her zaman belirtilen adrestir. Bu nedenle, IIS tarafından barındırılan hizmet uç noktaları için göreli uç nokta adreslerini kullanmanız gerekir. Tam bir uç nokta adresi sağlamak, hizmetin dağıtımındaki hatalara neden olabilir. Daha fazla bilgi için bkz. [Internet Information Services barındırılan BIR WCF hizmetini dağıtma](./feature-details/deploying-an-internet-information-services-hosted-wcf-service.md).

## <a name="defining-endpoint-addresses-in-configuration"></a>Yapılandırmada uç nokta adreslerini tanımlama

Bir yapılandırma dosyasında bir uç nokta tanımlamak için [\<endpoint>](../configure-apps/file-schema/wcf/endpoint-element.md) öğesini kullanın.

[!code-xml[S_UEHelloWorld#5](./snippets/specifying-an-endpoint-address/serviceapp2.config#5)]

<xref:System.ServiceModel.Channels.CommunicationObject.Open%2A>Yöntemi çağrıldığında (yani, barındırma uygulaması hizmeti başlatmaya çalıştığında), sistem [\<service>](../configure-apps/file-schema/wcf/service.md) "UE" belirten bir ad özniteliği olan bir öğe arar. Samples. HelloService ". [\<service>](../configure-apps/file-schema/wcf/service.md)Öğe bulunursa, sistem belirtilen sınıfı yükler ve yapılandırma dosyasında belirtilen uç nokta tanımlarını kullanarak bitiş noktaları oluşturur. Bu mekanizma, koddan bağlama ve adresleme bilgilerini korurken iki satırlık kodla bir hizmeti yükleyip başlatabilmeniz için izin verir. Bu yaklaşımın avantajı, bu değişikliklerin uygulamayı yeniden derlemek veya yeniden dağıtmak zorunda kalmadan yapılabilmesine neden olur.

İsteğe bağlı üstbilgiler bir içinde bildirilmiştir [\<headers>](../configure-apps/file-schema/wcf/headers-element.md) . Aşağıda iki üst bilgi arasında ayrım yapan bir yapılandırma dosyasındaki bir hizmetin uç noktalarını belirtmek için kullanılan öğelerin bir örneği verilmiştir: "Gold" istemcileri, `http://tempuri1.org/` ve "standart" istemcilerinden `http://tempuri2.org/` . Bu hizmeti çağıran istemcinin yapılandırma dosyasında uygun olması gerekir [\<headers>](../configure-apps/file-schema/wcf/headers-element.md) .

[!code-xml[S_UEHelloWorld#1](./snippets/specifying-an-endpoint-address/serviceapp.config#1)]

Üst bilgiler, bir uç noktasındaki tüm iletiler yerine tek tek iletilerde de ayarlanabilir (daha önce gösterildiği gibi). Bu, <xref:System.ServiceModel.OperationContextScope> Aşağıdaki örnekte gösterildiği gibi, Giden iletiye özel bir üst bilgi eklemek için bir istemci uygulamasında yeni bir bağlam oluşturmak üzere kullanılarak yapılır.

[!code-csharp[OperationContextScope#4](../../../samples/snippets/csharp/VS_Snippets_CFX/operationcontextscope/cs/client.cs#4)]
[!code-vb[OperationContextScope#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/operationcontextscope/vb/client.vb#4)]

## <a name="endpoint-address-in-metadata"></a>Meta verilerde uç nokta adresi

Bir uç nokta adresi, Web Hizmetleri Açıklama Dili (WSDL) olarak `EndpointReference` karşılık gelen bitiş noktası öğesinin IÇINDE ws-Addressing (EPR) öğesi olarak temsil edilir `wsdl:port` . EPR, uç noktanın adresini ve tüm adres özelliklerini içerir. `wsdl:port`Aşağıdaki örnekte görüldüğü gibi, IÇINDEKI EPR 'nin yerini unutmayın `soap:Address` .

## <a name="defining-endpoint-addresses-in-code"></a>Koddaki Endpoint adreslerini tanımlama

Sınıf ile kodda bir uç nokta adresi oluşturulabilir <xref:System.ServiceModel.EndpointAddress> . Uç nokta adresi için belirtilen URI, tam nitelenmiş bir yol veya hizmetin temel adresine göreli bir yol olabilir. Aşağıdaki kod, sınıfının bir örneğinin nasıl oluşturulacağını <xref:System.ServiceModel.EndpointAddress> ve <xref:System.ServiceModel.ServiceHost> hizmeti barındıran örneğe nasıl ekleneceğini gösterir.

Aşağıdaki örnekte, kodda tam uç nokta adresinin nasıl belirtileceği gösterilmektedir.

[!code-csharp[S_UEHelloWorld#2](../../../samples/snippets/csharp/VS_Snippets_CFX/s_uehelloworld/cs/snippet.cs#2)]

Aşağıdaki örnek, hizmet ana bilgisayarının temel adresine göreli bir adresin ("hizmetim") nasıl ekleneceğini gösterir.

[!code-csharp[S_UEHelloWorld#3](../../../samples/snippets/csharp/VS_Snippets_CFX/s_uehelloworld/cs/snippet.cs#3)]

> [!NOTE]
> Hizmet uygulamasındaki öğesinin özellikleri, <xref:System.ServiceModel.Description.ServiceDescription> <xref:System.ServiceModel.Channels.CommunicationObject.OnOpening%2A> üzerindeki yöntemine sonradan değiştirilmemelidir <xref:System.ServiceModel.ServiceHostBase> . Özelliği ve içindeki yöntemleri gibi bazı üyeler, <xref:System.ServiceModel.ServiceHostBase.Credentials%2A> `AddServiceEndpoint` <xref:System.ServiceModel.ServiceHostBase> <xref:System.ServiceModel.ServiceHost> Bu noktadan sonra değiştirilirse bir özel durum oluşturur. Diğerleri bunları değiştirmenize izin verir, ancak sonuç tanımsızdır.
>
> Benzer şekilde, istemcide, <xref:System.ServiceModel.Description.ServiceEndpoint> üzerine çağrısından sonra değerler değiştirilmemelidir <xref:System.ServiceModel.Channels.CommunicationObject.OnOpening%2A> <xref:System.ServiceModel.ChannelFactory> . <xref:System.ServiceModel.ChannelFactory.Credentials%2A>Bu noktadan sonra değiştirilirse özelliği bir özel durum oluşturur. Diğer istemci açıklama değerleri hata olmadan değiştirilebilir, ancak sonuç tanımsızdır.
>
> Hizmet veya istemci için, çağrılmadan önce açıklamayı değiştirmeniz önerilir <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> .

## <a name="using-default-endpoints"></a>Varsayılan uç noktaları kullanma

Kodda veya yapılandırmada hiçbir uç nokta belirtilmemişse, çalışma zamanı, hizmet tarafından uygulanan her bir hizmet sözleşmesinin her bir temel adresine bir varsayılan uç nokta ekleyerek varsayılan uç noktaları sağlar. Temel adres kodda veya yapılandırmada belirtilebilir ve varsayılan uç noktalar <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> , üzerinde çağrıldığında eklenir <xref:System.ServiceModel.ServiceHost> .

Uç noktalar açık olarak sağlanmışsa, çağrılmadan önce ' de çağırarak varsayılan uç noktalar eklenebilir <xref:System.ServiceModel.ServiceHostBase.AddDefaultEndpoints%2A> <xref:System.ServiceModel.ServiceHost> <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> . Varsayılan uç noktalar, bağlamalar ve davranışları hakkında daha fazla bilgi için bkz. [WCF Hizmetleri Için](./samples/simplified-configuration-for-wcf-services.md) [Basitleştirilmiş yapılandırma](simplified-configuration.md) ve Basitleştirilmiş yapılandırma.

## <a name="see-also"></a>Ayrıca bkz.

- <xref:System.ServiceModel.EndpointAddress>
- [Kimlik Doğrulama ile Hizmet Kimliği](./feature-details/service-identity-and-authentication.md)
- [Uç Noktası Oluşturma Genel Bakış](endpoint-creation-overview.md)
- [Hosting](./feature-details/hosting.md)
