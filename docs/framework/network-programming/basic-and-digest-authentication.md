---
title: 基本認証とダイジェスト認証
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- authentication [.NET Framework], classes
- Basic authentication
- authentication [.NET Framework], basic
- client authentication, basic
- user authentication, basic
- authentication [.NET Framework], digest
- receiving data, authentication
- client authentication, digest
- Internet, authentication
- digest authentication
- sending data, authentication
- network resources, authentication
- user authentication, digest
ms.assetid: 8cce2742-8d52-4643-9dd2-64ddf38aa878
author: mcleblanc
ms.author: markl
manager: markl
ms.openlocfilehash: fc061065caa4dad878a2a9b45e98ecb0d419d18b
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
---
# <a name="basic-and-digest-authentication"></a>基本認証とダイジェスト認証
基本認証とダイジェスト認証の <xref:System.Net> 実装では、RFC2617 – HTTP 認証: 基本認証とダイジェスト認証 (www.w3.org の World Wide Web コンソーシアム Web サイトで使用可能) に従います。  
  
 基本認証とダイジェスト認証を使用するには、次の例に示すように、アプリケーションはインターネットからデータを要求するために使用される <xref:System.Net.WebRequest> オブジェクトの <xref:System.Net.WebRequest.Credentials%2A> プロパティでユーザー名とパスワードを指定する必要があります。  
  
```vb  
Dim MyURI As String = "http://www.contoso.com/"  
Dim WReq As WebRequest = WebRequest.Create(MyURI)  
WReq.Credentials = New NetworkCredential(UserName, SecurelyStoredPassword)  
```  
  
```csharp  
String MyURI = "http://www.contoso.com/";  
WebRequest WReq = WebRequest.Create(MyURI);  
WReq.Credentials = new NetworkCredential(UserName, SecurelyStoredPassword);  
```  
  
> [!CAUTION]
>  基本認証およびダイジェスト認証で送信されるデータは暗号化されないため、敵対者がデータを見ることができます。 また、基本認証の資格情報 (ユーザー名とパスワード) はクリア テキストで送信されるので、傍受される可能性があります。  
  
## <a name="see-also"></a>参照  
 [NTLM 認証および Kerberos 認証](../../../docs/framework/network-programming/ntlm-and-kerberos-authentication.md)  
 [インターネット認証](../../../docs/framework/network-programming/internet-authentication.md)
