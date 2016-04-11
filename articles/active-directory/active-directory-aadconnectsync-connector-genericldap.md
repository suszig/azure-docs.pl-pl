<properties
   pageTitle="Azure AD Connect sync: Generic LDAP Connector"
   description="この記事では、Microsoft の Generic LDAP コネクタを構成する方法について説明します。"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="12/16/2015"
   ms.author="andkjell"/>

# Generic LDAP コネクタに関するテクニカル リファレンス

この記事では Generic LDAP コネクタについて説明します。 この記事は次の製品に適用されます。

- Microsoft Identity Manager 2016 (MIM2016)
- Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   4.1.3461.0 修正プログラムを使用する必要がありますか、後で [KB2870703](https://support.microsoft.com/kb/2870703)します。

MIM2016 および FIM2010R2、コネクタはからダウンロードできる、 [Microsoft ダウンロード センター](http://go.microsoft.com/fwlink/?LinkId=717495)します。

IETF RFC を参照する場合、このドキュメントでは形式 (RFC [RFC number]/[section in RFC document]) を使用します。例: (RFC 4512/4.3)。
Http://tools.ietf.org/html/rfc4500 (4500 を正しい RFC 番号に置き換える必要があります) に詳細が表示されます。

## Generic LDAP コネクタの概要

Generic LDAP コネクタを使用すると、同期サービスを LDAP v3 サーバーと統合することができます。

IETF RFC では、特定の操作やスキーマ要素 (たとえば、差分インポートを実行する上で必要なもの) は指定されていません。 これらの操作では、明示的に指定された LDAP ディレクトリのみがサポートされます。

包括的な観点から見た場合、コネクタの現在のリリースでサポートされている機能は次のとおりです。

| 機能 | サポート |
| --- | --- |
| 接続先のデータ ソース | コネクタはすべての LDAP v3 サーバー (RFC 4510 準拠) でサポートされています。 これは、次のようにテストされています。 <li>Microsoft Active Directory ライトウェイト ディレクトリ サービス (AD LDS)</li><li>Microsoft Active Directory グローバル カタログ (AD GC)</li><li>389 Directory Server</li><li>Apache Directory Server</li><li>IBM Tivoli DS</li><li>Isode Directory</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Open DJ</li><li>Open DS</li><li>Open LDAP (openldap.org)</li><li>Oracle (以前は Sun) Directory Server Enterprise Edition</li><li>RadiantOne Virtual Directory Server (VDS)</li><li>Sun One Directory Server</li>注目すべきディレクトリがサポートされていません。 <li>Microsoft Active Directory ドメイン サービス (AD DS) [代わりに組み込みの Active Directory コネクタを使用]</li><li>Oracle インターネット ディレクトリ (OID)</li> |
| シナリオ | <li>オブジェクトのライフサイクル管理</li><li>グループ管理</li><li>パスワード管理</li> |
| 操作 |すべての LDAP ディレクトリには、次の操作がサポートされています。 <li>フル インポート</li><li>エクスポート</li>次の操作は、指定されたディレクトリに対してのみサポートされます。<li>差分インポート</li><li>パスワードの設定、パスワードの変更</li> |
| スキーマ | <li>LDAP スキーマ (RFC3673 と RFC4512/4.2) からスキーマが検出されました</li><li>構造型クラス、aux クラス、および extensibleObject オブジェクト クラス (RFC4512/4.3) をサポートしています</li>

### 差分インポートおよびパスワード管理のサポート

差分インポートおよびパスワード管理がサポートされているディレクトリは次のとおりです。

- Microsoft Active Directory ライトウェイト ディレクトリ サービス (AD LDS)
    - 差分インポートのすべての操作をサポート
    - パスワードの設定とパスワードの変更をサポート
- Microsoft Active Directory グローバル カタログ (AD GC)
    - 差分インポートのすべての操作をサポート
    - パスワードの設定とパスワードの変更をサポート
- 389 Directory Server
    - 差分インポートのすべての操作をサポート
    - パスワードの設定をサポート
- Apache Directory Server
    - このディレクトリには永続的な変更ログがないため差分インポートはサポートしていません。
    - パスワードの設定をサポート
- IBM Tivoli DS
    - 差分インポートのすべての操作をサポート
    - パスワードの設定とパスワードの変更をサポート
- Isode Directory
    - 差分インポートのすべての操作をサポート
    - パスワードの設定とパスワードの変更をサポート
- Novell eDirectory and NetIQ eDirectory
    - 差分インポートの追加、更新、および名前変更操作をサポート
    - 差分インポートの削除操作はサポートしていません。
    - パスワードの設定とパスワードの変更をサポート
- Open DJ
    - 差分インポートのすべての操作をサポート
    - パスワードの設定をサポート
- Open DS
    - 差分インポートのすべての操作をサポート
    - パスワードの設定をサポート
- Open LDAP (openldap.org)
    - 差分インポートのすべての操作をサポート
    - パスワードの設定をサポート
    - パスワードの変更はサポートしていません。
- Oracle (以前は Sun) Directory Server Enterprise Edition
    - 差分インポートのすべての操作をサポート
    - パスワードの設定とパスワードの変更をサポート
- RadiantOne Virtual Directory Server (VDS)
    - 7.1.1 以上のバージョンを使用する必要があります。
    - 差分インポートのすべての操作をサポート
    - パスワードの設定とパスワードの変更をサポート
-  Sun One Directory Server
    - 差分インポートのすべての操作をサポート
    - パスワードの設定とパスワードの変更をサポート

### 前提条件

コネクタを使用する前に、前述した修正プログラムに加えて次のものが同期サーバーにインストールされていることを確認してください。

- Microsoft .NET 4.5.2 Framework 以降

### LDAP サーバーの検出

コネクタは、さまざまな技法に基づいて、LDAP サーバーを検出し識別します。 コネクタでは、ルート DSE を使用してベンダー名とバージョンを探し出し、スキーマを調べて LDAP サーバーに存在している既知の一意のオブジェクトおよび属性を見つけます。 このデータは、見つかると、コネクタでの構成オプションの事前設定に使用されます。

### 接続先のデータ ソースのアクセス権

接続されたディレクトリ内のオブジェクトに対してインポートおよびエクスポート操作を実行するには、それに必要なアクセス許可がコネクタのアカウントになければなりません。 コネクタは、エクスポートするためには書き込みアクセス許可を、インポートするためには読み取りアクセス許可を備えている必要があります。 アクセス許可は、ターゲット ディレクトリ自体の管理エクスペリエンス内で構成されます。

### ポートとプロトコル

コネクタでは、構成で指定されたポート番号を使用します。既定では、LDAP の場合は 389、LDAPS の場合は 636 が使用されます。

LDAPS では、SSL 3.0 または TLS を使用する必要があります。 SSL 2.0 はサポートされていないため、アクティブ化できません。

### 必要なコントロールと機能

コネクタが正常に動作するためには、LDAP サーバー上で次の LDAP コントロール/機能を利用できる必要があります。

- 1.3.6.1.4.1.4203.1.5.3 True/False フィルター

True または False のフィルターは、LDAP ディレクトリでサポートされている、頻繁には報告されずに表示される可能性があります、 **グローバル ページ** [ **必須機能が見つかりません。**します。 作成に使用される **または** たとえば複数のオブジェクトの種類をインポートするときに、LDAP クエリでフィルターします。 複数の種類のオブジェクトをインポートできる場合に、LDAP サーバーでサポートされます。

一意の識別子が、アンカーは、ディレクトリを使用する場合、次も使用できなければなりません (を参照してください、 [構成アンカー](#configure-anchors) 詳細については、この記事で後述する「)。

- 1.3.6.1.4.1.4203.1.5.1 すべての操作属性

1 回のディレクトリ呼び出しに収まり切れない数のオブジェクトがディレクトリにある場合は、ページングの使用をお勧めします。 ページングが機能するには、次のオプションのいずれか 1 つが必要です。

**方法 1:**

- 1.2.840.113556.1.4.319 pagedResultsControl

**方法 2:**

- 2.16.840.1.113730.3.4.9 VLVControl
- 1.2.840.113556.1.4.473 SortControl

コネクタの構成で両方のオプションが有効になっている場合は、pagedResultsControl が使用されます。

- 1.2.840.113556.1.4.417 ShowDeletedControl

ShowDeletedControl は USNChanged 差分インポート方法で使用できるだけであり、削除されたオブジェクトを確認できます。

コネクタは、サーバーに上記オプションがあるかどうか、検出を試みます。 オプションを検出できなかった場合、コネクタのプロパティ内のグローバル ページに警告が表示されます。 LDAP のすべてが、サポートしているコントロール/機能をすべて提示しているとは限らないため、この警告が表示されてもコネクタは問題なく動作する場合があります。

### 差分インポート

差分インポートは、サポート ディレクトリが検出された場合にのみ使用できます。 現在、次の方法が使用されています。

- LDAP アクセスログ。 参照してください [http://www.openldap.org/doc/admin24/overlays.html#Access ログ](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
- LDAP 変更ログ。 参照してください [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
- タイムスタンプ。 Novell/NetIQ eDirectory の場合、コネクタは最後の日付/時刻を使用して、作成されたオブジェクトおよび更新されたオブジェクトを取得します。 Novell/NetIQ eDirectory の場合、同等の方法を使用して、削除されたオブジェクトを取得することはできません。 タイムスタンプ オプションはまた、LDAP サーバー上で他にアクティブになっている差分インポート方法がない場合に使用することができます。 このオプションを使用して、削除されたオブジェクトをインポートすることはできません。
- USNChanged。 参照: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### サポートされていません

次の LDAP 機能はサポートされていません。

- サーバー間の LDAP 参照 (RFC 4511/4.1.10)

## 新しいコネクタの作成

汎用的な LDAP コネクタを作成する **同期サービス** 選択 **管理エージェント** と **作成**します。 選択、 **汎用的な LDAP (マイクロソフト)** コネクタです。

![コネクタの作成](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### 接続

[接続] ページで、ホスト、ポート、バインディングに関する情報を指定する必要があります。 選択したバインディングによっては、次のセクションに追加の情報が表示される場合があります。

![接続](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

- 接続タイムアウトの設定は、スキーマの検出時に行われるサーバーへの最初の接続でのみ使用されます。
- バインディングが匿名である場合は、ユーザー名/パスワードも証明書も使用されません。
- その他のバインディングの場合は、ユーザー名/パスワードに情報を入力するか、または証明書を選択します。
- Kerberos を使用して認証する場合は、ユーザーの領域/ドメインを指定します。

 **属性の別名** RFC4522 構文を使用して、スキーマで定義されている属性のテキスト ボックスを使用します。 スキーマの検出中にこれらの属性を検出することはできないため、コネクタがそれらの属性を識別できるようにサポートが必要です。 たとえば、バイナリ属性として userCertificate 属性を正しく識別するには、[属性の別名] ボックスに次を入力する必要があります。

`userCertificate;binary`

これがどのように表示されるかを次に示します。

![接続](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

選択、 **運用上の属性をスキーマに含める** も、サーバーによって作成された属性を含めるチェック ボックスをオンします。 これには、オブジェクトの作成日時および最終更新日時などの属性が含まれます。

選択 **スキーマに拡張可能な属性を含める** 拡張可能なオブジェクト (RFC4512/4.3) を使用し、このオプションを有効にすると、すべてのオブジェクトで使用されるすべての属性を許可するかどうか。 このオプションを選択すると、スキーマの規模が非常に大きくなります。接続先のディレクトリがこの機能を使用していない限り、このオプションは非選択のままにしておくことをお勧めします。

### グローバル パラメーター

[グローバル パラメーター] ページで、DN を差分変更ログおよびその他の LDAP 機能に構成します。 このページには、LDAP サーバーから提供された情報があらかじめ設定されています。

![接続](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

上部セクションには、サーバーの名前など、サーバーから提供された情報が表示されます。 コネクタでは、必須のコントロールがルート DSE に存在していることも確認します。 該当するコントロールが一覧されていない場合は、警告が表示されます。 LDAP ディレクトリの中には、一部の機能しかルート DSE に一覧表示しないものがあります。このため、警告が存在する場合でも、コネクタは問題なく動作する場合があります。

 **コントロールがサポートされている** のチェック ボックスは、特定の操作の動作を制御します。

- ツリー削除を選択すると、1 回の LDAP 呼び出しで階層が削除されます。 ツリー削除を選択していない場合、コネクタは必要に応じて再帰削除を実行します。
- 結果のページングが選択されている場合、コネクタは実行の手順で指定されたサイズに従ってページング インポートを行います。
- VLVControl および SortControl は、pagedResultsControl に代わる、LDAP ディレクトリからのデータの読み取り方法です。
- 3 つのオプション (pagedResultsControl、VLVControl、および SortControl) がいずれも選択されていない場合、コネクタは 1 回の操作であらゆるオブジェクトをインポートします。この場合、オブジェクトが大規模なディレクトリであると、失敗する可能性があります。
- ShowDeletedControl は、差分インポート方法が USNChanged である場合にのみ使用されます。

DN の変更ログなどの差分変更のログで使用される名前付けコンテキストは、 **cn = について**します。 差分インポートを実行するには、この値を指定する必要があります。

既定の変更ログ DN の一覧を次に示します。

| Directory | 差分変更ログ |
| --- | --- |
| Microsoft AD LDS および AD GC | 自動的に検出されます。 USNChanged。 |
| Apache Directory Server | 使用できません。 |
| Directory 389 | 変更ログ。 既定値を使用する: **cn = について** |
| IBM Tivoli DS | 変更ログ。 既定値を使用する: **cn = について** |
| Isode Directory | 変更ログ。 既定値を使用する: **cn = について**
| Novell/NetIQ eDirectory | 使用できません。 タイムスタンプ。 コネクタは、前回の更新の日時を使用して、追加されたレコードおよび更新されたレコードを取得します。 |
| Open DJ/DS | 変更ログ。  既定値を使用する: **cn = について** |
| Open LDAP | アクセス ログ。 既定値を使用する: **cn = accesslog** |
| Oracle DSEE | 変更ログ。 既定値を使用する: **cn = について** |
| RadiantOne VDS | 仮想ディレクトリ。 VDS に接続されているディレクトリに依存します。 |
| Sun One Directory Server | 変更ログ。 既定値を使用する: **cn = について** |

パスワード属性は、パスワード変更およびパスワード設定の操作でパスワードを設定する場合に、コネクタで使用する必要がある属性の名前です。
これは、既定で、 **userPassword** LDAP の特定のシステムに必要な場合に変更することができます。

追加のパーティションの一覧に加えて、自動的に検出されない名前空間を追加することもできます。 これは、複数のサーバーが 1 つの論理クラスターを構成していて、すべて同時にインポートする必要がある場合などに、使用することができます。 Active Directory では 1 つのフォレスト内に複数のドメインを保持することができますが、ドメインはすべて 1 つのスキーマを共有します。それと同じことを、このボックスに追加の名前空間を入力することでシミュレートできます。 それぞれの名前空間は、さまざまなサーバーからインポートすることができ、[パーティションと階層の構成] ページで追加の構成を行うことができます。 新しい行を取得するには Ctrl + Enter キーを使用します。

### プロビジョニング階層の構成

このページは、プロビジョニングが必要なオブジェクト タイプ (organizationalUnit など) に DN コンポーネント (OU など) をマップする場合に使用します。

![プロビジョニング階層](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

プロビジョニング階層を構成することによって、必要に応じて自動的に構造体を作成するようコネクタを構成できます。 たとえば、名前空間 dc=contoso,dc=com が存在し、新しいオブジェクト cn=Joe, ou=Seattle, c=US, dc=contoso, dc=com がプロビジョニングされている場合、コネクタでは、US の type country と Seattle の organizationalUnit がまだディレクトリ内に存在していない場合、それらから成る新しいオブジェクトを作成することができます。

### パーティションと階層の構成

パーティションおよび階層のページでは、インポートおよびエクスポートするオブジェクトを含んでいるすべての名前空間を選択します。

![パーティション](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

名前空間ごとに、[接続] 画面で指定した値をオーバーライドする接続設定を構成することもきます。 これらの値が既定の空白値のままである場合は、接続画面からの情報が使用されます。

コネクタでは、インポート元のコンテナーおよび OU と、エクスポート先のコンテナーおよび OU を選択することもできます。

### アンカーの構成

このページには、あらかじめ構成された値が常に表示されるため、ページの内容を変更することはできません。 サーバーのベンダーとバージョンが明らかになっている場合、このページには変更できない属性 (オブジェクトの GUID など) が設定されている場合があります。 サーバーのベンダーとバージョンが検出されていないか、または、それには変更できない属性がないことがわかっている場合、コネクタはアンカーとして dn (識別名) を使用します。

![アンカー](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)

使用する LDAP サーバーとアンカーの一覧を次に示します。

| Directory | アンカー属性 |
| --- | --- |
| Microsoft AD LDS および AD GC | objectGUID |
| 389 Directory Server | dn |
| Apache Directory | dn |
| IBM Tivoli DS | dn |
| Isode Directory | dn |
| Novell/NetIQ eDirectory | GUID |
| Open DJ/DS | dn |
| Open LDAP | dn |
| Oracle ODSEE | dn |
| RadiantOne VDS | dn |
| Sun One Directory Server | dn |

## 他の注意事項

このセクションでは、このコネクタに固有の特徴、または他の理由で理解しておく必要のある特徴を示します。

### 差分インポート

Open LDAP の差分ウォーターマークは、UTC 日付/時刻です。 このため、FIM 同期サービスと Open LDAP の間で時間を同期させる必要があります。 そうしないと、差分変更ログの一部のエントリが省略される場合があります。

Novell eDirectory の場合、差分インポートでオブジェクトの削除は検出されません。 このため、削除されたすべてのオブジェクトを検索するには、フル インポートを定期的に実行する必要があります。

日付/時刻に基づく差分変更ログを備えるディレクトリの場合は、フル インポートを定期的に実行して LDAP サーバーとコネクタ スペースに現在存在するものとの相違点を見つけることを強くお勧めします。

## トラブルシューティング

-   コネクタのトラブルシューティングを行うへのログ記録を有効にする方法については、次を参照してください。、 [コネクタの ETW トレースを有効にする方法](http://go.microsoft.com/fwlink/?LinkId=335731)します。


