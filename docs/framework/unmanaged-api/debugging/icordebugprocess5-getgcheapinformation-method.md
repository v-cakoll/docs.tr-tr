---
title: ICorDebugProcess5::GetGCHeapInformation Metodu
ms.date: 03/30/2017
api_name:
- ICorDebugProcess5.GetGCHeapInformation
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess5::GetGCHeapInformation
helpviewer_keywords:
- ICorDebugProcess5::GetGCHeapInformation method [.NET Framework debugging]
- GetGCHeapInformation method, ICorDebugProcess5 interface [.NET Framework debugging]
ms.assetid: b9538ceb-230a-4079-9cb2-903dbf5c1848
topic_type:
- apiref
ms.openlocfilehash: 62d45da44a95eae399fbbd287aa997a5f913d0b0
ms.sourcegitcommit: 488aced39b5f374bc0a139a4993616a54d15baf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83209662"
---
# <a name="icordebugprocess5getgcheapinformation-method"></a>ICorDebugProcess5::GetGCHeapInformation Metodu
Çöp toplama yığını hakkında şu anda numaralandırılabilir olup olmadığı dahil genel bilgiler sağlar.  
  
## <a name="syntax"></a>Sözdizimi  
  
```cpp  
HRESULT GetGCHeapInformation(  
    [out] COR_HEAPINFO *pHeapInfo  
);  
```  
  
## <a name="parameters"></a>Parametreler  
 `pHeapInfo`  
 dışı Çöp toplama yığını hakkında genel bilgi sağlayan [COR_HEAPINFO](cor-heapinfo-structure.md) değerine yönelik bir işaretçi.  
  
## <a name="remarks"></a>Açıklamalar  
 `ICorDebugProcess5::GetGCHeapInformation`İşlemdeki çöp toplama yapılarının Şu anda geçerli olduğundan emin olmak için, yığın veya ayrı yığın bölgelerini numaralandırmadan önce yöntemi çağrılmalıdır. Atık toplama yığını, bir koleksiyon devam ederken eklenebilir olamaz. Aksi takdirde, numaralandırma geçersiz çöp toplama yapılarını yakalayabilir.  
  
## <a name="requirements"></a>Gereksinimler  
 **Platformlar:** Bkz. [sistem gereksinimleri](../../get-started/system-requirements.md).  
  
 **Üst bilgi:** CorDebug. IDL, CorDebug. h  
  
 **Kitaplık:** Corguid. lib  
  
 **.NET Framework sürümleri:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Ayrıca bkz.

- [ICorDebugProcess5 Arabirimi](icordebugprocess5-interface.md)
- [Hata Ayıklama Arabirimleri](debugging-interfaces.md)
