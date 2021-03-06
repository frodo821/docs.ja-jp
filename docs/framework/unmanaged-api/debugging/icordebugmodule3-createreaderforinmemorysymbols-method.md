---
title: ICorDebugModule3::CreateReaderForInMemorySymbols メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugModule3.CreateReaderForInMemorySymbols
api_location:
- CorDebug.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule3::CreateReaderForInMemorySymbols
helpviewer_keywords:
- CreateReaderForInMemorySymbols method, ICorDebugModule3 interface [.NET Framework debugging]
- ICorDebugModule3::CreateReaderForInMemorySymbols method [.NET Framework debugging]
ms.assetid: af317171-d66d-4114-89eb-063554c74940
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 76f7b53f800bc8c5c23f49a0781287a38bf8c959
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
---
# <a name="icordebugmodule3createreaderforinmemorysymbols-method"></a>ICorDebugModule3::CreateReaderForInMemorySymbols メソッド
動的モジュールのデバッグ シンボル リーダーを作成します。  
  
## <a name="syntax"></a>構文  
  
```  
HRESULT CreateReaderForInMemorySymbols (  
      [in] REFIID riid,  
      [out][iid_is(riid)] void **    ppObj  
```  
  
#### <a name="parameters"></a>パラメーター  
 riid  
 [in]返す COM インターフェイスの IID です。 通常、これは、 [ISymUnmanagedReader インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-interface.md)です。  
  
 ppObj  
 [out]返されたインターフェイスへのポインターへのポインター。  
  
## <a name="return-value"></a>戻り値  
 S_OK  
 リーダーが正常に作成します。  
  
 CORDBG_E_MODULE_LOADED_FROM_DISK  
 モジュールは、メモリ内または動的モジュールではありません。  
  
 CORDBG_E_SYMBOLS_NOT_AVAILABLE  
 シンボルは、アプリケーションによって提供されていないまたはまだ利用できません。  
  
 E_FAIL (またはその他の E_ リターン コード)  
 リーダーを作成できません。  
  
## <a name="remarks"></a>コメント  
 このメソッドも、メモリ内の (動的ではない) モジュールのシンボル リーダー オブジェクトを作成するために使用されるのみするシンボルが利用可能にした後 (によって示される、 [UpdateModuleSymbols メソッド](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-updatemodulesymbols-method.md)コールバック)。  
  
 このメソッドが呼び出されるたびに、新しいリーダー インスタンスを返します (と同様に[CComPtrBase::CoCreateInstance](http://msdn.microsoft.com/library/c0965041-6cb6-40c5-b272-2b99f02668a6))。 そのため、デバッガーが結果をキャッシュして、基になるデータが変更された場合にのみ、新しいインスタンスを要求する必要があります (つまり場合、 [LoadClass メソッド](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-loadclass-method.md)コールバックを受信した)。  
  
 最初の型が読み込まれるまで動的モジュールは使用可能なすべてのシンボルはありません (によって示される、 [LoadClass メソッド](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-loadclass-method.md)コールバック)。  
  
## <a name="requirements"></a>要件  
 **プラットフォーム:** を参照してください[システム要件](../../../../docs/framework/get-started/system-requirements.md)です。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET framework のバージョン:** 4.5、4、3.5 SP1  
  
## <a name="see-also"></a>関連項目  
 [ICorDebugRemoteTarget インターフェイス](../../../../docs/framework/unmanaged-api/debugging/icordebugremotetarget-interface.md)  
 [ICorDebug インターフェイス](../../../../docs/framework/unmanaged-api/debugging/icordebug-interface.md)  
    
 [デバッグ インターフェイス](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)
