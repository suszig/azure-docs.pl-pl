<properties
   pageTitle="Azure AD Connect 同期: Lotus Domino Connector"
   description="この記事では、Microsoft の Lotus Domino Connector を構成する方法について説明します。"
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


# Lotus Domino Connector のテクニカル リファレンス

この記事では Lotus Domino Connector について説明します。 この記事は次の製品に適用されます。

- Microsoft Identity Manager 2016 (MIM2016)
- Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   4.1.3461.0 修正プログラムを使用する必要がありますか、後で [KB2870703](https://support.microsoft.com/kb/2870703)します。

MIM2016 および FIM2010R2、コネクタはからダウンロードできる、 [Microsoft ダウンロード センター](http://go.microsoft.com/fwlink/?LinkId=717495)します。

## Lotus Domino Connector の概要

Lotus Domino Connector を使用すると、IBM の Lotus Domino サーバーと同期サービスを統合することができます。

包括的な観点から見た場合、コネクタの現在のリリースでサポートされている機能は次のとおりです。

| 機能| サポート|
| --- | --- |
| 接続先のデータ ソース| サーバー: <li>ロータス ドミノ 8.5.x</li><li>ロータス ドミノ 9.x</li>クライアント:<li>Lotus Notes 8.5.x</li><li>Lotus Notes 9.x</li>|
| シナリオ| <li>オブジェクトのライフ サイクル管理</li><li>グループ管理</li><li>パスワード管理</li>|
| 操作| <li>完全および差分インポート</li><li>エクスポート</li><li>を設定し、HTTP のパスワードのパスワードを変更</li>|
| スキーマ| <li>ユーザー (ローミング ユーザー、連絡先 (ユーザー証明書がありません))</li><li>グループ</li><li>リソース (リソース、室、オンライン会議)</li><li>データベース メールに</li><li>サポートされているオブジェクトの属性の動的な検出</li>|

Lotus Domino Connector は、Lotus Domino サーバーとの通信に、Lotus Notes クライアントを使用します。 そのため、同期サーバーサポートされている Lotus Notes クライアントをインストールする必要があります。 クライアントとサーバー間の通信は、Lotus Notes .NET Interop (Interop.domino.dll) インターフェイスを介して実装されています。 このインターフェイスにより、Microsoft.NET プラットフォームと Lotus Notes 間の通信は促され、Lotus Domino ドキュメントおよびビューにアクセスできるようになります。 (選択している差分インポートの方法に応じて) 差分インポートで C++ ネイティブ インターフェイスを使用することも可能です。

### 前提条件

コネクタを使用する前に、前述した修正プログラムに加えて次のものが同期サーバーにインストールされていることを確認してください。

- Microsoft .NET 4.5.2 Framework 以降
- 同期サーバーへの Lotus Notes クライアントのインストール
- Lotus Domino Connector では、Domino Directory サーバー上に既定の Lotus Domino LDAP スキーマ データベース (schema.nsf) が必要です。 存在することを確認してください。 存在しない場合、Domino サーバーで LDAP サービスを実行または再起動して、インストールします。

### 接続先のデータ ソースのアクセス権

Lotus Domino Connector でサポートされている任意のタスクを実行するには、次のグループのメンバーである必要があります。

- フル アクセスを持つ管理者
- 管理者
- データベース管理者

次の表に、各操作に必要なアクセス権を示します。

| 操作| アクセス権|
| --- | --- |
| インポート| <li>パブリックのドキュメントを読み取る</li><li> フル アクセス管理者 (フル アクセスの管理者グループのメンバーでは、自動的にしたらへのアクセスを有効な ACL でします)。</li>|
| エクスポートおよびパスワードの設定| 有効なアクセス: <li>ドキュメントを作成する</li><li>ドキュメントを削除する</li><li>パブリックのドキュメントを読み取る</li><li>パブリックのドキュメントを書き込む</li><li>複製またはコピーのドキュメント</li>上記のアクセスだけでなく、エクスポート操作の次の役割を割り当てる必要があります: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li>|

### 直接の操作および AdminP

操作は、Domino ディレクトリに直接実行されるか、AdminP プロセスを介して実行されます。 次にサポートされているすべてのオブジェクト、操作、そして存在する場合は、関連する実装方法を示します。

**プライマリ アドレス帳**

| オブジェクト| 作成| 更新| 削除|
| --- | --- | --- | --- |
| Person| AdminP| 直接| AdminP|
| グループ| AdminP| 直接| AdminP|
| MailInDB| 直接| 直接| 直接|
| リソース| AdminP| 直接| AdminP|

**セカンダリ アドレス帳**

| オブジェクト| 作成| 更新| 削除|
| --- | --- | --- | --- |
| Person| 該当なし| 直接| 直接|
| グループ| 直接| 直接| 直接|
| MailInDB| 直接| 直接| 直接|
| リソース| 該当なし| 該当なし| 該当なし|

リソースが作成されると、Notes ドキュメントが作成されます。 同様にリソースが削除されると、Notes ドキュメントが削除されます。

### ポートとプロトコル

IBM Lotus Notes クライアントおよび Domino サーバーは、Notes Remote Procedure Call (NRPC) を使用して通信します。このとき、NRPC では TCP/IP が使用される必要があります。 既定のポート番号は 1352 ですが、これは Domino 管理者が変更することができます。

### サポートされていません

次の操作は、Lotus Domino Connector の現在のリリースではサポートされていません。

- サーバー間でのメールボックスの移動。

## 新しいコネクタの作成

### クライアント ソフトウェアのインストールおよび構成

Lotus Notes は、コネクタのインストール**前に**、サーバーにインストールする必要があります。

インストールする際には、必ず **[Single User Install]** を実行してください。 既定の **[Multi-User Install]** は機能しません。

![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

機能のページでは、必要な Lotus Notes 機能および **[Client Single Logon]** のみをインストールします。 コネクタが Domino サーバーにログオンするには、シングル ログオンが必要です。

![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**注:** Lotus Notes は、一度はコネクタのサービス アカウントとして使用するアカウントと同じサーバーにあるユーザーが開始する必要があります。

### コネクタの作成

Lotus Domino Connector を作成するには、**Synchronization Service** で **[管理エージェント]** を選択し、**[作成]** を選択します。 **[Lotus Domino (Microsoft)]** のコネクタを選択します。

![コネクタの作成](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

使用している同期サービスのバージョンで**アーキテクチャ**を設定できる場合、**プロセス**で実行できるようコネクタがその既定に設定されていることを確認します。

### 接続

[接続] ページで、Lotus Domino サーバー名を指定し、ログオン資格証明を入力します。

![接続](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Domino Server のプロパティでは、サーバー名に次の 2 つの形式がサポートされています。

- ServerName
- ServerName/DirectoryName

コネクタと Domino Server が通信する際、応答速度が速いため、この属性には **ServerName/DirectoryName** の形式が推奨されます。

提供されたユーザー Id のファイルは、同期サービスの構成データベースに格納されます。

**差分インポート**には、次のオプションがあります。

- **[なし]**。 コネクタは差分インポートを実行しません。
- **[追加/更新]**。 コネクタは、追加および更新操作を差分インポートします。 削除については、**[フル インポート]** 操作が必要です。 この操作では、Notes の .Net interop が使用されます。
- **[追加/更新/削除]**。 コネクタは、追加、更新および削除操作を差分インポートします。 この操作では、Notes のネイティブ C++ インターフェイスが使用されます。

**[スキーマ オプション]** には次のオプションがあります。

- **[既定のスキーマ]**。 既定のオプションで、コネクタは Domino サーバーからスキーマを検出します。
- **[DSML スキーマ]**。 Domino サーバーがスキーマを公開しない場合のみ使用されます。 この場合、スキーマを使用して作成した DSML ファイルを代わりにインポートします。 DSML の詳細については、次を参照してください。 [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml)します。

[次へ] をクリックすると UserID とパスワード構成のパラメーターが検証されます。

### グローバル パラメーター

[グローバル パラメーター] ページでは、タイム ゾーンと、インポートおよびエクスポート操作のオプションを構成します。

![グローバル パラメーター](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

**[Domino サーバーのタイム ゾーン]** パラメーターでは Domino サーバーの場所を定義します。

この構成オプションは、最後の 2 回のインポート間の変更を同期サービスが特定するのを可能にするため、**差分インポート**操作で必要です。

#### インポート設定および方法

**[フル インポートの実行方法]** には次のオプションがあります。

- 検索
- ビュー (推奨)

**[検索]** は Domino のインデックスを使用しますが、一般にインデックスはリアルタイムで更新されず、サーバーから返されるデータが常に正しいとは限りません。 このオプションは、変更が多いシステムではうまく機能しないことが多く、場合によっては削除が不正に行われることがあります。 ただし、**[検索]** の方が **[ビュー]** よりも高速です。

データの正しい状態を示す **[ビュー]** オプションが推奨されます。 これは [検索] より若干動作が遅くなります。

#### Virtual Contact オブジェクトの作成

**\_Contact オブジェクトの作成を有効にする** はこれらのオプションがあります。

- なし
- 非参照値
- 参照および非参照値

Domino の参照属性には、その他のオブジェクトを参照するさまざまな形式があります。 さまざまなバリエーションを表すことができる、コネクタを実装して \_Contact オブジェクトとも呼ばれます 仮想連絡先 (VC)。 これらのオブジェクトは、既存の MV オブジェクトに結合して、新しいオブジェクトとしてプロジェクションし、この方法で参照属性を保持できるよう作成されています。

この設定を有効にし、参照属性のコンテンツが DN の形式でない場合は、\_Contact オブジェクトが作成します。 たとえば、グループのメンバー属性に SMTP アドレスを含めることができます。 また、参照属性に shortName およびその他のオブジェクトを含めることができます。 このシナリオでは、**[非参照値]** を選択します。 これが Domino の実装で最も一般的な設定です。

Lotus Domino が同じオブジェクトを表す別の識別名を持つ別のアドレス帳に構成されている場合も \_Contact オブジェクトは、アドレス帳にあるすべての参照値を作成することができます。 このシナリオでは、**[参照および非参照値]** オプションを選択します。

Domino の属性 **FullName** に複数の値がある場合、参照が解決されるように Virtual Contacts の作成も有効にします。 この属性は、たとえば結婚や離婚の後に複数の値を持つことが可能です。 チェック ボックスをオン **... を有効にするFullName は、複数の値を持つ** このシナリオでします。

適切な属性で結合するには、\_Contact オブジェクトを MV オブジェクトに参加するは。

VC がこれらのオブジェクトには = \_Contact の DN に追加します。

#### インポート設定および競合オブジェクト

**競合するオブジェクトの除外**

大規模な Domino の実装では、レプリケーションの問題により、複数のオブジェクトの DN が同じである場合があります。 このような場合、コネクタには UniversalID は同じですが DN は異なる 2 つのオブジェクトが示されます。 これにより、コネクタ スペースに一時オブジェクトが作成されます。 コネクタは、Domino で選択したオブジェクトは、レプリケーション上作成されたものとして無視できます。 このチェックボックスはオンにすることをお勧めします。

#### 設定のエクスポート

**[参照の更新に AdminP を使用する]** オプションがオンになっていない場合、メンバーなどの参照属性のエクスポートは、AdminP プロセスは使用しない、直接での呼び出しになります。 これは、参照整合性を維持するために AdminP が構成されていない場合のみ使用します。

#### ルーティング情報

Domino の参照属性には、DN へのルーティング情報をサフィックスとして埋め込むことが可能です。たとえば、グループの member 属性に **CN=example/organization@ABC** を含めることが可能です。サフィックス @ABC がルーティング情報です。このルーティング情報は、Domino が (別の組織のシステムである場合もある) 正しい Domino システムに電子メールを送信する際に使用します。[ルーティング情報] フィールドには、コネクタのスコープで組織が使用しているルーティング サフィックスを指定します。これらのいずれかの値が参照属性のサフィックスとして検出される場合、参照からルーティング情報は削除され、コネクタ スペースのオブジェクトの DN と一致するようになります。参照値のルーティングのサフィックスの指定されているいずれかに一致する場合は、\_Contact オブジェクトが作成されます。これらの \_Contact オブジェクトが作成されます * * RO = @<RoutingSuffix>* * DN に挿入します。これらの \_Contact オブジェクトを次の属性は、必要に応じて、実際のオブジェクトへの参加を許可するようにも追加されます: \_routingName、\_contactName、\_displayName、および UniversalID です。

#### その他のアドレス帳

セカンダリ アドレス帳の名前を提供する **Directory Assistance** がインストールされていない場合、これらのアドレス帳を手動で入力できます。

#### 複数値の変換

Lotus Domino の属性の多くには値が複数あります。 対応するメタバース属性の値は、通常 1 つです。 インポートおよびエクスポート操作オプションを構成すると、影響を受ける属性の変換をコネクタが支援するようになり、構成が簡単になります。

**エクスポート**

エクスポート操作には、次の 2 つのモードがあります。

- 項目の追加
- 項目の置換

**項目の置換**: このオプションを選択すると、コネクタは常に Domino の属性の現在の値を削除し、指定された値に置き換えます。 指定する値は、1 つでも複数でも可能です。

例:
Person オブジェクトの Assistant 属性値は以下のとおりです。

- CN=Greg Winston/OU= Contoso /O=Americas,NAB=names.nsf
- CN=John Smith/OU= Contoso /O=Americas,NAB=names.nsf

**David Alexander** という名前の Assistant がこの Person オブジェクトに割り当てられる場合、次のような結果になります。

- CN=David Alexander/OU= Contoso /O=Americas,NAB=names.nsf

**項目の置換**: このオプションを選択すると、コネクタは Domino の属性の既存の値を保持し、データ リストの上部に新しい値を挿入します。

例:
Person オブジェクトの Assistant 属性値は以下のとおりです。

- CN=Greg Winston/OU= Contoso /O=Americas,NAB=names.nsf
- CN=John Smith/OU= Contoso /O=Americas,NAB=names.nsf

**David Alexander** という名前の Assistant がこの Person オブジェクトに割り当てられる場合、次のような結果になります。

- CN=David Alexander/OU= Contoso /O=Americas,NAB=names.nsf
- CN=Greg Winston/OU= Contoso /O=Americas,NAB=names.nsf
- CN=John Smith/OU= Contoso /O=Americas,NAB=names.nsf

**インポート**

エクスポート操作には、次の 2 つのモードがあります。

- 既定値
- 複数値から単一値

**既定**: 既定のオプションを選択すると、すべての属性のすべての値がインポートされます。

**複数値から単一値**: このオプションを選択すると、複数値の属性は単一値の属性に変換されます。 値が 1 つ以上存在する場合、上にある値 (通常これが最新の値です) が使用されます。

例:

Person オブジェクトの Assistant 属性値は以下のとおりです。

- CN=David Alexander/OU= Contoso /O=Americas,NAB=names.nsf
- CN=Greg Winston/OU= Contoso /O=Americas,NAB=names.nsf
- CN=John Smith/OU= Contoso /O=Americas,NAB=names.nsf

この属性に対する最新の更新は **David Alexander** です。 [インポート操作] オプションは、[複数値から単一値] に設定されているので、コネクタはコネクタ スペースに **David Alexander** のみをインポートします。

複数の値を持つ属性を単一値の属性に変換するロジックは、group オブジェクトの member 属性および Person オブジェクトの fullname 属性には使用できません。

グローバル規則に、例外として、複数の値を持つ属性に属性ごとのインポートおよびエクスポート変換規則を構成することも可能です。 このオプションを構成するには、[objecttype].[attributename] を **[インポートしない属性の一覧]** と **[エクスポートしない属性の一覧]** テキスト ボックスに入力します。 たとえば、すべての値をインポートするようにグローバル フラグが設定されている場合に Person.Assistant と入力したとき、最初の値のみがアシスタント用にインポートされます。

#### 証明者

すべての組織および組織単位が一覧表示されています。 プライマリ アドレス帳に Person オブジェクトをエクスポートするには、証明者とそのパスワードが必要です。

すべての証明者のパスワードが同一である場合、**[すべての証明者用パスワード]** を使用できます。 その後、ここにパスワードを入力し、証明者ファイルのみを指定します。

インポートのみを行う場合、証明者は指定する必要がありません。

### プロビジョニング階層の構成

Lotus Domino Connector を構成する場合は、このダイアログ ページをスキップできます。 これは、Lotus Domino Connector が階層のプロビジョニングをサポートしていないためです。

![プロビジョニング階層](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### パーティションと階層の構成

パーティションおよび階層を構成する場合、NAB=names.nsf というプライマリ アドレス帳を選択する必要があります。

存在する場合は、プライマリ アドレス帳のみでなく、セカンダリ アドレス帳を選択することも可能です。

![パーティション](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### 属性の選択

属性を構成する場合が付いているすべての属性を選択する必要があります **\_MMS\_**します。 これらの属性は、Lotus Domino に新しいオブジェクトをプロビジョニングする際に必要です。

![属性](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## オブジェクトのライフサイクル管理

このセクションでは、Domino のさまざまなオブジェクトの概要を説明します。

### Person オブジェクト

Person オブジェクトとは、組織と組織単位内のユーザーです。 Domino 管理者は、既定の属性以外にカスタム属性を Person オブジェクトに追加できます。 Person オブジェクトには、すべての必須属性を最低でも含んでいる必要があります。 必須の属性の一覧については、次を参照してください。 [Lotus Notes プロパティ](#lotus-notes-properties)します。 Person オブジェクトを登録する場合、次の前提条件を満たしている必要があります。

- アドレス帳 (names.nsf) が定義されており、それがプライマリ アドレス帳になっています。
- 組織および組織単位に特定のユーザーを登録するには、組織および組織単位の証明者 ID とパスワードが必要です。
- Person オブジェクトには、特定の Lotus Notes のプロパティ セットを定義する必要があります。 これらは、Person オブジェクトのプロビジョニングで使用されます。 詳細については、というセクションを参照してください。 [Lotus Notes プロパティ](#lotus-notes-properties) このドキュメントで後述します。
- Person 用の初期 HTTP パスワードは属性で、プロビジョニング時に設定されます。
- Person オブジェクトは、次の 3 つのサポートされている種類のいずれかである必要があります。
    1. メール ファイルおよびユーザー ID ファイルを持つ標準ユーザー
    2. ローミング ユーザー (すべてのローミング データベース ファイルを含む標準ユーザー)
    3. 連絡先 (ID ファイルを持たないユーザー)

(連絡先) を除く担当者は、米国のユーザーとインターナショナル ユーザーに \_MMS\_IDRegType プロパティの値で定義されているグループにさらに化できます。 これらは、Lotus Domino サーバー、データベースへのアクセスに Notes Client を使用する、Notes ID と Person ドキュメントを持つユーザーです。 Notes メールを使用している場合、メール ファイルも持っています。 ユーザーは、登録済みでないとアクティブにはなりません。 詳細情報

- [ノートのユーザーを設定します。](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
- [ユーザーの登録](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
- [ユーザーを管理します。](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
- [ユーザーの名前を変更します。](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

これらのすべての操作は Lotus Domino で実行された後に、同期サービスにインポートされます。

### リソースおよびルーム

リソースは Lotus Domino のもう 1 種類のデータベースです。 リソースは、プロジェクターなどのさまざまな種類の機器がある会議室である場合があります。 リソースには、リソースの種類の属性に基づいて定義される Lotus Domino Connector によってサポートされるそのサブタイプがあります。

| リソースの種類| リソースの種類の属性|
| --- | --- |
| ルーム| 1|
| リソース (その他)| 2|
| オンライン会議| 3|

リソース オブジェクトの種類が機能するには、次が必要です。

- Resource Reservation データベースが接続されている Domino サーバーに存在します。
- リソース用にサイトが定義済みです。

Resource Reservation データベースには、3 種類のドキュメントが含まれています。

- サイト プロファイル
- リソース
- 予約

データベースのリソースの予約を設定する方法の詳細を参照してください、 [リソース予約データベースを設定する](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)します。

**リソースの作成、更新、および削除**

作成、更新、および削除操作は、Resource Reservation データベース内で Lotus Domino コネクタによって実行されます。 この結果、リソースは Names.nsf (つまり、プライマリ アドレス帳) にドキュメントとして作成されます。 編集や、リソースの削除の詳細については、次を参照してください。 [の編集とリソースのドキュメントを削除する](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html)します。

**リソースのインポートおよびエクスポート操作**

リソースは、他の種類のオブジェクトと同様に、同期サービスからインポートおよびエクスポートできます。 オブジェクトの種類は、構成時にリソースとして選択する必要があります。 正しくエクスポートするには、リソースの種類、会議データベース、およびサイト名の詳細が必要です。

### メールイン データベース

メールイン データベースは、メールを受信するために設計されたデータベースです。 これは (独自の ID ファイルおよびパスワードを持たない) 特定の Lotus Domino ユーザー アカウントに関連付けられていない Lotus Domino のメールボックス (メール テンプレートに基づく Notes データベース) です。 メールイン データベースには、メールイン データベースとそれ独自のメール アドレスに関連付けられている一意の UserID ("省略名") があります。

各ユーザーには Lotus Domino アカウントは、1 つのみ必要です。 複数ユーザー間で共有できる、独自の電子メール アドレスを持つ別のメールボックスの必要性があるかどうか (例:: group@contoso.com)、ノートのアカウントを追加する代わりにメールでデータベースを作成します。 このメールボックスへのアクセスは、自身の ID ファイルおよびパスワードを使用して開くことが許可されている Notes ユーザーの名前が含まれたアクセス制御リスト (ACL) を介して制御されています。 メールイン データベースのアクセスには、別のパスワードは必要ありません。

必要な属性の一覧は、というセクションを参照してください。 [必須属性](#mandatory-attributes) この記事で後述します。

データベースがメールを受信するよう設計されている場合、Lotus Domino にメールイン データベースが作成されます。 このドキュメントは、データベースのコピーを格納するすべてのサーバーの Domino Directory で必要です。 データベース メールで文書の作成についての詳細な説明を参照してください。 [データベース メールにドキュメントを作成する](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html)します。

メールイン データベースを作成する前に、(Lotus の管理者が作成した) データベースが Domino サーバーに既に存在している必要があります。

### グループ管理

Lotus Domino のグループ管理の詳細な説明については、次を参照してください。

- [グループを使用します。](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
- [グループを作成します。](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
- [作成とグループの変更](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
- [グループを管理します。](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
- [グループ名の変更](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### パスワード管理

登録済みの Lotus Domino ユーザーには、次の 2 種類のパスワードがあります。

1. (User.id ファイルに格納されている) ユーザー パスワード
2. インターネット/HTTP パスワード

Lotus Domino Connector では、HTTP パスワードでの操作のみサポートしています。

パスワードを管理するには、Management Agent Designer でコネクタのパスワード管理を有効にする必要があります。 パスワード管理を有効にするには、**[拡張機能の構成]** ダイアログ ページで **[パスワードの管理を有効にする]** を選択してください。

![拡張機能の構成](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Lotus Domino Connector では、インターネット パスワードの次の操作をサポートしています。

- パスワードの設定: パスワードの設定を使用すると、Domino ユーザーに新しい HTTP およびインターネット パスワードを設定できます。 既定ではアカウントのロックは解除されます。 ロック解除のフラグは、同期エンジンの WMI インターフェイスで公開されています。
- パスワードの変更: これは、ユーザーがパスワードの変更を希望したり、指定した期間後にパスワードの変更が求めらたりするシナリオ用です。 この操作を実行するには、(新旧のパスワードの) 両方が必須です。 変更後、Lotus Domino で新しいパスワードが更新されます。

詳細については、次を参照してください。

- [インターネットのロックアウトの機能を使用します。](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
- [インターネット パスワードを管理します。](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## 参照情報

このセクションでは、Lotus Domino Connector の属性および属性の要件について説明します。

### Lotus Notes のプロパティ

Lotus Domino ディレクトリに Person オブジェクトをプロビジョニングする際、オブジェクトには特定の値が入った特定のプロパティのセットが必要です。 これらの値は、Create 操作でのみ必要です。

これらのプロパティとその説明は、次の表で示します。

| プロパティ| 説明|
| --- | --- |
| \_MMS_AltFullName| ユーザーの別の完全名。|
| \_MMS_AltFullNameLanguage| ユーザーの別の完全名を指定するために使用する言語。|
| \_MMS_CertDaysToExpire| 証明書が有効期限切れになる前の現在の日付からの日数。これを指定しない場合、既定の日付は現在の日から 2 年後になります。|
| \_MMS_Certifier| 認証者の組織階層名が格納されたプロパティ。例: OU=OrganizationUnit,O=Org,C=Country。|
| \_MMS_IDPath| このプロパティが空の場合、ユーザー ID ファイルが同期サーバーのローカルに作成されません。プロパティにファイル名が含まれる場合、ユーザー ID ファイルがメタデータ フォルダーに作成されます。このプロパティには、ユーザー ID ファイルが作成された場所の完全なパスを含めることも可能です。|
| \_MMS_IDRegType| Person は、連絡先、米国ユーザーおよび海外ユーザーに分類できます。次の表に、使用可能な値:<li>0 - 連絡先</li><li>1 - u. s. ユーザー</li><li>2 - 各国対応ユーザー</li>|
| \_MMS_IDStoreType| 米国および海外ユーザーに必須のプロパティ。このプロパティには、ユーザー ID を Notes のアドレス帳に添付ファイルとして格納するか、ユーザーのメール ファイルに格納するかを指定する整数値が含まれます。ユーザー ID のファイルが、アドレス帳の添付ファイルの場合は、\_MMS_IDPath のファイルとして必要に応じて作成できます。<li>空の ID、資格情報コンテナー (連絡先に使用される) なしの識別ファイル内の店舗 ID ファイルです。</li><li>1: Notes アドレス帳の添付ファイル。添付ファイルであるユーザーの id のファイルには、\_MMS_Password プロパティを設定する必要が</li><li>2 - ユーザーのメール ファイルのストアの ID。\_MMS_UseAdminP は、ユーザー登録時に作成するメール ファイルを false に設定する必要があります。\_MMS_Password プロパティは、ユーザーの id のファイルを設定する必要があります。</li>
| \_MMS_MailQuotaSizeLimit| 電子メール ファイル データベースに許可されるメガバイト数での領域。|
| \_MMS_MailQuotaWarningThreshold| 警告が発行される前に電子メール ファイル データベースに許可されるメガバイト数での領域。|
| \_MMS_MailTemplateName| ユーザーの電子メール ファイルの作成に使用される電子メール テンプレート ファイル。テンプレートを指定すると、指定したテンプレートを使用してメール ファイルが作成されます。テンプレートを指定しない場合、ファイルの作成に既定のテンプレート ファイルが使用されます。|
| \_MMS_OU| 証明者の下の OU 名である省略可能なプロパティ。連絡先の場合、このプロパティは空になっている必要があります。|
| \_MMS_Password| ユーザーに必須のプロパティ。このプロパティには、オブジェクトの ID ファイルのパスワードが含まれます。|
| \_MMS_UseAdminP| Domino サーバーの AdminP プロセスでメール ファイルを作成する場合、プロパティは true に設定する必要があります (エクスポート プロセスに対して非同期)。プロパティを false に設定する場合、メール ファイルは Domino ユーザーと共に作成されます (エクスポート プロセスで同期)。|

関連付けられている識別ファイルを持つユーザー、\_MMS_Password プロパティは、値を含める必要があります。 Lotus Notes クライアントを使用した電子メールでのアクセスの場合、ユーザーの MailServer および MailFile プロパティには値を含める必要があります。

Web ブラウザーを介して電子メールにアクセスするには、次のプロパティに値がある必要があります。

- MailFile: メール ファイルが格納されている Lotus Domino サーバーのパスが含まれる必須プロパティです。
- MailServer: Lotus Domino サーバーの名前を含む必須プロパティです。 Domino サーバー上に Lotus メール ファイルを作成するときに、使用する名前です。
- HTTPPassword: オブジェクトの Web アクセス用のパスワードを含む省略可能なプロパティです。

メール機能なしに Domino Server にアクセスするには、HTTPPassword プロパティには値が必要で、MailFile プロパティおよび MailServer プロパティは空にできます。

\_MMS_ IDStoreType = 2 (メール ファイル内の店舗 id)、NotesRegistrationclass の MailSystem プロパティは、REG_MAILSYSTEM_INOTES (3) に設定されます。

### 必須の属性

Lotus Domino Connector は、同期サービスで主に次の 4 種類のオブジェクト (ドキュメントの種類) をサポートしています。

- グループ
- メールイン データベース
- Person
- 連絡先 (証明者のない Person)
- リソース

このセクションでは、Domino サーバーへのオブジェクトのエクスポート用にサポートされている各種類のオブジェクトに必須の属性について説明します。

| オブジェクトの種類| 必須の属性|
| --- | --- |
| グループ| <li>ListName</li>|
| メールイン データベース| <li>FullName</li><li>MailFile</li><li>メール サーバー</li><li>MailDomain</li>|
| Person| <li>LastName</li><li>MailFile</li><li>ShortName</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>_MMS_UseAdminP</li>|
| 連絡先 (証明者のない Person)| <li>\_MMS_IDRegType</li>|
| リソース| <li>FullName</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Site</li><li>DisplayName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>|

## お問い合わせの多い問題および質問

### スキーマが検出できない

スキーマを検出するには、Domino サーバーに schema.nsf ファイルが存在している必要があります。 このファイルは、サーバー上に LDAP がインストールされている場合のみ表示されます。 スキーマを検出できない場合は、次を確認してください。

- ファイル schema.nsf が Domino サーバーのルート フォルダーにあります。
- ユーザーに schema.nsf ファイルを参照する権限があります。
- LDAP サーバーを再起動します。 'Lotus Domino コンソール' を開き、'Tell LDAP ReloadSchema' コマンドを使用して、スキーマを再度読み込みます。

### すべてのセカンダリ アドレス帳が表示されない

Domino Connector は、セカンダリ アドレス帳の検索を Directory Assistance 機能に依存しています。 セカンダリ アドレス帳が存在しない場合を確認してください場合 [案内](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) が有効にされ、ドミノ サーバーで構成します。

### Domino のカスタム属性

Domino には、コネクタで使用できるカスタム属性など、スキーマを拡張するいくつかの方法があります。

**方法 1: Lotus Domino スキーマの拡張**

1. ドミノ ディレクトリのテンプレートのコピーを作成 <PUBNAMES.NTF> に従って [手順](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (する必要がありますをカスタマイズしない既定の IBM Lotus Domino ディレクトリ テンプレート)。
2. コピーのドミノ ディレクトリ テンプレートを開く <CONTOSO.NTF> Domino デザイナーやフォローでのみ作成するテンプレート、以下の手順。
    - [Shared Elements] をクリックし、[Subforms] を展開します。
    - $[ObjectName]InheritableSchema サブフォームをダブルクリックします (ここで [ObjectName] は既定の構造上のオブジェクト クラス名です。例: Person)。
    - スキーマ [MyPersonAtrribute] に追加する属性の名前を対応するメニューの [作成] を選択して、フィールドを作成し、メニューから ['Field' を選択します。
    - 追加したフィールドの [Properties] ウィンドウで、その種類、スタイル、サイズ、フォントおよびその他の関連パラメーターを選択して、そのプロパティを設定します。
    - 属性の既定値は、その属性の名前と同じにします (例: 属性名が MyPersonAttribute の場合は、既定値は同じ名前にします)。
    - 更新値での $[ObjectName]InheritableSchema サブフォームの保存
3. ドミノ ディレクトリのテンプレートを置き換える <PUBNAMES.NTF> 新しいカスタム テンプレート [CONTOSO です。NTF] に従って [手順](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html)します。
4. Domino Admin を閉じ、Domino コンソールを開いて LDAP サービスを再起動して、LDAP スキーマを再度読み込みます。
    - ドミノ コンソールで [コマンドの挿入 **Domino コマンド** - LDAP サービスを再起動するために提出テキスト [再起動タスク LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)します。
    - LDAP スキーマを再度読み込むために Tell LDAP コマンドの Tell LDAP ReloadSchema を使用します。
5. Domino Admin を開き、[People & Groups] タブを選択し、追加した属性が Domino の [Add Person] に反映されていることを確認します。
6. [Files] タブから Schema.nsf を開き、追加した属性が dominoPerson LDAP オブジェクト クラスに反映されていることを確認します。

**方法 2: は、カスタム属性を使用し auxClass を作成し、オブジェクト クラスと関連付ける**

1. ドミノ ディレクトリ テンプレート [PUBNAMES のコピーを作成します。NTF] に従って [手順]()http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html (既定の IBM Lotus Domino ディレクトリ テンプレートをカスタマイズすることはありません)。
2. 作成したばかりの Domino ディレクトリ テンプレート [CONTOSO.NTF] テンプレートのコピーを Domino Designer で開きます。
3. 左のウィンドウから [Shared Code]、[Subforms] の順に選択します。
4. [New Subform] をクリックします。
5. 次を実行し、新しいサブフォームのプロパティを指定します。
    - 新しいサブフォームが開いたら、[Design - Subform Properties] を選択します。
    - [Name] プロパティの横に、TestSubform など、補助オブジェクト クラスの名前を入力します。
    - [Options] プロパティの [Include in Insert Subform... dialog] のチェックボックスはオンのままにします。
    - [Options] プロパティの [Render pass through HTML in Notes] のチェックボックスはオフにします。
    - その他のプロパティはそのままにし、[Subform Properties] ボックスを閉じます。
    - 新しいサブフォームを保存し閉じます。
6. 次を実行し、補助オブジェクト クラスを定義するフィールドを追加します。
    - 作成したばかりのサブフォームを開きます。
    - [Create]、[Field] の順に選択します。
    - [Field] ダイアログ ボックスの [Basics] タブで [Name] の横に、[MyPersonTestAttribute] などの任意の名前を指定します。
    - 追加したフィールドの種類、スタイル、サイズ、フォントおよびその他の関連パラメーターを選択して、そのプロパティを設定します。
    - 属性の既定値は、その属性の名前と同じにします (例: 属性名が MyPersonTestAttribute の場合は、既定値は同じ名前にします)。
    - 更新された値でサブフォームを保存し、次を実行します。
        - 左のウィンドウから [Shared Code]、[Subforms] の順に選択します。
        - 新しいサブフォームを選択し、[Design - Design Properties] を選択します。
        - 左から 3 番目のタブをクリックし、**[Propagate this prohibition of design change]** を選択します。
7. $[ObjectName]ExtensibleSchema サブフォームを開きます (ここで [ObjectName] は、Person などの既定の構造オブジェクト クラスです)。
8. リソースを挿入し、サブフォーム (TestSubform など作成したばかりのもの) を選択し、$[ObjectName]ExtensibleSchema サブフォームを保存します。
9. ドミノ ディレクトリ テンプレート [PUBNAMES を置き換えます。NTF] 新しいカスタム テンプレート [CONTOSO です。NTF] に従って [手順](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html)します。
10. Domino Admin を閉じ、Domino コンソールを開いて LDAP サービスを再起動して、LDAP スキーマを再度読み込みます。
    - ドミノ コンソールで [コマンドの挿入 **Domino コマンド** - LDAP サービスを再起動するために提出テキスト [再起動タスク LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)します。
    - LDAP スキーマを再度読み込むために Tell LDAP コマンドの Tell LDAP ReloadSchema を使用します。
11. Domino Admin を開き、[People & Groups] タブを選択し、追加した属性が Domino の ([Others] タブ下の) [Add Person] に反映されていることを確認します。
12. **[Files]** タブから Schema.nsf を開き、追加した属性が TestSubform LDAP 補助オブジェクト クラスに反映されていることを確認します。

**方法 3: ExtensibleObject クラスへのカスタム属性の追加**

1. ルート ディレクトリにある [Schema.nsf] ファイルを開きます。
2. 左のメニューの **[All Schema Documents]** の下の [Select LDAP Object Classes] を選択し、**[Add Object class]** ボタンをクリックします。
3. [###ExtensibleSchema] の形式で、スキーマを拡張するオブジェクトへの参照と共に LDAP 名を指定します (ここで ### は Person などの既定の構造オブジェクトの名前です)。 たとえば、Person オブジェクト クラスのスキーマを拡張するには、LDAP 名 [PersonExtensibleSchema] を提供します。
4. スキーマを拡張する上位のオブジェクト クラス名を入力します。 たとえば、Person オブジェクト クラスのスキーマを拡張するには、上位オブジェクト クラス名 [dominoPerson] を入力します。
5. オブジェクト クラスに対応する有効な OID を入力します。
6. 必要に応じて、[Mandatory Attribute Types] または [Optional Attribute Types] フィールドの下で [Extended attributes] または [custom attributes] を選択します。
7. 必要なオブジェクトを ExtensibleObjectClass に追加した後、**[Save & Close]** ボタンをクリックします。
8. ExtensibleObjectClass が、拡張属性を使用して該当する既定のオブジェクト クラス用に作成されます。

## トラブルシューティング

-   コネクタのトラブルシューティングを行うへのログ記録を有効にする方法については、次を参照してください。、 [コネクタの ETW トレースを有効にする方法](http://go.microsoft.com/fwlink/?LinkId=335731)します。





