---
title: HOST_TYPE Numaralandırması
ms.date: 03/30/2017
api_name:
- HOST_TYPE
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- HOST_TYPE
helpviewer_keywords:
- HOST_TYPE enumeration [.NET Framework hosting]
ms.assetid: 51f848be-84c5-4036-9839-c762c576bbf5
topic_type:
- apiref
ms.openlocfilehash: e8dda83df8a320733f45dbcc13599cdf37d26492
ms.sourcegitcommit: 27db07ffb26f76912feefba7b884313547410db5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83617158"
---
# <a name="host_type-enumeration"></a>HOST_TYPE Numaralandırması
Bir uygulamayı başlatan ana bilgisayar türünü belirten değerleri içerir.  
  
## <a name="syntax"></a>Söz dizimi  
  
```cpp  
typedef enum {  
    HOST_TYPE_DEFAULT     = 0x0,  
    HOST_TYPE_APPLAUNCH   = 0x1,  
    HOST_TYPE_CORFLAG     = 0x2  
} HOST_TYPE;  
```  
  
## <a name="members"></a>Üyeler  
  
|Üye|Açıklama|  
|------------|-----------------|  
|`HOST_TYPE_APPLAUNCH`|Uygulamayı Applalaunch. exe ' den başlatın.<br /><br /> Kısmen güvenilen uygulamalar için bu değeri kullanın.|  
|`HOST_TYPE_CORFLAG`|Uygulamayı doğrudan başlatın. Diğer bir deyişle, uygulamayı kendi. exe dosyasından başlatın.<br /><br /> Tam güvenilir uygulamalar için bu değeri kullanın.|  
|`HOST_TYPE_DEFAULT`|HOST_TYPE_APPLAUNCH ile aynı.|  
  
## <a name="requirements"></a>Gereksinimler  
 **Platformlar:** Bkz. [sistem gereksinimleri](../../get-started/system-requirements.md).  
  
 **Üst bilgi:** MSCorEE. h  
  
 **Kitaplık:** MSCorEE. dll  
  
 **.NET Framework sürümleri:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Ayrıca bkz.

- [Barındırma Sabit Listeleri](hosting-enumerations.md)
