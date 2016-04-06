<properties 
    pageTitle="Cloud App Discovery のプロキシ サービス用レジストリ設定 | Microsoft Azure" 
    description="このトピックでは、Cloud App Discovery エージェントを実行しているコンピューターで、必要なポートを設定するために実行する必要がある手順を説明します。" 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="stevenpo"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2015" 
    ms.author="markusvi"/>

# Cloud App Discovery のプロキシ サービス用レジストリ設定

既定では、Cloud App Discovery エージェントはポート 80 または 443 のみを使用するように構成されています。 
Cloud App Discovery を、カスタム ポート (80 でも 443 でもない) を使用しているプロキシ サーバーを含む環境にインストールしようと計画している場合は、エージェントもこのポートを使用するように構成する必要があります。 
構成は、レジストリ キーに基づきます。


このトピックでは、Cloud App Discovery エージェントを実行しているコンピューターで、必要なポートを設定するために実行する必要がある手順を説明します。



**Cloud App Discovery エージェントを実行しているコンピューターで使用されるポートを変更するには、次の手順を実行します。**


1. レジストリ エディターを起動します。 <br> ![実行](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)

2. 移動または、次のレジストリ キーを作成します。 <br> **Hklm_local_machine \software\microsoft\cloud App discovery \endpoint** 

3. 新しい **複数行文字列** と呼ばれる値 **ポート**します。 ![新しい](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)

4. 開くには、 **複数行文字列の編集** ダイアログ ボックスで、ポートの値をダブルクリックします。


5. 値のデータ] ボックスに、次の値を入力し、組織で使用されるすべてのカスタム ポートを追加します。 <br><br>
**80** <br>
**8080** <br>
**8118** <br>
**8888** <br>
**81** <br>
**12080** <br>
**6999** <br>
**30606** <br>
**31595** <br>
**4080** <br>
**使用します** <br>
**1110** <br><br>
![複数行文字列を編集します。](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)

6. をクリックして **OK** を閉じる、 **複数行文字列の編集** ダイアログ。



**その他のリソース**


* [自分の組織内で使用される承認されていないクラウド アプリを検出する方法](active-directory-cloudappdiscovery-whatis.md) 




