---
title: 'Nasıl yapılır: Özel Bir WS-Metadata Değişimi Bağlaması Yapılandırma'
ms.date: 03/30/2017
helpviewer_keywords:
- WS-Metadata Exchange [WCF]
- WS-Metadata Exchange [WCF], configuring a custom binding
ms.assetid: cdba4d73-da64-4805-bc56-9822becfd1e4
ms.openlocfilehash: 6459e3f0cf0ab72af8027bd6802a0e7aa574aece
ms.sourcegitcommit: 1c1a1f9ec0bd1efb3040d86a79f7ee94e207cca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80635792"
---
# <a name="how-to-configure-a-custom-ws-metadata-exchange-binding"></a>Nasıl yapılır: Özel Bir WS-Metadata Değişimi Bağlaması Yapılandırma

Bu makalede, özel bir WS-Meta veri değişimi bağlama yapılandırmak için nasıl açıklanmaktadır. Windows Communication Foundation (WCF) dört sistem tanımlı meta veri bağlaması içerir, ancak meta verileri istediğiniz bağlamayı kullanarak yayımlayabilirsiniz. Bu makalede, meta verileri kullanarak `wsHttpBinding`nasıl yayımlayacağınızı gösterir. Bu bağlama, meta verileri güvenli bir şekilde açığa çıkarma seçeneği sunar. Bu makaledeki kod [Başlarken](../samples/getting-started-sample.md)dayanmaktadır.  
  
### <a name="using-a-configuration-file"></a>Yapılandırma dosyası kullanma  
  
1. Hizmetin yapılandırma dosyasında, `serviceMetadata` etiketi içeren bir hizmet davranışı ekleyin:  
  
    ```xml  
    <behaviors>  
       <serviceBehaviors>  
         <behavior name="CalculatorServiceBehavior">  
           <serviceMetadata httpGetEnabled="True"/>  
         </behavior>  
       </serviceBehaviors>  
    </behaviors>  
    ```  
  
2. Bu `behaviorConfiguration` yeni davranışa başvuran hizmet etiketine bir öznitelik ekleyin:  
  
    ```xml  
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
    behaviorConfiguration="CalculatorServiceBehavior" />
    ```  
  
3. Mex'i adres olarak, `wsHttpBinding` bağlama olarak ve <xref:System.ServiceModel.Description.IMetadataExchange> sözleşme olarak belirten bir meta veri bitiş noktası ekleyin:  
  
    ```xml  
    <endpoint address="mex"  
              binding="wsHttpBinding"  
              contract="IMetadataExchange" />  
    ```  
  
4. Meta veri değişimi bitiş noktasının doğru çalıştığını doğrulamak için istemci yapılandırma dosyasına bir uç nokta etiketi ekleyin:  
  
    ```xml  
    <endpoint name="MyMexEndpoint"               address="http://localhost:8000/servicemodelsamples/service/mex"  
              binding="wsHttpBinding"  
              contract="IMetadataExchange"/>  
    ```  
  
5. İstemcinin Main() yönteminde, <xref:System.ServiceModel.Description.MetadataExchangeClient> yeni bir <xref:System.ServiceModel.Description.MetadataExchangeClient.ResolveMetadataReferences%2A> örnek `true`oluşturun, <xref:System.ServiceModel.Description.MetadataExchangeClient.GetMetadata%2A> özelliğini döndürülen meta verilerin toplanması yoluyla , aramak ve sonra yinelemek için ayarlayın:  
  
    ```csharp
    string mexAddress = "http://localhost:8000/servicemodelsamples/service/mex";  
  
    MetadataExchangeClient mexClient = new MetadataExchangeClient("MyMexEndpoint");  
    mexClient.ResolveMetadataReferences = true;  
    MetadataSet mdSet = mexClient.GetMetadata(new EndpointAddress(mexAddress));  
    foreach (MetadataSection section in mdSet.MetadataSections)  
    Console.WriteLine("Metadata section: " + section.Dialect.ToString());  
    ```  
  
### <a name="configuring-by-code"></a>Koda göre yapılandırma  
  
1. Bağlayıcı <xref:System.ServiceModel.WSHttpBinding> bir örnek oluşturun:  
  
    ```csharp  
    WSHttpBinding binding = new WSHttpBinding();  
    ```  
  
2. Bir <xref:System.ServiceModel.ServiceHost> örnek oluşturun:  
  
    ```csharp  
    ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService), baseAddress);  
    ```  
  
3. Hizmet bitiş noktası ekleyin <xref:System.ServiceModel.Description.ServiceMetadataBehavior> ve bir örnek ekleyin:  
  
    ```csharp  
    serviceHost.AddServiceEndpoint(typeof(ICalculator), binding, baseAddress);  
    ServiceMetadataBehavior smb = new ServiceMetadataBehavior();  
    smb.HttpGetEnabled = true;  
    serviceHost.Description.Behaviors.Add(smb);  
    ```  
  
4. Daha önce <xref:System.ServiceModel.WSHttpBinding> oluşturulan belirterek bir meta veri değişimi bitiş noktası ekleyin:  
  
    ```csharp  
    serviceHost.AddServiceEndpoint(typeof(IMetadataExchange), binding, mexAddress);  
    ```  
  
5. Meta veri alışverişi bitiş noktasının doğru çalıştığını doğrulamak için istemci yapılandırma dosyasına bir uç nokta etiketi ekleyin:  
  
    ```xml  
    <endpoint name="MyMexEndpoint"               address="http://localhost:8000/servicemodelsamples/service/mex"  
              binding="wsHttpBinding"  
              contract="IMetadataExchange"/>  
    ```  
  
6. İstemcinin Main() yönteminde, <xref:System.ServiceModel.Description.MetadataExchangeClient> yeni bir <xref:System.ServiceModel.Description.MetadataExchangeClient.ResolveMetadataReferences%2A> örnek `true`oluşturun, <xref:System.ServiceModel.Description.MetadataExchangeClient.GetMetadata%2A> özelliği , arama ve döndürülen meta verilerin toplanması yoluyla yinelemek için ayarlayın:  
  
    ```csharp  
    string mexAddress = "http://localhost:8000/servicemodelsamples/service/mex";  
  
    MetadataExchangeClient mexClient = new MetadataExchangeClient("MyMexEndpoint");  
    mexClient.ResolveMetadataReferences = true;  
    MetadataSet mdSet = mexClient.GetMetadata(new EndpointAddress(mexAddress));  
    foreach (MetadataSection section in mdSet.MetadataSections)  
    Console.WriteLine("Metadata section: " + section.Dialect.ToString());  
    ```  
  
## <a name="see-also"></a>Ayrıca bkz.

- [Meta Veri Yayımlama Davranışı](../samples/metadata-publishing-behavior.md)
- [Meta Verileri Alma](../samples/retrieve-metadata.md)
- [Meta veriler](../feature-details/metadata.md)
- [Meta Verileri Yayımlama](../feature-details/publishing-metadata.md)
- [Meta Veri Uç Noktalarını Yayımlama](../publishing-metadata-endpoints.md)
