<properties 
    pageTitle="PhoneFactor エージェントから Azure Multi-Factor Authentication Server へのアップグレード" 
    description="このドキュメントでは、Azure MFA Server を開始する方法と、古い phonefactor エージェントからアップグレードする方法について説明します。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/19/2015" 
    ms.author="billmath"/>

# PhoneFactor エージェントから Azure Multi-Factor Authentication Server へのアップグレード

PhoneFactor エージェント v5.x 以前から Azure Multi-factor Authentication Server にアップグレードするには、PhoneFactor エージェントおよび関連するコンポーネントをアンインストールし、その後 Multi-Factor Authentication Server および関連するコンポーネントをインストールする必要があります。 

## PhoneFactor エージェントから Azure Multi-Factor Authentication Server にアップグレードするには
<ol>
<li>まず、PhoneFactor データ ファイルをバックアップします。 既定のインストール先は C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata です。


<li>ユーザー ポータルがインストールされている場合は、</li>
<ol>
<li>インストール フォルダーに移動し、web.config ファイルをバックアップします。 既定のインストール先は C:\inetpub\wwwroot\PhoneFactor です。</li>


<li>カスタム テーマをポータルに追加した場合は、C:\inetpub\wwwroot\PhoneFactor\App_Themes ディレクトリ以下にカスタム フォルダーをバックアップします。</li>


<li>ユーザー ポータルを PhoneFactor エージェント (PhoneFactor エージェントと同じサーバーにインストールされている場合にのみ使用可能) または Windows の [プログラムと機能] よりアンインストールします。</li></ol>




<li>場合は、モバイル アプリ Web サービスがインストールされます。
<ol>
<li>インストール フォルダーに移動し、web.config ファイルをバックアップします。 既定のインストール場所は C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService です。</li>
<li>Windows でのモバイル アプリ Web サービスをアンインストールするプログラムと機能します。</li></ol>

<li>Web サービス SDK がインストールされている場合は、PhoneFactor エージェントまたは Windows の [プログラムと機能] からアンインストールします。

<li>Windows で PhoneFactor エージェントのアンインストール プログラムと機能します。

<li>Multi-factor Authentication Server をインストールします。 インストール パスは前回の PhoneFactor エージェントのインストールのレジストリから取得されるため、同じ場所 (C:\Program Files\PhoneFactor) にインストールされます。 新しくインストールする場合は、異なる既定のインストール パス (C:\Program Files\Multi-Factor Authentication Server など) が作成されます。 以前の PhoneFactor エージェントにより残されたデータ ファイルはインストール時にアップグレードされるため、ユーザーおよび設定は新しい Multi-Factor Authentication Server のインストール後もそこに残す必要があります。

<li>メッセージが表示されたら、Multi-factor Authentication Server をアクティブ化し、適切なレプリケーション グループに割り当てられるようにします。

<li>Web サービス SDK がインストールされていた場合は、Multi-factor Authentication Server ユーザー インターフェイスより新しい Web サービス SDK をインストールします。 既定の仮想ディレクトリ名は、"PhoneFactorWebServiceSdk" ではなく "MultiFactorAuthWebServiceSdk" であることに注意してください。 以前の名前を使用する場合は、インストール時に仮想ディレクトリ名を変更する必要があります。 そのようにしない場合、インストールで新しい既定の名前の使用を許可する場合は、ユーザー ポータルやモバイル アプリ Web サービスなどの Web サービス SDK を参照する任意のアプリケーションの URL を正しい場所を示すように変更する必要があります。

<li>PhoneFactor エージェント サーバーには、ユーザー ポータルがインストールされていた、Multi-factor Authentication Server ユーザー インターフェイスより新しい Multi-factor Authentication ユーザー ポータルをインストールします。 既定の仮想ディレクトリ名は、"PhoneFactor" ではなく "MultiFactorAuth" であることに注意してください。 以前の名前を使用する場合は、インストール時に仮想ディレクトリ名を変更する必要があります。 そのようにしない場合、インストールで新しい既定の名前の使用を許可する場合は、Multi-Factor Authentication Server の [ユーザー ポータル] アイコンをクリックし、[設定] タブのユーザー ポータル URL を更新する必要があります。 

<li>PhoneFactor エージェントから別のサーバーにユーザー ポータルやモバイル アプリ Web サービスをインストールした場合。
<ol>
<li>インストール場所 (C:\Program \phonefactor など) に移動し、適切なインストーラーを他のサーバーにコピーします。 ユーザー ポータルとモバイル アプリ Web サービスには、32 ビットおよび 64 ビットのインストーラーがあります。 それぞれ、という MultiFactorAuthenticationUserPortalSetupXX.msi と MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi です。</li>
<li>ユーザー ポータル web サーバーにインストールするには、管理者としてコマンド プロンプトを開き、MultiFactorAuthenticationUserPortalSetupXX.msi を実行します。 既定の仮想ディレクトリ名は、"PhoneFactor" ではなく "MultiFactorAuth" であることに注意してください。 以前の名前を使用する場合は、インストール時に仮想ディレクトリ名を変更する必要があります。 そのようにしない場合、インストールで新しい既定の名前の使用を許可する場合は、Multi-Factor Authentication Server の [ユーザー ポータル] アイコンをクリックし、[設定] タブのユーザー ポータル URL を更新する必要があります。 既存のユーザーは、新しい URL を通知する必要があります。</li>
<li>移動ユーザー ポータルに先 (C:\inetpub\wwwroot\MultiFactorAuth など) をインストールし、web.config ファイルを編集します。 新しい web.config ファイルへのアップグレード前にバックアップされた元の web.config ファイルから、appSettings および applicationSettings セクションの値をコピーします。 Web サービス SDK をインストールするときに新しい既定の仮想ディレクトリ名が保持された場合は、正しい場所を示すように applicationSettings セクションの URL を変更します。 その他の任意の既定値は、以前の web.config ファイルに変更された場合は、その同様の変更を新しい web.config ファイルに適用されます。</li>
<li>モバイル アプリ Web サービスを web サーバーにインストールするには、管理者としてコマンド プロンプトを開き、MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi を実行します。 既定の仮想ディレクトリ名は、"PhoneFactorPhoneAppWebService" ではなく "MultiFactorAuthMobileAppWebService" であることに注意してください。 以前の名前を使用する場合は、インストール時に仮想ディレクトリ名を変更する必要があります。 エンドユーザーがモバイル デバイスに簡単に入力できるように、短い名前を選択できます。 それ以外の場合、新しい既定の名前を使用するインストールを許可する場合は、Multi-factor Authentication server モバイル アプリ] アイコンをクリックし、モバイル アプリ Web サービス URL を更新する必要があるします。</li>
<li>移動のインストール先 (C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService など)、モバイル アプリ Web サービスにし、web.config ファイルを編集します。 新しい web.config ファイルへのアップグレード前にバックアップされた元の web.config ファイルから、appSettings および applicationSettings セクションの値をコピーします。 Web サービス SDK をインストールするときに新しい既定の仮想ディレクトリ名が保持された場合は、正しい場所を示すように applicationSettings セクションの URL を変更します。 その他の任意の既定値は、以前の web.config ファイルに変更された場合は、その同様の変更を新しい web.config ファイルに適用されます。</li></ol>


 


 
