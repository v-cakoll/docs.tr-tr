---
title: Azure İzleyici
description: Sisteminizin üzerinde görünürlük elde etmek için Azure Izleyici 'yi kullanma.
ms.date: 05/13/2020
ms.openlocfilehash: e3ff673c63ecbc380cb8b74ae54065a091882d7b
ms.sourcegitcommit: 27db07ffb26f76912feefba7b884313547410db5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83614272"
---
# <a name="azure-monitor"></a>Azure İzleyici

Azure 'da bulunan bir bulut uygulaması izleme çözümünün dışında başka bir bulut sağlayıcısı yoktur. Azure Izleyici, sisteminizin durumunun görünürlüğünü, herhangi bir sorun hakkında öngörüleri ve uygulamanızın iyileştirmesini sağlamak için tasarlanan bir araç koleksiyonu için şemsiye bir addır.

![Azure Izleyici, bulut Yerel uygulamasının nasıl çalıştığını kavramak için araçlara yönelik bir koleksiyon. ](./media/azure-monitor.png)
 **Şekil 7-12**. Azure Izleyici, bulut Yerel uygulamasının nasıl çalıştığını kavramak için araçlara yönelik bir koleksiyon.

## <a name="gathering-logs-and-metrics"></a>Günlükleri ve ölçümleri toplama

Herhangi bir izleme çözümünde ilk adım mümkün olduğunca çok veri toplamaktır. Daha fazla veri toplandıktan sonra, elde edilen öngörülere daha derin erişebilirsiniz. Aletsiz sistemler, geleneksel olarak güçleşir. Basit Ağ Yönetim Protokolü (SNMP), makine düzeyinde bilgi toplamak için kullanılan altın standart protokoldür, ancak çok fazla bilgi ve yapılandırma gerektirdi. Neyse ki, en yaygın ölçümler Azure Izleyici tarafından otomatik olarak toplandığından, bu sabit çalışmanın çoğu ortadan kaldırılmıştır.

Dağıtılan uygulamaya özgü olduklarından, uygulama düzeyi ölçümleri ve olayları otomatik olarak işaretleme yapılamaz. Bu ölçümleri toplamak için, bir müşterinin bir siparişi kaydolduğunda veya tamamladığı durumlarda olduğu gibi, bu bilgileri doğrudan raporlamak için [kullanılabilir SDK 'lar ve API 'ler](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics) vardır. Ayrıca, özel durumlar Application Insights aracılığıyla Azure Izleyici 'ye geri yakalanıp rapor edilebilir. SDK 'lar, Go, Python, JavaScript ve .NET dillerini de içeren bulut Yerel uygulamalarında bulunan her dilin çoğunu destekler.

Uygulamanızın durumu hakkında bilgi toplamanın nihai hedefi, son kullanıcılarınızın iyi bir deneyim aldığından emin olmaktır. Kullanıcıların, [dışarıdaki Web testlerini](https://docs.microsoft.com/azure/azure-monitor/app/monitor-web-app-availability)yapmaktan sorun yaşadığını söylemek için daha iyi bir yol var mı? Bu testler, dünyanın dört bir yanındaki konumlardan veya aracılardan biri sitede oturum açıp Kullanıcı eylemlerinin benzetimini yaparak Web sitenizde ping hale kadar basit olabilir.

## <a name="reporting-data"></a>Raporlama verileri

Veriler toplandıktan sonra, kullanıcıların sorun olduğunda anında görmesine izin veren grafikler üzerinde değiştirilebilir, özetlenir ve çizilebilirler. Bu grafikler, sistemin bazı yönlerini anlatmak üzere tasarlanan çok sayfalı bir rapor olan panolar veya çalışma kitaplarında toplanabilir.

Yapay zeka veya Machine Learning olmadan modern uygulama tamamlanmaz. Bu uçta, veriler başka bir şekilde gizlenebileceği eğilimleri ve bilgileri ayıklamanızı sağlamak için Azure 'daki çeşitli makine öğrenimi araçlarına veri [geçirilebilir](https://www.youtube.com/watch?v=Cuza-I1g9tw) .

Application Insights, kayıtları bulmak, özetlemek ve hatta grafikleri çizmek için kullanılabilecek kusto adlı güçlü bir sorgu dili sağlar. Örneğin, bu sorgu, 2007 Kasım ayının tüm kayıtlarını bulur, bunları durumlarına göre gruplayın ve bir pasta grafiği olarak ilk 10 ' u çizmez.

```kusto
StormEvents
| where StartTime >= datetime(2007-11-01) and StartTime < datetime(2007-12-01)
| summarize count() by State
| top 10 by count_
| render piechart
```

![](./media/azure-monitor.png)
**Şekil 7-13**Application Insights sorgu sonucu. Application Insights sorgusunun sonucu.

[Kusto sorguları denemek için bir oyun](https://dataexplorer.azure.com/clusters/help/databases/Samples) vardır. Bu, bir saat veya iki harcayabileceğiniz harika bir yerdir. [Örnek sorguları](https://docs.microsoft.com/azure/kusto/query/samples) okuma da çalıştırılabilir.

## <a name="dashboards"></a>Panolar

Azure Izleyici 'den bilgileri yüzeye almak için kullanılabilecek çeşitli farklı Pano teknolojileri vardır. Belki de en basit, Application Insights sorguları çalıştırmak ve [verileri bir grafiğe çizmeniz](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-app-dashboards)olabilir.

![Ana Azure panosu ](./media/azure-monitor.png)
 **Şekil 7-14**' de gömülü Application Insights grafiklerin bir örneği. Ana Azure panosuna katıştırılmış Application Insights grafik örneği.

Bu grafikler daha sonra Pano özelliğinin kullanımıyla Azure portal uygun şekilde gömülebilir. Çeşitli veri katmanlarında ayrıntıya gidebilmek gibi daha fazla exacting gereksinimi olan kullanıcılar için Azure Izleyici verileri [Power BI](https://powerbi.microsoft.com/)kullanılabilir. Power BI, birçok farklı veri kaynağından veri toplayabilen sektör lideri, kurumsal sınıf, iş zekası aracıdır.

![Dashboard ](./media/azure-monitor.png)
 **şekil 7-15**Power BI bir örnektir. Pano Power BI bir örnek.

## <a name="alerts"></a>Uyarılar

Bazen veri panoları yeterli değildir. Panoları izlemek için kimse uyanık yoksa, bir sorun giderilebilmesi veya hatta algılanmadan önce bu süre çok saat kadar sürebilir. Azure Izleyici, bu uçta bir üst düzey [Uyarı çözümü](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)de sağlar. Uyarılar aşağıdakiler dahil olmak üzere çok çeşitli koşullarla tetiklenebilir:

- Ölçüm değerleri
- Günlük arama sorguları
- Etkinlik günlüğü olayları
- Temel alınan Azure platformunun durumu
- Web sitesi kullanılabilirliği için testler

Tetiklendiğinde, uyarılar çok çeşitli görevler gerçekleştirebilir. Basit tarafta, uyarılar yalnızca bir posta listesine e-posta bildirimi veya bir kişiye metin iletisi gönderebilir. Daha fazla ilgili uyarı, belirli bir uygulama için kimin çağırdığının farkında olan Pagerharcı gibi bir araçta iş akışını tetikleyebiliyor. Uyarılar, iş akışları için sınırsız olasılıklardan [Microsoft Flow](https://flow.microsoft.com/) kilit açma sırasında eylemleri tetikleyebilir.

Uyarıların yaygın nedenleri tanımlandıkları için uyarılar, uyarıların genel nedenleri ve bunları çözmek için gereken adımlar hakkında ayrıntılarla iyileştirilen şekilde geliştirilebilir. Yüksek derecede çok büyük ölçekli bulut Yerel uygulama dağıtımları, hatalı düğümleri bir ölçek kümesinden kaldırma veya otomatik ölçeklendirme etkinliğini tetikleme gibi eylemleri gerçekleştiren kendi kendini onaran görevleri başlatabilir. Son olarak, sistem kendisini bir sonraki sabah işe ulaşana kadar bir süre önce veya en az bir şekilde ayarlayacağından, bir canlı site sorununu çözmek için, daha önce 2.

Azure Izleyici, dağıtılan uygulamaların normal işletim parametrelerini anlamak için makine öğrenimini otomatik olarak kullanır. Bu, BT 'nin normal parametrelerinin dışında çalışan hizmetleri algılamasını sağlar. Örneğin, sitedeki tipik iş günü trafiği dakikada 10.000 istek olabilir. Daha sonra, belirli bir hafta boyunca istek sayısı, dakikada yüksek olağandışı 20.000 istek üzerine gelir. [Akıllı algılama](https://docs.microsoft.com/azure/azure-monitor/app/proactive-diagnostics) , norm karşı bu sapmayı fark eder ve bir uyarı tetikler. Aynı zamanda, trafik yükü beklendiğinde hatalı pozitif sonuçlar tetiklemeden kaçınmak için eğilim analizi yeterince akıllı olur.

## <a name="references"></a>Başvurular

- [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview)

>[!div class="step-by-step"]
>[Önceki](monitoring-azure-kubernetes.md) 
> [Sonraki](identity.md)
