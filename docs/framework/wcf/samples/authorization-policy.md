---
title: 承認ポリシー
ms.date: 03/30/2017
ms.assetid: 1db325ec-85be-47d0-8b6e-3ba2fdf3dda0
ms.openlocfilehash: 3744afb20c06e1ca85b91dadde6549d87ac89337
ms.sourcegitcommit: 15109844229ade1c6449f48f3834db1b26907824
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="authorization-policy"></a>承認ポリシー
このサンプルでは、カスタム クレーム承認ポリシーと、関連するカスタム サービス承認マネージャーを実装する方法を示します。 この方法は、サービスがクレームに基づくアクセス チェックをサービス操作に行う場合や、アクセス チェックを行う前に呼び出し元に特定の権限を与える場合に便利です。 このサンプルでは、クレームの追加プロセスと、完了したクレーム セットに対してアクセス チェックを行うプロセスの両方を示します。 クライアント/サーバー間のすべてのアプリケーション メッセージは署名され、暗号化されます。 `wsHttpBinding` バインディングを使用する際の既定では、クライアントによって提供されるユーザー名とパスワードが、有効な Windows NT アカウントへのログオンに使用されます。 このサンプルは、カスタムを利用する方法を示します<!--zz <xref:System.IdentityModel.Selectors.UsernamePasswordValidator>-->`System.IdentityModel.Selectors.UsernamePasswordValidator`クライアントを認証します。 さらにこのサンプルでは、クライアントが X.509 証明書を使用してサービスを認証する方法を示します。 また、<xref:System.IdentityModel.Policy.IAuthorizationPolicy> と <xref:System.ServiceModel.ServiceAuthorizationManager> の実装も示します。これらの間では、特定のユーザーに対するサービスの特定のメソッドへのアクセスが許可されます。 このサンプルがに基づいて、[メッセージ セキュリティ ユーザー名](../../../../docs/framework/wcf/samples/message-security-user-name.md)より前のバージョンの信頼性情報の変換を実行する方法を示しますが、<xref:System.ServiceModel.ServiceAuthorizationManager>呼び出されています。  
  
> [!NOTE]
>  このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
 このサンプルで示す処理の概要は次のとおりです。  
  
-   クライアントはユーザー名とパスワードを使用して認証される。  
  
-   クライアントは X.509 証明書を使用して認証される。  
  
-   サーバーはクライアント資格情報をカスタム `UsernamePassword` 検証と照合する。  
  
-   サーバーがそのサーバーの X.509 証明書を使用して認証される。  
  
-   サーバーが <xref:System.ServiceModel.ServiceAuthorizationManager> を使用して、サービス内の特定メソッドへのアクセスを制御する。  
  
-   <xref:System.IdentityModel.Policy.IAuthorizationPolicy> の実装方法。  
  
 サービスは、そのサービスとの通信に使用する 2 つのエンドポイントを公開します。エンドポイントは構成ファイル App.config で定義します。各エンドポイントは、アドレス、バインディング、およびコントラクトがそれぞれ 1 つずつで構成されます。 1 つのバインディングの構成には、WS-Security とクライアントのユーザー名認証を使用する、標準の `wsHttpBinding` バインディングが使用されます。 もう 1 つのバインディングの構成には、WS-Security とクライアント証明書による認証を使用する、標準の `wsHttpBinding` バインディングが使用されます。 [\<動作 >](../../../../docs/framework/configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md)ユーザーの資格情報がサービスの認証に使用することを指定します。 サーバー証明書が同じ値を含める必要があります、`SubjectName`プロパティとして、`findValue`属性、 [ \<serviceCertificate >](../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md)です。  
  
```xml  
<system.serviceModel>  
  <services>  
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
             behaviorConfiguration="CalculatorServiceBehavior">  
      <host>  
        <baseAddresses>  
          <!-- configure base address provided by host -->  
          <add baseAddress ="http://localhost:8001/servicemodelsamples/service"/>  
        </baseAddresses>  
      </host>  
      <!-- use base address provided by host, provide two endpoints -->  
      <endpoint address="username"  
                binding="wsHttpBinding"  
                bindingConfiguration="Binding1"   
                contract="Microsoft.ServiceModel.Samples.ICalculator" />  
      <endpoint address="certificate"  
                binding="wsHttpBinding"  
                bindingConfiguration="Binding2"   
                contract="Microsoft.ServiceModel.Samples.ICalculator" />  
    </service>  
  </services>  
  
  <bindings>  
    <wsHttpBinding>  
      <!-- Username binding -->  
      <binding name="Binding1">  
        <security mode="Message">  
    <message clientCredentialType="UserName" />  
        </security>  
      </binding>  
      <!-- X509 certificate binding -->  
      <binding name="Binding2">  
        <security mode="Message">  
          <message clientCredentialType="Certificate" />  
        </security>  
      </binding>  
    </wsHttpBinding>  
  </bindings>  
  
  <behaviors>  
    <serviceBehaviors>  
      <behavior name="CalculatorServiceBehavior" >  
        <serviceDebug includeExceptionDetailInFaults ="true" />  
        <serviceCredentials>  
          <!--   
          The serviceCredentials behavior allows one to specify a custom validator for username/password combinations.    
          -->  
          <userNameAuthentication userNamePasswordValidationMode="Custom" customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.MyCustomUserNameValidator, service" />  
          <!--   
          The serviceCredentials behavior allows one to specify authentication constraints on client certificates.  
          -->  
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
          <!--   
          The serviceCredentials behavior allows one to define a service certificate.  
          A service certificate is used by a client to authenticate the service and provide message protection.  
          This configuration references the "localhost" certificate installed during the setup instructions.  
          -->  
          <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
        </serviceCredentials>  
        <serviceAuthorization serviceAuthorizationManagerType="Microsoft.ServiceModel.Samples.MyServiceAuthorizationManager, service">  
          <!--   
          The serviceAuthorization behavior allows one to specify custom authorization policies.  
          -->  
          <authorizationPolicies>  
            <add policyType="Microsoft.ServiceModel.Samples.CustomAuthorizationPolicy.MyAuthorizationPolicy, PolicyLibrary" />  
          </authorizationPolicies>  
        </serviceAuthorization>  
      </behavior>  
    </serviceBehaviors>  
  </behaviors>  
  
</system.serviceModel>  
```  
  
 各クライアント エンドポイント構成は、構成名、サービス エンドポイントの絶対アドレス、バインディング、およびコントラクトで構成されます。 指定された適切なセキュリティ モード クライアント バインディングは構成されるこの場合、 [\<セキュリティ >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-wshttpbinding.md)と`clientCredentialType`で指定されたとおり、 [\<メッセージ >](../../../../docs/framework/configure-apps/file-schema/wcf/message-of-wshttpbinding.md).  
  
```xml  
<system.serviceModel>  
  
    <client>  
      <!-- Username based endpoint -->  
      <endpoint name="Username"  
            address="http://localhost:8001/servicemodelsamples/service/username"   
    binding="wsHttpBinding"   
    bindingConfiguration="Binding1"   
                behaviorConfiguration="ClientCertificateBehavior"  
                contract="Microsoft.ServiceModel.Samples.ICalculator" >  
      </endpoint>  
      <!-- X509 certificate based endpoint -->  
      <endpoint name="Certificate"  
                        address="http://localhost:8001/servicemodelsamples/service/certificate"   
                binding="wsHttpBinding"   
            bindingConfiguration="Binding2"   
                behaviorConfiguration="ClientCertificateBehavior"  
                contract="Microsoft.ServiceModel.Samples.ICalculator">  
      </endpoint>  
    </client>  
  
    <bindings>  
      <wsHttpBinding>  
        <!-- Username binding -->  
      <binding name="Binding1">  
        <security mode="Message">  
          <message clientCredentialType="UserName" />  
        </security>  
      </binding>  
        <!-- X509 certificate binding -->  
        <binding name="Binding2">  
          <security mode="Message">  
            <message clientCredentialType="Certificate" />  
          </security>  
        </binding>  
    </wsHttpBinding>  
    </bindings>  
  
    <behaviors>  
      <behavior name="ClientCertificateBehavior">  
        <clientCredentials>  
          <serviceCertificate>  
            <!--   
            Setting the certificateValidationMode to PeerOrChainTrust  
            means that if the certificate   
            is in the user's Trusted People store, then it will be   
            trusted without performing a  
            validation of the certificate's issuer chain. This setting   
            is used here for convenience so that the   
            sample can be run without having to have certificates   
            issued by a certification authority (CA).  
            This setting is less secure than the default, ChainTrust.   
            The security implications of this   
            setting should be carefully considered before using   
            PeerOrChainTrust in production code.   
            -->  
            <authentication certificateValidationMode = "PeerOrChainTrust" />  
          </serviceCertificate>  
        </clientCredentials>  
      </behavior>  
    </behaviors>  
  
  </system.serviceModel>  
```  
  
 ユーザー名に基づくエンドポイントには、使用するユーザー名とパスワードがクライアント実装で設定されます。  
  
```  
// Create a client with Username endpoint configuration  
CalculatorClient client1 = new CalculatorClient("Username");  
  
client1.ClientCredentials.UserName.UserName = "test1";  
client1.ClientCredentials.UserName.Password = "1tset";  
  
try  
{  
    // Call the Add service operation.  
    double value1 = 100.00D;  
    double value2 = 15.99D;  
    double result = client1.Add(value1, value2);  
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
    ...  
}  
catch (Exception e)  
{  
    Console.WriteLine("Call failed : {0}", e.Message);  
}  
  
client1.Close();  
```  
  
 証明書に基づくエンドポイントには、使用するクライアント証明書がクライアント実装で設定されます。  
  
```  
// Create a client with Certificate endpoint configuration  
CalculatorClient client2 = new CalculatorClient("Certificate");  
  
client2.ClientCredentials.ClientCertificate.SetCertificate(StoreLocation.CurrentUser, StoreName.My, X509FindType.FindBySubjectName, "test1");  
  
try  
{  
    // Call the Add service operation.  
    double value1 = 100.00D;  
    double value2 = 15.99D;  
    double result = client2.Add(value1, value2);  
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
    ...  
}  
catch (Exception e)  
{  
    Console.WriteLine("Call failed : {0}", e.Message);  
}  
  
client2.Close();  
```  
  
 このサンプルでは、カスタム <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> を使用してユーザー名とパスワードを検証します。 サンプルは、`MyCustomUserNamePasswordValidator` から派生する <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> を実装しています。 <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> の詳細については、ドキュメントを参照してください。 <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> との統合を示すため、カスタム検証のこのサンプルでは次のコード例に示すように、<xref:System.IdentityModel.Selectors.UserNamePasswordValidator.Validate%2A> メソッドを実装して、ユーザー名がパスワードと一致するユーザー名とパスワードの組み合わせを許可します。  
  
```  
public class MyCustomUserNamePasswordValidator : UserNamePasswordValidator  
{  
  // This method validates users. It allows in two users,   
  // test1 and test2 with passwords 1tset and 2tset respectively.  
  // This code is for illustration purposes only and   
  // MUST NOT be used in a production environment because it   
  // is NOT secure.      
  public override void Validate(string userName, string password)  
  {  
    if (null == userName || null == password)  
    {  
      throw new ArgumentNullException();  
    }  
  
    if (!(userName == "test1" && password == "1tset") && !(userName == "test2" && password == "2tset"))  
    {  
      throw new SecurityTokenException("Unknown Username or Password");  
    }  
  }  
}  
```  
  
 サービス コードに検証を実装した場合、使用する検証インスタンスをサービス ホストに通知する必要があります。 これは次のコードで実行されます。  
  
```  
Servicehost.Credentials.UserNameAuthentication.UserNamePasswordValidationMode = UserNamePasswordValidationMode.Custom;  
serviceHost.Credentials.UserNameAuthentication.CustomUserNamePasswordValidator = new MyCustomUserNamePasswordValidatorProvider();  
```  
  
 または、構成で同じことを実現できます。  
  
```xml  
<behavior ...>  
    <serviceCredentials>  
      <!--   
      The serviceCredentials behavior allows one to specify a custom validator for username/password combinations.    
      -->  
      <userNameAuthentication userNamePasswordValidationMode="Custom" customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.MyCustomUserNameValidator, service" />  
    ...  
    </serviceCredentials>  
</behavior>  
```  
  
 Windows Communication Foundation (WCF) は、アクセス チェックを実行するための豊富なクレームに基づくモデルを提供します。 <xref:System.ServiceModel.ServiceAuthorizationManager> オブジェクトを使用するとアクセス チェックが実行され、クライアントに関連付けられたクレームがサービス メソッドへのアクセスに必要な要件を満たすかどうかが判断されます。  
  
 デモの目的は、このサンプルの実装を示します<xref:System.ServiceModel.ServiceAuthorizationManager>を実装する、<xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccessCore%2A>メソッドへのユーザーのアクセスを許可するメソッドが型のクレームに基づくhttp://example.com/claims/allowedoperation値がある操作のアクション URI。呼び出される許可します。  
  
```  
public class MyServiceAuthorizationManager : ServiceAuthorizationManager  
{  
  protected override bool CheckAccessCore(OperationContext operationContext)  
  {                  
    string action = operationContext.RequestContext.RequestMessage.Headers.Action;  
    Console.WriteLine("action: {0}", action);  
    foreach(ClaimSet cs in operationContext.ServiceSecurityContext.AuthorizationContext.ClaimSets)  
    {  
      if ( cs.Issuer == ClaimSet.System )  
      {  
        foreach (Claim c in cs.FindClaims("http://example.com/claims/allowedoperation", Rights.PossessProperty))  
        {  
          Console.WriteLine("resource: {0}", c.Resource.ToString());  
          if (action == c.Resource.ToString())  
            return true;  
        }  
      }  
    }  
    return false;                   
  }  
}  
```  
  
 カスタム <xref:System.ServiceModel.ServiceAuthorizationManager> を実装した場合、使用する <xref:System.ServiceModel.ServiceAuthorizationManager> をサービス ホストに通知する必要があります。 これを行うコードは次のようになります。  
  
```xml  
<behavior ...>  
    ...  
    <serviceAuthorization serviceAuthorizationManagerType="Microsoft.ServiceModel.Samples.MyServiceAuthorizationManager, service">  
        ...  
    </serviceAuthorization>  
</behavior>  
```  
  
 実装する主要な <xref:System.IdentityModel.Policy.IAuthorizationPolicy> メソッドは、<xref:System.IdentityModel.Policy.IAuthorizationPolicy.Evaluate%28System.IdentityModel.Policy.EvaluationContext%2CSystem.Object%40%29> メソッドです。  
  
```  
public class MyAuthorizationPolicy : IAuthorizationPolicy  
{  
    string id;  
  
    public MyAuthorizationPolicy()  
    {  
    id =  Guid.NewGuid().ToString();  
    }  
  
    public bool Evaluate(EvaluationContext evaluationContext,   
                                            ref object state)  
    {  
        bool bRet = false;  
        CustomAuthState customstate = null;  
  
        if (state == null)  
        {  
            customstate = new CustomAuthState();  
            state = customstate;  
        }  
        else  
            customstate = (CustomAuthState)state;  
        Console.WriteLine("In Evaluate");  
        if (!customstate.ClaimsAdded)  
        {  
           IList<Claim> claims = new List<Claim>();  
  
           foreach (ClaimSet cs in evaluationContext.ClaimSets)  
              foreach (Claim c in cs.FindClaims(ClaimTypes.Name,   
                                         Rights.PossessProperty))  
                  foreach (string s in   
                        GetAllowedOpList(c.Resource.ToString()))  
                  {  
                       claims.Add(new  
               Claim("http://example.com/claims/allowedoperation",   
                                    s, Rights.PossessProperty));  
                            Console.WriteLine("Claim added {0}", s);  
                      }  
                   evaluationContext.AddClaimSet(this,   
                           new DefaultClaimSet(this.Issuer,claims));  
                   customstate.ClaimsAdded = true;  
                   bRet = true;  
                }  
         else  
         {  
              bRet = true;  
         }  
         return bRet;  
     }  
...  
}  
```  
  
 前のコードでは、<xref:System.IdentityModel.Policy.IAuthorizationPolicy.Evaluate%28System.IdentityModel.Policy.EvaluationContext%2CSystem.Object%40%29> メソッドが、処理に影響を与える新しいクレームが追加されていないことをチェックして特定のクレームを追加する方法を示しています。 許可されるクレームは `GetAllowedOpList` メソッドから取得されます。このメソッドが実装されると、ユーザーによる実行が許可されている特定の操作リストが返されます。 承認ポリシーは、特定の操作にアクセスするためのクレームを追加します。 このポリシーは、アクセス チェックの決定を実行する <xref:System.ServiceModel.ServiceAuthorizationManager> によって後で使用されます。  
  
 カスタム <xref:System.IdentityModel.Policy.IAuthorizationPolicy> を実装した場合、使用する承認ポリシーをサービス ホストに通知する必要があります。  
  
```xml  
<serviceAuthorization ...>  
       <authorizationPolicies>   
            <add policyType='Microsoft.ServiceModel.Samples.CustomAuthorizationPolicy.MyAuthorizationPolicy, PolicyLibrary' />  
       </authorizationPolicies>   
</serviceAuthorization>  
```  
  
 このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。 クライアントでは Add、Subtract、および Multiple メソッドは正常に呼び出されますが、Divide メソッドを呼び出そうとすると、"アクセスが拒否されました" のメッセージが発生します。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。  
  
## <a name="setup-batch-file"></a>セットアップ バッチ ファイル  
 このサンプルに用意されている Setup.bat バッチ ファイルを使用すると、適切な証明書を使用してサーバーを構成し、サーバー証明書ベースのセキュリティを必要とする自己ホスト型アプリケーションを実行できるようになります。  
  
 次に、バッチ ファイルのセクションのうち、該当する構成で実行するために変更が必要となる部分を示します。  
  
-   サーバー証明書の作成。  
  
     Setup.bat バッチ ファイルの次の行は、使用するサーバー証明書を作成します。 %SERVER_NAME% 変数はサーバー名を指定します。 この変数を変更して、使用するサーバー名を指定します。 既定値は、localhost です。  
  
    ```  
    echo ************  
    echo Server cert setup starting  
    echo %SERVER_NAME%  
    echo ************  
    echo making server cert  
    echo ************  
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe  
    ```  
  
-   サーバー証明書のクライアントの信頼された証明書ストアへのインストール。  
  
     Setup.bat バッチ ファイルの次の行は、サーバー証明書をクライアントの信頼されたユーザーのストアにコピーします。 この手順が必要なのは、Makecert.exe によって生成される証明書がクライアント システムにより暗黙には信頼されないからです。 マイクロソフト発行の証明書など、クライアントの信頼されたルート証明書に基づいた証明書が既にある場合は、クライアント証明書ストアにサーバー証明書を配置するこの手順は不要です。  
  
    ```  
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
-   クライアント証明書の作成。  
  
     Setup.bat バッチ ファイルの次の行は、使用するクライアント証明書を作成します。 %USER_NAME% 変数はユーザー名を指定します。 この値は "test1" に設定されます。`IAuthorizationPolicy` によってこの名前が検索されたためです。 %USER_NAME% の値を変更した場合は、`IAuthorizationPolicy.Evaluate` メソッド内の対応する値を変更する必要があります。  
  
     証明書は、CurrentUser ストアの場所の My (Personal) ストアに保存されます。  
  
    ```  
    echo ************  
    echo making client cert  
    echo ************  
    makecert.exe -sr CurrentUser -ss MY -a sha1 -n CN=%CLIENT_NAME% -sky exchange -pe  
    ```  
  
-   クライアント証明書のサーバーの信頼された証明書ストアへのインストール。  
  
     Setup.bat バッチ ファイルの次の行は、クライアント証明書を信頼されたユーザーのストアにコピーします。 この手順が必要なのは、Makecert.exe によって生成される証明書がサーバー システムにより暗黙には信頼されないからです。 マイクロソフト発行の証明書など、信頼されたルート証明書に基づく証明書が既にある場合は、サーバー証明書ストアにクライアント証明書を配置するこの手順は不要です。  
  
    ```  
    certmgr.exe -add -r CurrentUser -s My -c -n %CLIENT_NAME% -r LocalMachine -s TrustedPeople  
    ```  
  
#### <a name="to-set-up-and-build-the-sample"></a>サンプルをセットアップしてビルドするには  
  
1.  指示に従って、ソリューションをビルドする[Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)です。  
  
2.  サンプルを単一コンピューター構成で実行するか、複数コンピューター構成で実行するかに応じて、次の手順に従います。  
  
> [!NOTE]
>  Svcutil.exe を使用してこのサンプルの構成を再生成した場合は、クライアント コードに一致するように、クライアント構成内のエンドポイント名を変更してください。  
  
#### <a name="to-run-the-sample-on-the-same-computer"></a>サンプルを同じコンピューターで実行するには  
  
1.  管理特権で [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] コマンド プロンプト ウィンドウを開き、サンプルのインストール フォルダーから Setup.bat を実行します。 これにより、サンプルの実行に必要なすべての証明書がインストールされます。  
  
    > [!NOTE]
    >  Setup.bat バッチ ファイルは、[!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] コマンド プロンプトから実行します。 [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)] コマンド プロンプト内で設定された PATH 環境変数は、Setup.bat スクリプトで必要な実行可能ファイルが格納されているディレクトリを指しています。  
  
2.  管理特権を使用して Visual Studio コマンド プロンプトを開き、サンプルのインストール フォルダーから Setup.bat を実行します。 これにより、サンプルの実行に必要なすべての証明書がインストールされます。  
  
3.  Service.exe を service\bin で起動します。  
  
4.  Client.exe を \client\bin で起動します。 クライアント アクティビティがクライアントのコンソール アプリケーションに表示されます。  
  
5.  クライアントとサービスできない場合は通信するためを参照してください。[トラブルシューティングのヒント](http://msdn.microsoft.com/library/8787c877-5e96-42da-8214-fa737a38f10b)です。  
  
#### <a name="to-run-the-sample-across-computers"></a>サンプルを複数のコンピューターで実行するには  
  
1.  サービス コンピューターにディレクトリを作成します。  
  
2.  \service\bin のサービス プログラム ファイルを、サービス コンピューターのディレクトリにコピーします。 Setup.bat、Cleanup.bat、GetComputerName.vbs、ImportClientCert.bat の各ファイルもサービス コンピューターにコピーします。  
  
3.  クライアント コンピューターにクライアント バイナリ用のディレクトリを作成します。  
  
4.  クライアント プログラム ファイルを、クライアント コンピューターに作成したクライアント ディレクトリにコピーします。 Setup.bat、Cleanup.bat、ImportServiceCert.bat の各ファイルもクライアントにコピーします。  
  
5.  サーバー上で管理特権を使用して Visual Studio コマンド プロンプトを開き、`setup.bat service` を実行します。 実行している`setup.bat`で、`service`引数サービス証明書を指定のエクスポートの完全修飾ドメイン名、サービス証明書を作成 Service.cer というファイルにします。  
  
6.  新しい証明書名を反映するように Service.exe.config を編集 (で、`findValue`属性、 [ \<serviceCertificate >](../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md)) は、コンピューターの完全修飾ドメイン名と同じです。 コンピューター名を変更しても、\<サービス >/\<baseAddresses > 要素を localhost からサービス コンピューターの完全修飾名。  
  
7.  Service.cer ファイルを、サービス ディレクトリからクライアント コンピューターのクライアント ディレクトリにコピーします。  
  
8.  クライアント上で、管理特権を使用して Visual Studio コマンド プロンプトを開き、`setup.bat client` を実行します。 実行している`setup.bat`で、`client`引数は、test1 というクライアント証明書を作成し、クライアント証明書が Client.cer というファイルにエクスポートします。  
  
9. クライアント コンピューターの Client.exe.config ファイルで、エンドポイントのアドレス値をサービスの新しいアドレスに合わせます。 そのためには、localhost をサーバーの完全修飾ドメイン名に置き換えます。  
  
10. Client.cer ファイルを、クライアント ディレクトリからサーバーのサービス ディレクトリにコピーします。  
  
11. クライアント上で、管理特権を使用して Visual Studio コマンド プロンプトを開き、ImportServiceCert.bat を実行します。 これにより、サービス証明書が Service.cer ファイルから CurrentUser - TrustedPeople ストアにインポートされます。  
  
12. サーバー上で、管理特権を使用して Visual Studio コマンド プロンプトを開き、ImportClientCert.bat を実行します。 これにより、クライアント証明書が Client.cer ファイルから LocalMachine - TrustedPeople ストアにインポートされます。  
  
13. サーバー コンピューターで、コマンド プロンプト ウィンドウから Service.exe を起動します。  
  
14. クライアント コンピューターで、コマンド プロンプト ウィンドウから Client.exe を起動します。 クライアントとサービスできない場合は通信するためを参照してください。[トラブルシューティングのヒント](http://msdn.microsoft.com/library/8787c877-5e96-42da-8214-fa737a38f10b)です。  
  
#### <a name="to-clean-up-after-the-sample"></a>サンプルの実行後にクリーンアップするには  
  
1.  サンプルの実行が終わったら、サンプル フォルダーにある Cleanup.bat を実行します。 これにより、証明書ストアからサーバー証明書とクライアント証明書が削除されます。  
  
> [!NOTE]
>  このサンプルを複数のコンピューターで実行している場合、このスクリプトはサービス証明書をクライアントから削除しません。 複数のコンピューターで証明書を使用して、必ず、CurrentUser - にインストールされているサービス証明書をオフにする WCF サンプルを実行している場合 TrustedPeople を格納します。 削除するには、コマンド `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` を実行します。たとえば、`certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com` となります。  
  
## <a name="see-also"></a>関連項目
