---
title: ICorDebugManagedCallback::UpdateModuleSymbols Yöntemi
ms.date: 03/30/2017
api_name:
- ICorDebugManagedCallback.UpdateModuleSymbols
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugManagedCallback::UpdateModuleSymbols
helpviewer_keywords:
- UpdateModuleSymbols method [.NET Framework debugging]
- ICorDebugManagedCallback::UpdateModuleSymbols method [.NET Framework debugging]
ms.assetid: 0863f644-58e8-45a0-b0c3-a28e99b20938
topic_type:
- apiref
ms.openlocfilehash: c0381cf924e44e581c8b275c9750cacba045cf1b
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501787"
---
# <a name="icordebugmanagedcallbackupdatemodulesymbols-method"></a>ICorDebugManagedCallback::UpdateModuleSymbols Yöntemi
Hata ayıklayıcısını, ortak dil çalışma zamanı modülü simgelerinin değiştiği hakkında bilgilendirir.  
  
## <a name="syntax"></a>Söz dizimi  
  
```cpp  
HRESULT UpdateModuleSymbols (  
    [in] ICorDebugAppDomain *pAppDomain,  
    [in] ICorDebugModule    *pModule,  
    [in] IStream            *pSymbolStream  
);  
```  
  
## <a name="parameters"></a>Parametreler  
 `pAppDomain`  
 'ndaki Simgenin değiştiği modülün bulunduğu uygulama etki alanını temsil eden ICorDebugAppDomain nesnesine yönelik bir işaretçi.  
  
 `pModule`  
 'ndaki Simgelerin değiştiği modülü temsil eden ICorDebugModule nesnesine yönelik bir işaretçi.  
  
 `pSymbolStream`  
 'ndaki Değiştirilen sembolleri içeren bir Win32 COM `IStream` nesnesi işaretçisi.  
  
## <a name="remarks"></a>Açıklamalar  
 Bu yöntem, [ıvmunmanagedreader:: UpdateSymbolStore](../diagnostics/isymunmanagedreader-updatesymbolstore-method.md) veya [ıımunmanagedreader:: Synccesymbolstore](../diagnostics/isymunmanagedreader-replacesymbolstore-method.md)çağırarak hata ayıklayıcının bir modülün sembol görünümünü güncelleştirme fırsatı sağlar.  
  
 Bu geri çağırma aynı modül için birden çok kez gerçekleşebilir.  
  
 Bir hata ayıklayıcı, ilişkisiz kaynak düzeyinde kesme noktaları bağlamayı denemelidir.  
  
## <a name="requirements"></a>Gereksinimler  
 **Platformlar:** Bkz. [sistem gereksinimleri](../../get-started/system-requirements.md).  
  
 **Üst bilgi:** CorDebug. IDL, CorDebug. h  
  
 **Kitaplık:** Corguid. lib  
  
 **.NET Framework sürümleri:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Ayrıca bkz.

- [ICorDebugManagedCallback Arabirimi](icordebugmanagedcallback-interface.md)
