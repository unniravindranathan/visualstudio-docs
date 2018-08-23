---
title: "FRAMEINFO | Microsoft Docs"
ms.custom: ""
ms.date: "2018-06-30"
ms.prod: "visual-studio-dev14"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "vs-ide-sdk"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "FRAMEINFO"
helpviewer_keywords: 
  - "FRAMEINFO structure"
ms.assetid: 95001b89-dddb-45bb-889d-8327994e38a5
caps.latest.revision: 9
ms.author: "gregvanl"
manager: "ghogen"
---
# FRAMEINFO
[!INCLUDE[vs2017banner](../../../includes/vs2017banner.md)]

The latest version of this topic can be found at [FRAMEINFO](https://docs.microsoft.com/visualstudio/extensibility/debugger/reference/frameinfo).  
  
Describes a stack frame.  
  
## Syntax  
  
```cpp#  
typedef struct tagFRAMEINFO {   
   FRAMEINFO_FLAGS    m_dwValidFields;  
   BSTR               m_bstrFuncName;  
   BSTR               m_bstrReturnType;  
   BSTR               m_bstrArgs;  
   BSTR               m_bstrLanguage;  
   BSTR               m_bstrModule;  
   UINT64             m_addrMin;  
   UINT64             m_addrMax;  
   IDebugStackFrame2* m_pFrame;  
   IDebugModule2*     m_pModule;  
   BOOL               m_fHasDebugInfo;  
   BOOL               m_fStaleCode;  
   BOOL               m_fAnnotatedFrame;  
} FRAMEINFO;  
```  
  
```csharp  
public struct FRAMEINFO {   
   public uint              m_dwValidFields;  
   public string            m_bstrFuncName;  
   public string            m_bstrReturnType;  
   public string            m_bstrArgs;  
   public string            m_bstrLanguage;  
   public string            m_bstrModule;  
   public ulong             m_addrMin;  
   public ulong             m_addrMax;  
   public IDebugStackFrame2 m_pFrame;  
   public IDebugModule2     m_pModule;  
   public int               m_fHasDebugInfo;  
   public int               m_fStaleCode;  
   public int               m_fAnnotatedFrame;  
} FRAMEINFO;  
```  
  
## Members  
 m_dwValidFields  
 A combination of flags from the [FRAMEINFO_FLAGS](../../../extensibility/debugger/reference/frameinfo-flags.md) enumeration that specifies which fields are filled in.  
  
 m_bstrFuncName  
 The function name associated with the stack frame.  
  
 m_bstrReturnType  
 The return type associated with the stack frame.  
  
 m_bstrArgs  
 The arguments to the function associated with the stack frame.  
  
 m_bstrLanguage  
 The language in which the function is implemented.  
  
 m_bstrModule  
 The module name associated with the stack frame.  
  
 m_addrMin  
 The minimum physical stack address.  
  
 m_addrMAX  
 The maximum physical stack address.  
  
 m_pFrame  
 The [IDebugStackFrame2](../../../extensibility/debugger/reference/idebugstackframe2.md) object that represents this stack frame.  
  
 m_pFrame  
 The [IDebugModule2](../../../extensibility/debugger/reference/idebugmodule2.md) object that represents the module that contains this stack frame.  
  
 m_fHasDebugInfo  
 Non-zero (`TRUE`) if debug information exists in the given frame.  
  
 m_fHasDebugInfo  
 Non-zero (`TRUE`) if the stack frame is associated with code that is no longer valid.  
  
 m_fHasDebugInfo  
 Non-zero (`TRUE`) if the stack frame is annotated by the session debug manager (SDM).  
  
## Remarks  
 This structure is passed to the [GetInfo](../../../extensibility/debugger/reference/idebugstackframe2-getinfo.md) method to be filled in. This structure is also contained in a list that is contained in the [IEnumDebugFrameInfo2](../../../extensibility/debugger/reference/ienumdebugframeinfo2.md) interface which, in turn, is returned from a call to the [EnumFrameInfo](../../../extensibility/debugger/reference/idebugthread2-enumframeinfo.md) method.  
  
## Requirements  
 Header: msdbg.h  
  
 Namespace: Microsoft.VisualStudio.Debugger.Interop  
  
 Assembly: Microsoft.VisualStudio.Debugger.Interop.dll  
  
## See Also  
 [Structures and Unions](../../../extensibility/debugger/reference/structures-and-unions.md)   
 [FRAMEINFO_FLAGS](../../../extensibility/debugger/reference/frameinfo-flags.md)   
 [IDebugStackFrame2](../../../extensibility/debugger/reference/idebugstackframe2.md)   
 [IDebugModule2](../../../extensibility/debugger/reference/idebugmodule2.md)   
 [GetInfo](../../../extensibility/debugger/reference/idebugstackframe2-getinfo.md)   
 [IEnumDebugFrameInfo2](../../../extensibility/debugger/reference/ienumdebugframeinfo2.md)   
 [EnumFrameInfo](../../../extensibility/debugger/reference/idebugthread2-enumframeinfo.md)
