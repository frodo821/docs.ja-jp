---
title: "&lt;netHttpBinding&gt; の &lt;message&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 9def5a35-475d-40d6-b716-ccdbd93863c7
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# &lt;netHttpBinding&gt; の &lt;message&gt;
[\<basicHttpBinding\>](../../../../../docs/framework/configure-apps/file-schema/wcf/basichttpbinding.md)のメッセージ レベル セキュリティの設定を定義します。  
  
## 構文  
  
```  
  
<message   
   algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"  
      clientCredentialType="UserName/Certificate"/>  
```  
  
## 属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### 属性  
  
|属性|説明|  
|--------|--------|  
|algorithmSuite|メッセージの暗号化とキー ラップ アルゴリズムを設定します。  この属性は、アルゴリズムとキー サイズを指定する <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> 型です。  これらのアルゴリズムは、Security Policy Language \(WS\-SecurityPolicy\) の仕様で指定されているアルゴリズムに対応付けられています。<br /><br /> 既定値は `Basic256` です。|  
|clientCredentialType|メッセージ ベースのセキュリティを使用してクライアント認証を実行するときに使用される資格情報の種類を指定します。  既定値は、`UserName` です。|  
  
## clientCredentialType 属性  
  
|値|説明|  
|-------|--------|  
|UserName|-   UserName 資格情報を使用してクライアントをサーバーに認証するように要求します。  この資格情報は、\<`clientCredentials`\> 要素を使用して指定する必要があります。<br />-   [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] は、パスワード ダイジェストの送信、またはパスワードを使用したキーの派生、およびメッセージ セキュリティでのそのようなキーの使用をサポートしません。  そのため、[!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] では UserName 資格情報を使用する場合は、トランスポートが強制的にセキュリティで保護されます。  `basicHttpBinding` の場合は、SSL チャネルの設定が必要です。|  
|証明書|証明書を使用してクライアントをサーバーに認証するように要求します。  この場合のクライアント資格情報は、\<`clientCredentials`\> および \<`clientCertificate`\> を使用して指定する必要があります。  さらに、メッセージのセキュリティ モードを使用する場合は、クライアントにサービス証明書を準備する必要があります。  この場合のサービス資格情報は、<xref:System.ServiceModel.Description.ClientCredentials> クラスまたは `ClientCredentials` 動作要素を使用し、serviceCredentials の \<serviceCertificate\> 要素を使用してサービス証明書を指定して、指定する必要があります。|  
  
### 子要素  
 なし  
  
### 親要素  
  
|要素|説明|  
|--------|--------|  
|\<`security`\> 要素 \(\<`netHttpBinding`\>\)|\<`netHttpBinding`\> 要素のセキュリティ機能を定義します。|  
  
## 使用例  
 このサンプルでは、basicHttpBinding とメッセージ セキュリティを使用するアプリケーションを実装する方法を示します。  サービスの次の構成例では、エンドポイント定義によって basicHttpBinding が指定され、`Binding1` という名前のバインディング構成が参照されます。  サービスがクライアントに対してサービス自体を認証するために使用する証明書は、`serviceCredentials` 要素の下にある構成ファイルの `behaviors` セクションで設定されます。  クライアントがサービスに対してクライアント自体を認証するために使用する証明書に適用される検証モードも、`clientCertificate` 要素の下にある `behaviors` セクションで設定されます。  
  
 同じバインディングとセキュリティの詳細が、クライアントの構成ファイルで指定されます。  
  
```  
<system.serviceModel>  
    <services>  
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
               behaviorConfiguration="CalculatorServiceBehavior">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>  
          </baseAddresses>  
        </host>  
        <!-- this endpoint is exposed at the base address provided by host: http://localhost:8000/ServiceModelSamples/service  -->  
        <endpoint address=""  
                  binding="basicHttpBinding"  
                  bindingConfiguration="Binding1"   
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
        <!-- the mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex -->  
        <endpoint address="mex"  
                  binding="mexHttpBinding"  
                  contract="IMetadataExchange" />  
      </service>  
    </services>  
  
    <bindings>  
      <basicHttpBinding>  
        <!--   
        This configuration defines the SecurityMode as Message and   
        the clientCredentialType as Certificate.  
        -->  
        <binding name="Binding1" >  
          <security mode = "Message">  
            <message clientCredentialType="Certificate"/>  
          </security>  
        </binding>  
      </basicHttpBinding>  
    </bindings>  
  
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="CalculatorServiceBehavior">  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <!--  
        The serviceCredentials behavior allows one to define a service certificate.  
        A service certificate is used by a client to authenticate the service and provide message protection.  
        This configuration references the "localhost" certificate installed during the setup instructions.  
        -->  
          <serviceCredentials>  
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
            <clientCertificate>  
              <!--   
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
            is in the user's Trusted People store, then it will be trusted without performing a  
            validation of the certificate's issuer chain. This setting is used here for convenience so that the   
            sample can be run without having to have certificates issued by a certification authority (CA).  
            This setting is less secure than the default, ChainTrust. The security implications of this   
            setting should be carefully considered before using PeerOrChainTrust in production code.   
            -->  
              <authentication certificateValidationMode="PeerOrChainTrust" />  
            </clientCertificate>  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
</system.serviceModel>  
```  
  
## 参照  
 <xref:System.ServiceModel.NetHttpMessageSecurity>   
 <xref:System.ServiceModel.Configuration.NetHttpSecurityElement.Message%2A>   
 <xref:System.ServiceModel.NetHttpSecurity.Message%2A>   
 <xref:System.ServiceModel.Configuration.NetHttpMessageSecurityElement>   
 [サービスおよびクライアントのセキュリティ保護](../../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)   
 [バインディング](../../../../../docs/framework/wcf/bindings.md)   
 [システムが提供するバインディングの構成](../../../../../docs/framework/wcf/feature-details/configuring-system-provided-bindings.md)   
 [Using Bindings to Configure Windows Communication Foundation Services and Clients](http://msdn.microsoft.com/ja-jp/bd8b277b-932f-472f-a42a-b02bb5257dfb)   
 [\<binding\>](../../../../../docs/framework/misc/binding.md)