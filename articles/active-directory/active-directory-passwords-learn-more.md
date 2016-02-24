<properties 
    pageTitle="詳細情報: Azure AD のパスワード管理 | Microsoft Azure" 
    description="パスワード ライトバックのしくみ、パスワード ライトバックのセキュリティ、パスワード リセット ポータルのしくみ、パスワードのリセットで使用されるデータなど、Azure AD のパスワード管理に関する高度なトピック。" 
    services="active-directory" 
    documentationCenter="" 
    authors="asteen" 
    manager="kbrint" 
    editor="billmath"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/16/2015" 
    ms.author="asteen"/>

# パスワード管理の詳細情報
既にパスワード管理をデプロイしている場合、またはデプロイする前に技術的な動作方法についてさらにしく知りたい場合は、サービスの背景技術の概念について、このセクションで概要を説明します。 次の内容を説明します。

* [**パスワード ライトバックの概要**](#password-writeback-overview)
  - [パスワード ライトバックのしくみ](#how-password-writeback-works)
  - [パスワード ライトバックでサポートされているシナリオ](#scenarios-supported-for-password-writeback)
  - [パスワード ライトバックのセキュリティ モデル](#password-writeback-security-model)
* [**パスワード リセット ポータルのしくみ**](#how-does-the-password-reset-portal-work)
  - [パスワードのリセットで使用されるデータ](#what-data-is-used-by-password-reset)
  - [ユーザーのパスワード リセット データにアクセスする方法](#how-to-access-password-reset-data-for-your-users)

## パスワード ライトバックの概要
パスワード ライトバックは、 [Azure Active Directory Connect](active-directory-aadconnect) コンポーネントを有効になっているし、Azure Active Directory Premium の現行のサブスクライバーで使用されることができます。 詳細については、次を参照してください。 [Azure Active Directory のエディション](active-directory-editions.md)します。 

パスワード ライトバックを使用すると、オンプレミスの Active Directory にパスワードを書き戻すようにクラウド テナントを構成できます。  これにより、複雑なオンプレミスのセルフサービス パスワード リセット ソリューションを設定して管理する必要がなくなります。ユーザーはどこにいても、便利なクラウドベースの方法で自分のオンプレミスのパスワードをリセットできます。  次に、パスワード ライトバックの主な機能を示します。

- **ゼロ遅延フィードバック。**パスワード ライトバックは同期操作です。  ユーザーのパスワードがポリシーに合わなかった場合や、何らかの理由でリセットまたは変更できなかった場合は、すぐにユーザーに通知します。
- **AD FS または他のフェデレーション技術を使用して、ユーザーのパスワードをリセットできます。**パスワード ライトバック使うと、フェデレーション ユーザー アカウントと Azure AD テナントが同期している限り、クラウドからオンプレミスの AD パスワードを管理できます。
- **パスワード ハッシュ同期を使用して、ユーザーのパスワードをリセットできます。**パスワード リセット サービスは、同期されているユーザー アカウントでパスワード ハッシュ同期が有効になっていることを検出すると、このアカウントのオンプレミスのパスワードとクラウドのパスワードの両方を同時にリセットします。
- **アクセス パネルと Office 365 からパスワードの変更をサポートします。**フェデレーション ユーザーかパスワード同期されたユーザーが有効期限切れまたは有効期限切れでないパスワードを変更すると、これらのパスワードはローカル AD 環境に書き戻されます。
- **管理者からリセットする場合は、パスワードをライトバックをサポート、** [**Azure 管理ポータル**](https://manage.windowsazure.com)します。  管理者がユーザーのパスワードをリセットするときに、 [Azure 管理ポータル](https://manage.windowsazure.com), 、そのユーザーがフェデレーションされる場合、またはパスワード同期は、ローカル AD でも、管理者が選択したパスワードを設定します。  現時点で、これは Office の管理ポータルではサポートされていません。
- **オンプレミスの強制 AD パスワード ポリシー。**ユーザーが自分のパスワードをリセットする場合、パスワードがオンプレミスの AD ポリシーに準拠していることを確認してから、そのディレクトリにコミットします。  このポリシーには、履歴、複雑さ、年齢、パスワード フィルター、ローカル AD で定義された他のパスワード制限が含まれます。
- **受信のファイアウォール規則は必要ありません。**パスワード ライトバックは、基礎となる通信チャネルとして Azure Service Bus Relay を使用します。つまり、この機能を使用するためにファイアウォールで受信ポートを開く必要はありません、
- **内部設置型 Active Directory で保護されたグループ内に存在するユーザー アカウントに対してサポートされていません。**保護グループの詳細については、次を参照してください。 [保護アカウントおよび Active Directory でグループ](https://technet.microsoft.com/library/dn535499.aspx)します。

### パスワード ライトバックのしくみ
パスワード ライトバックには次の 3 つの主要コンポーネントがあります。

- パスワード リセットのクラウド サービス (これは Azure AD のパスワード変更ページにも統合されています)
- テナント固有の Azure Service Bus Relay
- オンプレミスのパスワード リセット エンドポイント

これらは、以下の図で説明するように統合されています。

  ![][001]

フェデレーション ユーザーまたはパスワード ハッシュ同期されたユーザーがクラウドで自分のパスワードをリセットまたは変更する場合は、次の処理が行われます。

1.  ユーザーのパスワードの種類が確認されます。  パスワードがオンプレミスで管理されていることが確認されると、ライトバック サービスが起動し、実行します。  オンプレミスで管理されている場合は続行しますが、管理されていない場合は、パスワードをリセットできないことがユーザーに通知されます。
2.  次に、ユーザーが適切な認証ゲートを通過すると、パスワードのリセット画面が表示されます。
3.  ユーザーは新しいパスワードを選択して確認します。
4.  送信をクリックすると、プレーンテキスト パスワードがライトバックのセットアップ プロセス時に作成された対称キーを使用して暗号化されます。
5.  パスワードは暗号化された後、ペイロードに含められ、HTTPS チャネル経由でテナント固有の Service Bus Relay (ライトバックのセットアップ中にも設定される) に送信されます。  このリレーは、オンプレミスのインストールのみを認識するランダムに生成されたパスワードによって保護されます。
6.  メッセージが Service Bus に到達すると、パスワード リセット エンドポイントが自動的にアクティブになり、保留中のリセット要求があるかどうかを確認します。
7.  サービスは、クラウドのアンカー属性を使用して該当するユーザーを探します。  この検索が正常に動作するには、ユーザー オブジェクトが AD コネクタ スペースに存在し、これが対応する MV オブジェクトにリンクし、さらにこれが対応する AAD コネクタ オブジェクトにリンクする必要があります。 最後に、同期によってこのユーザー アカウントを検索するには、AD コネクタ オブジェクトから MV へのリンクに `Microsoft.InfromADUserAccountEnabled.xxx` 同期ルールが含まれている必要があります。  これが必要なのは、クラウドから呼び出される場合に、同期エンジンは cloudAnchor 属性を使用して AAD コネクタ スペース オブジェクトを検索し、MV オブジェクトのリンクに戻り、次に AD オブジェクトのリンクに戻るためです。 同じユーザーに対して複数の AD オブジェクト (マルチ フォレスト) がある可能性があるため、同期エンジンは `Microsoft.InfromADUserAccountEnabled.xxx` のリンクに依存して正しいユーザー アカウントを選択します。
8.  ユーザー アカウントが見つかると、適切な AD フォレスト内で直接パスワードのリセットを試みます。
9.  パスワードの設定操作に成功すると、ユーザーにパスワードが変更されたことが通知され、そのまま続行されます。
10. パスワードの設定操作に失敗した場合は、エラーが返され、やり直す必要があります。  サービスがダウンした、選択したパスワードが組織のポリシーを満たしていない、ローカル AD でユーザーが見つからないなど、さまざまな原因で操作に失敗する可能性があります。  多くの場合、特定のメッセージが表示され、問題解決の手段がユーザーに通知されます。

### パスワード ライトバックでサポートされているシナリオ
次の表では、同期機能のバージョンでサポートされているシナリオについて説明します。  一般の最新バージョンをインストールすることを強くお勧めします。 [Azure AD Connect](active-directory-aadconnect.md#download-azure-ad-connect) パスワード ライトバックを使用する場合。

  ![][002]

### パスワード ライトバックのセキュリティ モデル
パスワード ライトバックは、安全性と信頼性の高いサービスです。  ユーザーの情報を確実に保護するために、以下で説明する 4 層のセキュリティ モデルを実現しています。

- **テナント固有の service bus relay** –、サービスを設定すると Microsoft へのアクセスがランダムに生成された強力なパスワードで保護されているテナント固有の service bus リレーを設定します。
- **ロックダウンされ、暗号強度の高いパスワード暗号化キー** – サービス バス リレーを作成した後、ネットワーク経由で渡されるパスワードの暗号化を使用して、強力な対称キーを作成します。  このキーは、クラウド内の会社のシークレット ストアのみに存在し、厳重にロックダウンされ、ディレクトリ内のパスワードと同様に監査されます。
- **業界標準の TLS** – パスワード リセットや変更操作は、クラウド、プレーン テキスト パスワードが取得し、公開キーで暗号化します。  次に、これを HTTPS メッセージにして、マイクロソフトの SSL 証明書を使用して暗号化されたチャネルを介して Service Bus Relay に送信されます。  このメッセージが Service Bus に到着すると、オンプレミスのエージェントがアクティブになり、前に生成された強力なパスワードを使用して Service Bus に認証され、暗号化されたメッセージを取得し、生成された秘密キーを使用して復号化され、AD DS SetPassword API を使用してパスワードの設定を試みます。  この手順に従うと、クラウドで AD オンプレミスのパスワード ポリシー (複雑さ、年齢、履歴、フィルターなど) を適用できます。
- **メッセージの有効期限ポリシー** – 最後に、メッセージは、何らかの理由でオンプレミス サービスがダウンしているために Service Bus に置かれ場合は、タイムアウトにされ、さらにセキュリティを強化するために、数分後に削除します。

## パスワード リセット ポータルのしくみ
ユーザーがパスワード リセット ポータルに移動すると、そのユーザー アカウントは有効か、そのユーザーはどの組織に属しているか、そのユーザーのパスワードはどこに管理されているか、ユーザーは機能を使用するライセンスが付与されているかを判断するためのワークフローが開始されます。  パスワード リセット ページの背後にあるロジックの詳細については、次の手順をお読みください。

1.  ユーザーがクリックする、アクセスできませんが、アカウントのリンクに直接 [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com)します。
2.  ユーザーは、ユーザー ID を入力し、CAPTCHA を渡します。
3.  Azure AD は、次の手順を行うことで、ユーザーがこの機能を使用できるかどうかを確認します。
    - ユーザーがこの機能を有効にしていて、Azure AD ライセンスが割り当てられていることを確認します。
        - ユーザーがこの機能を有効にしていない、またはライセンスが割り当てられていない場合、そのユーザーは管理者に連絡してパスワードをリセットするように求められます。
    - ユーザーは、管理者ポリシーに従って、自分のアカウントに正しいチャレンジ データが定義されていることを確認します。
        - ポリシーが 1 つのチャレンジのみを要求する場合は、管理者ポリシーで有効になっている 1 つ以上のチャレンジに対して、ユーザーが適切なデータを定義していることが確認されます。
          - ユーザーが構成されていない場合、そのユーザーは管理者に連絡してパスワードをリセットするように求められます。
        - ポリシーが 2 つのチャレンジを要求する場合は、管理者ポリシーで有効になっている 2 つ以上のチャレンジに対して、ユーザーが適切なデータを定義していることが確認されます。
          - ユーザーが構成されていない場合、そのユーザーは管理者に連絡してパスワードをリセットするように求められます。
    - ユーザーのパスワード (フェデレーションまたはパスワード ハッシュ同期された) がオンプレミスで管理されているかどうかを確認します。
       - ライトバックがデプロイされていて、ユーザーのパスワードがオンプレミスで管理されている場合、ユーザーは自分のパスワードを認証してリセットできます。
       - ライトバックがデプロイされておらず、ユーザーのパスワードがオンプレミスで管理されている場合、ユーザーは管理者に連絡してパスワードをリセットするように求められます。
4.  ユーザーが自分のパスワードを正常にリセットできると判断された場合は、リセット プロセスの説明がユーザーに示されます。

パスワード ライトバックを展開する方法の詳細について [概要: Azure AD パスワード管理](active-directory-passwords-getting-started.md)します。

### パスワードのリセットで使用されるデータ
次の表は、このデータがパスワードのリセット時に使用される場所と方法を示し、組織に適した認証オプションを決定する際に役立つように作られています。 この表では、このデータを検証しない入力パスからユーザーに代わってデータを指定する場合の形式の要件も示しています。

> [AZURE.NOTE] ユーザーは現在、ディレクトリ内には、このプロパティを編集できないために、勤務先電話番号は、登録ポータルでは表示されません。

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>連絡方法</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Azure Active Directory のデータ要素</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>使用される場所 / 設定可能な場所</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>形式の要件</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>会社電話</p>
            </td>
            <td>
              <p>PhoneNumber</p>
              <p>例: Set-MsolUser -UserPrincipalName JWarner@contoso.com -PhoneNumber "+1 1234567890x1234"</p>
            </td>
            <td>
              <p>使用される場所: </p>
              <p>パスワード リセット ポータル</p>
              <p>設定可能な場所: </p>
              <p>PhoneNumber は、PowerShell、DirSync、Microsoft Azure 管理ポータル、Office の管理ポータルから設定可能です</p>
            </td>
            <td>
              <p>+ccc xxxyyyzzzz (例: +1 1234567890)</p>
              <ul>
                <li class="unordered">
                                        Must provide a country code<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Must provide an area code (where applicable)<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Must have provide a + in front of the country code<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Must have a space between country code and the rest of the number<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Extensions are not supported, if you have any extensions specified, we will strip it from the number before dispatching the phone call.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>携帯電話</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>または</p>
              <p>MobilePhone</p>
              <p>(認証用電話はデータが存在する場合に使用されます。それ以外の場合は、携帯電話フィールドが使用されます)。</p>
              <p>例: Set-MsolUser -UserPrincipalName JWarner@contoso.com -MobilePhone "+1 1234567890x1234"</p>
            </td>
            <td>
              <p>使用される場所: </p>
              <p>パスワード リセット ポータル</p>
              <p>登録ポータル</p>
              <p>設定可能な場所:  </p>
              <p>AuthenticationPhone は、パスワード リセット登録ポータルまたは MFA 登録ポータルから設定可能です。</p>
              <p>MobilePhone は、PowerShell、DirSync、Microsoft Azure 管理ポータル、Office の管理ポータルから設定可能です</p>
            </td>
            <td>
              <p>+ccc xxxyyyzzzz (例: +1 1234567890)</p>
              <ul>
                <li class="unordered">
                                        Must provide a country code.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Must provide an area code (where applicable).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Must have provide a + in front of the country code.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Must have a space between country code and the rest of the number.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Extensions are not supported, if you have any extensions specified, we ignore it when dispatching the phone call.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>連絡用メール アドレス</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>または</p>
              <p>AlternateEmailAddresses [0] </p>
              <p>(認証用電子メールはデータが存在する場合に使用されます。それ以外の場合は、連絡用メール アドレス フィールドが使用されます)</p>
              <p>注: 連絡用メール アドレス フィールドは、ディレクトリ内の文字列の配列として指定されます。  この配列内の最初のエントリが使用されます。</p>
              <p>例: Set-MsolUser -UserPrincipalName JWarner@contoso.com -AlternateEmailAddresses "email@live.com"</p>
            </td>
            <td>
              <p>使用される場所: </p>
              <p>パスワード リセット ポータル</p>
              <p>登録ポータル</p>
              <p>設定可能な場所:  </p>
              <p>AuthenticationEmail は、パスワード リセット登録ポータルまたは MFA 登録ポータルから設定可能です。</p>
              <p>AlternateEmail は、PowerShell、Microsoft Azure 管理ポータル、Office の管理ポータルから設定可能です。</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a> または甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
                                        Emails should follow standard formatting as per .<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Unicode emails are supported.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>セキュリティの質問と回答</p>
            </td>
            <td>
              <p>ディレクトリ内で直接変更することはできません。</p>
            </td>
            <td>
              <p>使用される場所: </p>
              <p>パスワード リセット ポータル</p>
              <p>登録ポータル </p>
              <p>設定可能な場所:  </p>
              <p>セキュリティの質問は、Microsoft Azure 管理ポータルからのみ設定可能です。</p>
              <p>特定のユーザーからのセキュリティに関する質問への回答は、登録ポータルからのみ設定可能です。</p>
            </td>
            <td>
              <p>セキュリティの質問は、最小 3 文字 ～ 最大 200 文字で指定します。</p>
              <p>回答は、最小 3 文字 ～ 最大 40 文字で指定します。</p>
            </td>
          </tr>
        </tbody></table>

###ユーザーのパスワード リセット データにアクセスする方法
####同期によって設定できるデータ
次のフィールドは、オンプレミスから同期することができます。

* 携帯電話
* 会社電話

####Azure AD PowerShell を使用して設定できるデータ
次のフィールドは、Azure AD PowerShell と Graph API を使用してアクセスすることができます。

* 連絡用メール アドレス
* 携帯電話
* 会社電話
* 認証用電話
* 認証用メール

####登録 UI でのみ設定できるデータ
次のフィールドでは、大文字と小文字に SSPR 登録 UI (https://aka.ms/ssprsetup) によるアクセスのみ。

* セキュリティの質問と回答

####ユーザーの登録時に発生すること
[登録] ページには、ユーザーが登録時に **常に** 、次のフィールドを設定します。

* 認証用電話
* 認証用メール
* セキュリティの質問と回答

値を指定した場合 **携帯電話** または **連絡用電子メール**, 、ユーザーすぐに使用できます、自分のパスワードをリセットする場合でも、サービスに登録していない、します。  さらに、これらの値は、ユーザーが初めて登録するときに表示され、ユーザーは必要に応じてそれらを変更することができます。  ただし、登録が成功する後これらの値は永続化で、 **認証用電話** と **認証用電子メール** フィールドに、それぞれします。

これは、SSPR を使用できるように多数のユーザーをブロック解除すると同時に、ユーザーが登録プロセス中にこの情報を確認できるようにする便利な方法として利用できます。

####PowerShell を使用したパスワード リセット データの設定
次のフィールドの値は、Azure AD PowerShell を使用して設定することができます。

* 連絡用メール アドレス
* 携帯電話
* 会社電話

最初に、最初にする必要があります [をダウンロードして、Azure AD PowerShell モジュールをインストール](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)します。  インストールした後、次の手順に従って各フィールドを構成できます。

#####連絡用メール アドレス
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####携帯電話
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####会社電話
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####PowerShell を使用したパスワード リセット データの読み取り
次のフィールドの値は、Azure AD PowerShell を使用して読み取ることができます。

* 連絡用メール アドレス
* 携帯電話
* 会社電話
* 認証用電話
* 認証用メール

最初に、最初にする必要があります [をダウンロードして、Azure AD PowerShell モジュールをインストール](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)します。  インストールした後、次の手順に従って各フィールドを構成できます。

#####連絡用メール アドレス
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####携帯電話
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####会社電話
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####認証用電話
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####認証用メール
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## パスワードのリセットに関するドキュメントへのリンク
Azure AD のパスワードのリセットに関するすべてのドキュメント ページへのリンクを以下に示します。 

* [**自分のパスワードをリセット**](active-directory-passwords-update-your-own-password.md) -リセットまたはシステムのユーザーとして、自分のパスワードを変更する方法について説明
* [**そのしくみ**](active-directory-passwords-how-it-works.md) -サービスとの 6 つの異なるコンポーネントについてそれぞれの機能
* [**作業の開始**](active-directory-passwords-getting-started.md) -ユーザーをリセットしたり、クラウドまたはオンプレミスのパスワード変更を許可する方法について
* [**カスタマイズ**](active-directory-passwords-customize.md) -、外観と、組織のニーズに合わせてサービスの動作をカスタマイズする方法について
* [**ベスト プラクティス**](active-directory-passwords-best-practices.md) -を迅速に展開し、組織内のパスワードを効果的に管理する方法について説明
* [**情報を把握**](active-directory-passwords-get-insights.md) -統合レポート機能について
* [**よく寄せられる質問**](active-directory-passwords-faq.md) -よく寄せられる質問に対する回答を得る
* [**トラブルシューティング**](active-directory-passwords-troubleshoot.md) -サービスに問題を迅速にトラブルシューティングする方法について説明



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"

