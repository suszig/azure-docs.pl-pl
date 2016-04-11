<properties
    pageTitle="Access Control の使用方法 (Java)  | Microsoft Azure"
    description="Azure 上で Java により Access Control を開発して使用する方法について説明します。"
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="jimbe" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/12/2015" 
    ms.author="robmcm" />

# Eclipse を使用して Azure の Access Control Service で Web ユーザーを認証する方法

このガイドでは、Azure Toolkit for Eclipse 内で Azure の Access Control Service (ACS) を使用する方法について説明します。 ACS の詳細については、次を参照してください。、 [次のステップ](#next_steps) セクションです。

> [AZURE.NOTE]
> Azure Access Control Services Filter は Community Technology Preview 版です。 プレリリース版ソフトウェアとしてマイクロソフトによって正式にサポートされていません。

## ACS とは

ほとんどの開発者は id の専門家ではない、一般的にしないようにするには
費やして時間認証と承認メカニズムを開発用
アプリケーションとサービスです。 ACS は、Azure サービスを提供します。
使用して web にアクセスする必要があるユーザーを認証するための簡単
アプリケーションとを使えば、複雑なサービス
認証ロジックをコードにします。

ACS では、次の機能を使用できます。

-   Windows Identity Foundation (WIF) との統合。
-   Windows Live ID、Google、Yahoo!、Facebook などの一般的な Web ID プロバイダーのサポート。
-   Active Directory フェデレーション サービス (AD FS) 2.0 のサポート。
-   ACS 設定へのプログラムによるアクセスを提供する Open Data Protocol (OData) ベースの管理サービス。
-   ACS 設定への管理アクセスが可能な管理ポータル。

ACS の詳細については、次を参照してください。 [Access Control Service 2.0][]します。

## 概念

Azure ACS はクレーム ベース id のプリンシパルに基づいており、
一貫したアプローチでの認証メカニズムを作成します。
オンプレミスで実行されているアプリケーションまたはクラウドにします。 クレーム ベース id
取得するには、アプリケーションとサービスの一般的な方法を提供します
組織内のユーザーに関する必要な id 情報
他の組織とインターネットのです。

このガイドのタスクを完了するには、次の事項
概念:

**クライアント** -このガイドのコンテキストで、これは、ブラウザーを
web アプリケーションへのアクセスを試行します。

**証明書利用者 (rp) アプリケーション** -RP アプリケーションとは、web サイトまたは
外部の機関に認証を委託するサービスです。 イン
ユーザーの専門用語、RP がその機関を信頼するある言います。 このガイドは、
ACS を信頼するアプリケーションを構成する方法について説明します。

**トークン** -トークンは通常はセキュリティ データのコレクション
ユーザーの成功した認証に対して発行されます。 セットが含まれている *クレーム*, 、認証されたユーザーの属性です。 クレームが表すことができます、
ユーザー名、ユーザーの年齢などをユーザーが属しているロールの id
同様に続きます。 トークンは通常、電子署名済み、つまり、そのことができます。
常に発行者を明らかにするし、そのコンテンツを改ざんできません
します。 ユーザーは、有効な提示することによって、RP アプリケーションへのアクセスを得る
RP アプリケーションが信頼している機関によって発行されたトークン。

**Id プロバイダー (IP)** -IP は認証した機関
ユーザー id と問題のセキュリティ トークン。 実際に発行する作業
トークンがセキュリティ トークンと呼ばれる特別なサービスを介して実装されます。
実行されます。 IP の典型的な例としては、Windows Live ID、
Facebook、ビジネス ユーザー リポジトリ (Active Directory など) があります。
ACS を構成して、IP を信頼する場合、システムを許可し、
その IP によって発行されたトークンを検証します。 ACS は、1 回に複数の Ip を信頼できます。
できるアプリケーションでは、ACS を信頼するときに使用して即座には
すべての ip の認証されたすべてのユーザーに、アプリケーションが提示します。
あなたの代わりに、ACS を信頼します。

**フェデレーション プロバイダー (FP)** -Ip は、ユーザーの直接の知識を持つ
認証の資格情報とはどのようなクレームを発行を使用して、
その情報を持っています。 フェデレーション プロバイダー (FP) は、別の種類
証明機関: として機能するユーザーを直接認証ではなく、
1 つの RP と 1 つ以上の間で仲介者とブローカーの認証
仲介します。 IP と FP はどちらもセキュリティ トークンを発行するため、
Security Token Services (STS) を使用します。 ACS は FP の 1 つです。

**ACS 規則エンジン** -から受信するトークンを変換するためのロジック
RP で使用されるトークンに信頼できる Ip がフォームに体系化されて
単純なクレーム変換規則です。 エンジンのルールを ACS の機能
指定した任意の変換ロジックを適用することを行います
RP。

**ACS 名前空間** -名前空間は、ACS の最上位レベルの部分を
設定を整理するために使用します。 名前空間は、ip アドレスのリストを保持します。
必要なルールを提供する RP アプリケーションの信頼
ルール エンジンで受信トークンの処理にしたりできます。 名前空間
アプリケーションによって使用されるさまざまなエンドポイントを公開して、
開発者はその機能を実行するように ACS を取得します。

次の図は、web での ACS 認証のしくみを示しています。
。

![ACS フロー図][acs_flow]

1.  クライアント (ここではブラウザー) が RP のページを要求します。
2.  要求がまだ認証されていないので、RP は信頼している機関 (つまり ACS) にユーザーをリダイレクトします。 ACS は、この RP 用に指定されている IP の選択肢をユーザーに提示します。 ユーザーが適切な IP を選択します。
3.  クライアントは IP の認証ページに移動し、ユーザーにログオンを求めま す。
4.  クライアントが認証されたら (たとえば、ID 資格情報が入力されたら)、IP はセキュリティ トークンを発行します。
5.  セキュリティ トークンの発行後、IP はクライアントを ACS にリダイレクトし、クライアントは IP によって発行されたセキュリティ トークンを ACS に送信します。
6.  ACS は IP によって発行されたセキュリティ トークンを検証し、 このトークンの ID クレームを ACS 規則エンジンに入力して出力 ID クレームを計算し、これらの出力クレームを含む新しいセキュリティ トークンを発行します。
7.  ACS がクライアントを RP にリダイレクトします。 クライアントが ACS によって発行された新しいセキュリティ トークンを RP に送信します。 RP が ACS によって発行されたセキュリティ トークンの署名を検証し、このトークンのクレームを検証して、最初に要求されたページを返します。

## 前提条件

このガイドのタスクを完了するには、次のものが必要です。

- A Java Developer Kit (JDK) v 1.6 以降。
- Eclipse IDE for Java EE Developers Indigo 以降。 これはからダウンロードできる <http://www.eclipse.org/downloads/>します。 
- Java ベースの Web サーバーまたはアプリケーション サーバーのディストリビューション (Apache Tomcat、GlassFish、JBoss Application Server、Jetty など)。
- Azure サブスクリプション。 から入手できます <http://www.microsoft.com/windowsazure/offers/>します。
- Azure Toolkit for Eclipse、2014 年 4 月のリリース以降。 詳細については、次を参照してください。 [Azure Toolkit for Eclipse をインストールする](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx)です。
- アプリケーションで使用する X.509 証明書。 この証明書は .cer (公開証明書) と .PFX (Personal Information Exchange) の両方の形式のものが必要です。 (この証明書の作成方法についてはこのチュートリアルで後で説明)。
- 使いやすさと Azure コンピューティング エミュレーターと展開の手法がで説明されている [Eclipse で Azure の Hello World アプリケーションを作成する](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx)です。

## ACS 名前空間の作成

Azure で Access Control Service (ACS) の使用を開始するには、
ACS 名前空間を作成する必要があります。 名前空間では、
アプリケーション内から ACS リソースのアドレスを指定するための一意のスコープが提供されます。

1. ログイン、 [Azure 管理ポータル][]します。
2. クリックして **Active Directory**します。 
3. 新しい Access Control 名前空間を作成する] をクリックして **新規**, 、] をクリックして **App Services**, 、クリックして **アクセス制御**, 、順にクリック **簡易作成**します。 
4. 名前空間の名前を入力します。 名前が一意であるかが確認されます。
5. 名前空間が使用されるリージョンを選択します。 パフォーマンスを最高にするには、アプリケーションを展開するリージョンと同じにし、[作成] をクリックします。
6. 複数のサブスクリプションがある場合は、ACS 名前空間に使用するサブスクリプションを選択します。
7. クリックして **作成**します。

名前空間が作成されて有効化されます。 新しい名前空間の状態になるまでの待機 **Active** 続行する前にします。 

## ID プロバイダーの追加

このタスクでの RP アプリケーションと共に使用できるように Ip を追加します。
認証します。 デモンストレーションを目的として、このタスクでは
Windows Live を IP として追加する方法を説明しますが、ACS 管理ポータルの一覧にある任意の IP を
使用できます。


1.   [Azure 管理ポータル][], をクリックして **Active Directory**, 、Access Control 名前空間を選択し、[クリックして **管理**します。 ACS 管理ポータルが開きます。
2.  ACS 管理ポータルの左側のナビゲーション ウィンドウで、クリックして **Id プロバイダー**します。
3.  Windows Live ID は既定で有効になっており、削除することはできません。 このチュートリアルでは、Windows Live ID のみ使用します。 この画面がクリックして、ほかの Ip を追加できます、 **追加** ] ボタンをクリックします。

これで、Windows Live ID が ACS 名前空間の IP として有効になりました。 次に、します。
指定 (後で作成) する Java web アプリケーションを RP として。

## 証明書利用者アプリケーションの追加

このタスクで Java web アプリケーションを認識するように ACS を構成します。
有効な RP アプリケーションとしてアプリケーションです。

1.  ACS 管理ポータルで、をクリックして **証明書利用者アプリケーション**します。
2.   **証明書利用者アプリケーション** ] ページで [ **追加**します。
3.   **証明書利用者アプリケーションの追加** ] ページで、次の操作します。
    1.   **名**, 、RP の名前を入力します。 このチュートリアルの目的で、入力 **Azure Web
        アプリ**します。
    2.   **モード**, [ **設定を入力して手動で**します。
    3.   **レルム**, 、ACS によって発行されたセキュリティ トークンを適用する URI を入力します。 このタスクでは、「 **http://localhost:8080/**します。
        ![コンピューティング エミュレーターで使用する証明書利用者の領域][relying_party_realm_emulator]
    4.   **戻り先 URL** ACS がセキュリティ トークンを返す URL を入力します。 このタスクでは、「 **http://localhost:8080/MyACSHelloWorld/index.jsp**
        ![コンピューティング エミュレーターで使用する証明書利用者のパーティの戻り先 URL][relying_party_return_url_emulator]
    5.  その他のフィールドは、既定値のままにします。

4.  クリックして **保存**します。

これで、Azure コンピューティング エミュレーターで実行される Java Web アプリケーション (
http://localhost:8080/)、ACS 名前空間の RP としてにします。 次に、作成します。
このルールは、ACS が RP のクレームの処理に使用します。

## 規則の作成

このタスクでは、クレームが IP から RP にどのように渡されるかを決定する
規則を定義します。 このガイドでは、単に入力クレームの種類と値を
ACS は、入力のコピーにクレームの種類と、出力の直接値
フィルタ リングや変更は行わない場合は、トークンです。

1.  ACS 管理ポータルのメイン ページで、をクリックして **ルール グループ**します。
2.   **ルール グループ** ] ページで [ **Azure Web App の既定のルール グループ**します。
3.   **ルール グループの編集** ] ページで [ **生成**します。
4.   **規則の生成: Azure Web App の既定のルール グループ** ] ページで Windows Live ID を確認することを確認してをクリックし、 **生成**します。    
5.   **ルール グループの編集** ] ページで [ **保存**します。

## ACS 名前空間への証明書のアップロード

このタスクでは、ACS 名前空間によって作成されたトークン要求への署名に使用される .PFX 証明書をアップロードします。

1.  ACS 管理ポータルのメイン ページで、をクリックして **証明書とキー**します。
2.   **証明書とキー** ] ページで [ **追加** 上 **トークン署名**します。
3.   **追加のトークン署名証明書またはキー** ページ。
    1.  **ため** ] をクリックして **証明書利用者アプリケーション** を選択し、 **Azure Web App** (これは、証明書利用者アプリケーションの名前で設定した)。
    2.  **型** ] セクションの [ **X.509 証明書**します。
    3.  **証明書** セクションで、[参照] ボタンをクリックし、使用する X.509 証明書ファイルに移動します。 これが .PFX ファイルになります。 ファイルを選択して [ **開く**,  、証明書のパスワードを入力し、 **パスワード** テキスト ボックスです。 テスト目的で、自己署名証明書を使用してもかまいません。 自己署名証明書を作成するには、使用、 **新規** ボタンをクリックして、 **ACS Filter Library** ダイアログ (後で説明)、または使用して、 **encutil.exe** からユーティリティ、 [プロジェクト web サイト][] Azure Starter Kit for Java のです。
    4. いることを確認 **プライマリ** がオンになっています。  **追加のトークン署名証明書またはキー** ページは、次のようになります。
        ![トークン署名証明書を追加します。][add_token_signing_cert]
    5. をクリックして **保存** の設定を保存して閉じる、 **追加のトークン署名証明書またはキー** ページです。

次に、[アプリケーション統合] ページの情報を確認し、
ACS を使用するように Java Web アプリケーションを構成するために
必要な URI をコピーします。

## [アプリケーション統合] ページの確認

表示、すべての情報とコードを構成するために必要です
Java web アプリケーション (RP アプリケーション) で、ACS が連携するには
ACS 管理ポータルのアプリケーション統合] ページ。 次の
この情報の Java web アプリケーションを構成するときに必要
フェデレーション認証。

1.  ACS 管理ポータルで、をクリックして **アプリケーションの統合**します。  
2.   **アプリケーションの統合** ] ページで [ **ログイン ページ**します。
3.   **ログイン ページ統合** ] ページで [ **Azure Web App**します。

 **ログイン ページ統合: Azure Web App** ] ページに表示された URL **オプション 1: ACS ホステッド ログイン ページへのリンク** Java web アプリケーションで使用されます。 この値は、Azure の Access Control Services Filter ライブラリを Java アプリケーションに追加するときに必要になります。

## Java Web アプリケーションの作成
1. Eclipse のメニューで [ **ファイル**, をクリックして **新規**, 、順にクリック **[Dynamic Web Project**します。 (表示されない場合 **[Dynamic Web Project** をクリックすると、使用可能なプロジェクトとして表示されている **ファイル**, 、**新規**, 、次の操作:] をクリックして **ファイル**, 、] をクリックして **新規**, 、] をクリックして **プロジェクト**, 、展開 **Web**, 、] をクリックして **[Dynamic Web Project**, 、] をクリック **次**.)このチュートリアルの目的で、プロジェクトに名前を **MyACSHelloWorld**します。 (この名前を使用していることを確認してください。このチュートリアルの以降の手順では、WAR ファイルの名前が MyACSHelloWorld であるとします)。 画面は次のようになります。

    ![ACS サンプル用の Hello World プロジェクトの作成][create_acs_hello_world]

    クリックして **完了**します。
2. Eclipse の Project Explorer ビューでは、展開 **MyACSHelloWorld**します。 右クリック **WebContent**, をクリックして **新規**, 、順にクリック **JSP ファイル**します。
3.  **New JSP File** ダイアログ ボックスで、[ファイル名 **index.jsp**します。 次に示しているように、親フォルダーは MyACSHelloWorld/WebContent のままにすることに注意してください。

    ![ACS サンプル用の JSP ファイルの追加][add_jsp_file_acs]

    クリックして **次**します。

4.  **Select JSP Template** ] ダイアログ ボックスで選択 **New JSP File (html)** ] をクリック **完了**します。
5. Index.jsp ファイルが Eclipse で開いたら、表示するテキストの追加 **こんにちは ACS World!** 既存 `<body>` 要素。 更新した `<body>` の内容は次のようになります。

        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
    
    index.jsp を保存します。
  
## アプリケーションへの ACS Filter ライブラリの追加

1. Eclipse の Project Explorer で、右クリック **MyACSHelloWorld**, 、クリックして **Build Path**, 、順にクリック **Configure Build Path**します。
2.  **Java Build Path** ダイアログ ボックスで、をクリックして、 **ライブラリ** ] タブをクリックします。
3. クリックして **ライブラリを追加**します。
4. クリックして **Azure Access Control Services Filter (by MS Open Tech)** ] をクリックし、 **次**します。  **Azure Access Control Services Filter** ダイアログが表示されます。  (、 **場所** フィールドは、Eclipse のインストール場所によって、別のパスにあり、バージョン番号はソフトウェアの更新状況によって異なる可能性があります)。

    ![ACS Filter ライブラリの追加][add_acs_filter_lib]

5. ブラウザーで開いた、 **ログイン ページ統合** 示された URL のコピーである管理ポータル ページの **オプション 1: ACS ホステッド ログイン ページへのリンク** フィールドに貼り付けることにし、 **ACS Authentication Endpoint** 、Eclipse ダイアログ ボックスのフィールドです。
6. ブラウザーで開いた、 **証明書利用者アプリケーションの編集** 示された URL のコピーである管理ポータル ページの **領域** フィールドに貼り付けることにし、 **Relying Party Realm** 、Eclipse ダイアログ ボックスのフィールドです。
7. 内で、 **セキュリティ** 既存の証明書を使用して、をクリックする場合、Eclipse ダイアログ ボックスの「 **参照**, 、を使用して、選択し、[] をクリックしてする証明書に移動 **開く**します。 または、新しい証明書を作成する場合は、クリックして **新規** を表示する、 **新しい証明書** ] ダイアログ ボックスで、パスワード、.cer ファイルの名前と新しい証明書の .pfx ファイルの名前を指定します。
8. 確認 **証明書を WAR ファイルに埋め込む**します。 証明書はこの方法で埋め込むことでデプロイに追加されます。手動でコンポーネントとして追加する必要はありません (代わりに WAR ファイルから外部証明書を保存する必要がありますをロール コンポーネントとして証明書を追加し、ボックスをオフにする可能性があります **証明書を WAR ファイルに埋め込む**.)
9. [省略可能]保持 **Require HTTPS connections** チェックします。 このオプションを設定する場合は、HTTPS プロトコルを使用してアプリケーションにアクセスすることが必要になります。 HTTPS 接続が不要な場合は、このチェック ボックスをオフにします。
10. コンピューティング エミュレーターに展開する、 **Azure ACS Filter** 設定は、次のようになります。

    ![コンピューティング エミュレーターにデプロイする場合の [Azure ACS Filter] 設定][add_acs_filter_lib_emulator]

11. クリックして **完了**します。
12. クリックして **はい** web.xml ファイルが作成されることを伝えるダイアログ ボックスを表示します。
13. をクリックして **OK** を閉じる、 **Java Build Path** ダイアログ。

## コンピューティング エミュレーターへのデプロイ

1. Eclipse の Project Explorer で、右クリック **MyACSHelloWorld**, 、クリックして **Azure**, 、順にクリック **Package for Azure**します。
2.  **プロジェクト名**, 、型 **MyAzureACSProject** ] をクリック **次**します。
3. JDK とアプリケーション サーバーを選択します (次の手順での詳細については、 [Eclipse で Azure の Hello World アプリケーションを作成する](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) チュートリアル)。
4. クリックして **完了**します。
5. クリックして、 **Run in Azure Emulator** ] ボタンをクリックします。
6. Java Web アプリケーションがコンピューティング エミュレーターで起動した後、ブラウザーのすべてのインスタンスを閉じます (ブラウザー セッションが開いていると ACS ログイン テストの妨げになるためです)。
7. 開くことで、アプリケーションを実行 <http://localhost:8080/MyACSHelloWorld/> お使いのブラウザーで (または <https://localhost:8080/MyACSHelloWorld/> オンにした場合 **Require HTTPS connections**)。 Windows Live ID でログインするように求められます。その資格情報は指定した証明書利用者アプリケーションの戻り先 URL に送信されます。
99.  アプリケーションの表示が完了したら、クリックして、 **Reset Azure Emulator** ] ボタンをクリックします。

## Azure へのデプロイ

Azure にデプロイするには、ACS 名前空間の証明書利用者領域と戻り先 URL を変更する必要があります。

1. Azure の管理ポータル内で、 **証明書利用者アプリケーションの編集** ] ページで、変更 **レルム** 、デプロイするサイトの URL であります。 置換 **例** を指定する展開の DNS 名。

    ![運用で使用する証明書利用者の領域][relying_party_realm_production]

2. 変更 **戻り先 URL** 、アプリケーションの URL であります。 置換 **例** を指定する展開の DNS 名。

    ![運用で使用する証明書利用者の戻り先 URL][relying_party_return_url_production]

3. クリックして **保存** を更新した証明書利用者の領域を保存し、URL の変更を返します。
4. 保持、 **ログイン ページ統合** ブラウザーで開いたページで、すぐそこからコピーする必要があります。
5. Eclipse の Project Explorer で、右クリック **MyACSHelloWorld**, 、クリックして **Build Path**, 、順にクリック **Configure Build Path**します。
6. クリックして、 **ライブラリ** ] タブをクリックして **Azure Access Control Services Filter**, 、] をクリックし、 **編集**します。
7. ブラウザーで開いた、 **ログイン ページ統合** 示された URL のコピーである管理ポータル ページの **オプション 1: ACS ホステッド ログイン ページへのリンク** フィールドに貼り付けることにし、 **ACS Authentication Endpoint** 、Eclipse ダイアログ ボックスのフィールドです。
8. ブラウザーで開いた、 **証明書利用者アプリケーションの編集** 示された URL のコピーである管理ポータル ページの **領域** フィールドに貼り付けることにし、 **Relying Party Realm** 、Eclipse ダイアログ ボックスのフィールドです。
9. 内で、 **セキュリティ** 既存の証明書を使用して、をクリックする場合、Eclipse ダイアログ ボックスの「 **参照**, 、を使用して、選択し、[] をクリックしてする証明書に移動 **開く**します。 または、新しい証明書を作成する場合は、クリックして **新規** を表示する、 **新しい証明書** ] ダイアログ ボックスで、パスワード、.cer ファイルの名前と新しい証明書の .pfx ファイルの名前を指定します。
10. 保持 **証明書を WAR ファイルに埋め込む** オンにして、証明書を WAR ファイルに埋め込むものとします。
11. [省略可能]保持 **Require HTTPS connections** チェックします。 このオプションを設定する場合は、HTTPS プロトコルを使用してアプリケーションにアクセスすることが必要になります。 HTTPS 接続が不要な場合は、このチェック ボックスをオフにします。
12. Azure に展開する場合、[Azure ACS Filter] 設定は次のようになります。

    ![運用デプロイ用の [Azure ACS Filter] 設定][add_acs_filter_lib_production]

13. をクリックして **完了** を閉じる、 **Edit Library** ダイアログ。
14. をクリックして **OK** を閉じる、 **Properties for MyACSHelloWorld** ダイアログ。
15. Eclipse で [、 **Publish to Azure Cloud** ] ボタンをクリックします。 行うものと同様に、プロンプトに応答、 **を Azure にアプリケーションを配置する** のセクションで、 [Eclipse で Azure の Hello World アプリケーションを作成する](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) トピックです。 

Web アプリケーションのデプロイ後、開いているブラウザー セッションをすべて閉じてから、Web アプリケーションを実行します。Windows Live ID 資格情報でログインするように求められます。その資格情報は証明書利用者アプリケーションの戻り先 URL に送信されます。

後は、ACS Hello World アプリケーションを使用して、展開を削除するに注意してください (で展開を削除する方法を学習できます、 [Eclipse で Azure の Hello World アプリケーションを作成する](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) トピック)。


## <a name="next_steps"></a>次のステップ

調べるの Security Assertion Markup Language (SAML) ACS によってアプリケーションに返される場合は、次を参照してください。 [Azure Access Control サービスによって返される SAML を表示する方法][]します。 さらに ACS の機能を調べたりより高度なシナリオを試してみたりする場合、次を参照してください。 [Access Control Service 2.0][]します。

また、この例の使用、 **証明書を WAR ファイルに埋め込む** オプション。 このオプションを使用すると証明書のデプロイが簡単になります。 代わりに WAR ファイル以外からの署名証明書を保存する場合は、次の手法を使用できます。

1. 内で、 **セキュリティ** のセクションで、 **Azure Access Control Services Filter** ダイアログで、「 **${env します。JAVA_HOME}/mycert.cer** ボックスをオフに **証明書を WAR ファイルに埋め込む**します。 (証明書のファイル名が異なる場合は mycert.cer を調整)。クリックして **完了** ダイアログを閉じます。
2. 配置内のコンポーネントとして証明書をコピーする: Eclipse の Project Explorer で [ **MyAzureACSProject**, を右クリックして **WorkerRole1**, 、] をクリックして **プロパティ**, 、展開 **Azure ロール**, 、] をクリック **コンポーネント**します。
3. クリックして **追加**します。
4. 内で、 **コンポーネントの追加** ダイアログ。
    1.  **インポート** セクション。
        1. 使用して、 **ファイル** ] ボタンを使用する証明書をクリックします。 
        2.  **メソッド**, [ **コピー**します。
    2.  **名前を付けて**, でテキスト ボックスをクリックし、既定の名前をそのまま使用します。
    3.  **展開** セクション。
        1.  **メソッド**, [ **コピー**します。
        2.  **ディレクトリに**, 、型 **%java_home%**します。
    4.  **コンポーネントの追加** ダイアログ ボックスは、次のようになります。

        ![証明書コンポーネントの追加][add_cert_component]

    5. Click **OK**.

これで、証明書はデプロイに追加されました。 証明書を WAR ファイルに埋め込むか、配置にコンポーネントとして追加をたかにかかわらず必要があります」の説明に従って、名前空間に、証明書をアップロードする、 [ACS 名前空間への証明書のアップロード][] セクションです。

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Upload a certificate to your ACS namespace]: #upload-certificate
[Review the Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add the ACS Filter library to your application]: #add_acs_filter_library
[Deploy to the compute emulator]: #deploy_compute_emulator
[Deploy to Azure]: #deploy_azure
[Next steps]: #next_steps
[project website]: http://wastarterkit4java.codeplex.com/releases/view/61026
[How to view SAML returned by the Azure Access Control Service]: /en-us/develop/java/how-to-guides/view-saml-returned-by-acs/
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Azure Management Portal]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png
 


