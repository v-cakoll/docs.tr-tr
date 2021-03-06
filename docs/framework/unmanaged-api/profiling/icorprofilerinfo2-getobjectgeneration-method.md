---
title: ICorProfilerInfo2::GetObjectGeneration Yöntemi
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetObjectGeneration
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetObjectGeneration
helpviewer_keywords:
- GetObjectGeneration method [.NET Framework profiling]
- ICorProfilerInfo2::GetObjectGeneration method [.NET Framework profiling]
ms.assetid: b0d25f76-0bd5-4aa6-96cf-bfec0e1de28b
topic_type:
- apiref
ms.openlocfilehash: 1263202c1fe524c924a88b9356e5ab9116cea553
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502866"
---
# <a name="icorprofilerinfo2getobjectgeneration-method"></a>ICorProfilerInfo2::GetObjectGeneration Yöntemi
Belirtilen nesneyi içeren yığının segmentini alır.  
  
## <a name="syntax"></a>Söz dizimi  
  
```cpp  
HRESULT GetObjectGeneration(  
    [in] ObjectID objectId,  
    [out] COR_PRF_GC_GENERATION_RANGE *range);  
```  
  
## <a name="parameters"></a>Parametreler  
 `objectId`  
 'ndaki Nesnenin KIMLIĞI.  
  
 `range`  
 dışı Atık toplama yapılmakta olan neslin içindeki bir bellek aralığını (yani bir blok) açıklayan [COR_PRF_GC_GENERATION_RANGE](cor-prf-gc-generation-range-structure.md) yapısına yönelik bir işaretçi. Bu Aralık belirtilen nesneyi içerir.  
  
## <a name="remarks"></a>Açıklamalar  
 `GetObjectGeneration`Çöp toplama işlemi devam ettiğinden, yöntemi herhangi bir profil oluşturucu geri çağrısından çağrılabilir. Diğer bir deyişle, [ICorProfilerCallback2:: GarbageCollectionStarted](icorprofilercallback2-garbagecollectionstarted-method.md) ve [ICorProfilerCallback2:: GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)arasında gerçekleşenler hariç herhangi bir geri aramadan çağrılabilir.  
  
## <a name="requirements"></a>Gereksinimler  
 **Platformlar:** Bkz. [sistem gereksinimleri](../../get-started/system-requirements.md).  
  
 **Üst bilgi:** CorProf. IDL, CorProf. h  
  
 **Kitaplık:** Corguid. lib  
  
 **.NET Framework sürümleri:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Ayrıca bkz.

- [ICorProfilerInfo Arabirimi](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 Arabirimi](icorprofilerinfo2-interface.md)
