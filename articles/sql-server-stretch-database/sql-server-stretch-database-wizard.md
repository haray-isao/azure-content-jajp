<properties
	pageTitle="[Stretch Database を有効にする] ウィザードを実行する方法の概要 | Microsoft Azure"
	description="[Stretch Database を有効にする] ウィザードを実行し、Stretch Database のデータベースを設定する方法について説明します。"
	services="sql-server-stretch-database"
	documentationCenter=""
	authors="douglaslMS"
	manager=""
	editor=""/>

<tags
	ms.service="sql-server-stretch-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="06/14/2016"
	ms.author="douglasl"/>

# [Stretch Database を有効にする] ウィザードを実行する方法の概要

Stretch Database のデータベースを設定するには、[Stretch Database を有効にする] ウィザードを実行します。このトピックでは、ウィザードで入力し、選択する情報について説明します。

Stretch Database の詳細については、「[Stretch Database](sql-server-stretch-database-overview.md)」を参照してください。

## ウィザードを起動する

1.  SQL Server Management Studio のオブジェクト エクスプローラーで、Stretch を有効にするデータベースを選択します。

2.  右クリックして **[タスク]** を選択し、**[Stretch]** を選択し、**[有効化]** を選択してウィザードを起動します。

## <a name="Intro"></a>はじめに
ウィザードの目的と前提条件を確認します。

重要な前提条件を以下に示します。
-   データベースの設定を変更するため、管理者である必要があります。
-   Microsoft Azure サブスクリプションを持っている必要があります。
-   SQL Server がリモート Azure サーバーと通信できる必要があります。

![Stretch Database ウィザードの概要ページ][StretchWizardImage1]

## <a name="Tables"></a>テーブルを選択する
Stretch を有効にするテーブルを選択します。

行の多いテーブルは、並べ替えられた一覧の上部に表示されます。ウィザードでは、テーブルの一覧を表示する前に、Strech Database で現在サポートされていないデータ型が含まれていないかどうかを分析します。

![Stretch データベース ウィザードの [テーブルの選択] ページ][StretchWizardImage2]

|分割|説明|
|----------|---------------|
|(タイトルなし)|選択したテーブルの Stretch を有効にするにはこの列のチェック ボックスを選択します。|
|**名前**|テーブルの列の名前を指定します。|
|(タイトルなし)|この列の記号は、選択したテーブルの Stretch の有効化を妨げない警告を表している場合があります。また、テーブルでサポートされないデータ型を使用しているなどの理由により、選択したテーブルの Stretch の有効化を妨げるブロック問題を表している場合もあります。記号の上にマウス カーソルを合わせると、ヒント形式で詳細が表示されます。詳細については、「[Stretch Database の制限事項](sql-server-stretch-database-limitations.md)」を参照してください。|
|**ストレッチ済み**|テーブルの Stretch が既に有効になっているかどうかを示します。|
|**移行**|テーブル全体を移行することも (**[テーブル全体]**)、テーブル内の既存の列に対するフィルターを指定することもできます。移行する行を選択するフィルター述語を変更する場合は、ウィザードを終了した後、ALTER TABLE ステートメントを実行してフィルター述語を指定します。フィルター述語の詳細については、「[Select rows to migrate by using a filter predicate (移行する行の選択にフィルター述語を使用する)](sql-server-stretch-database-predicate-function.md)」を参照してください。述語の適用方法の詳細については、「[テーブルの Stretch Database を有効にする](sql-server-stretch-database-enable-table.md)」または「[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)」を参照してください。|
|**行**|テーブルの行数を指定します。|
|**サイズ (KB)**|テーブルのサイズを KB 単位で指定します。|

## <a name="Filter"></a>必要に応じて、行フィルターを指定する

フィルター述語を指定して移行する行を選択する場合は、**[テーブルの選択]** ページで次のようにします。

1.  **[Select the tables you want to stretch (ストレッチするテーブルの選択)]** ボックスの一覧で、テーブルの行の **[テーブル全体]** をクリックします。**[Select rows to stretch (ストレッチする行の選択)]** ダイアログ ボックスが表示されます。

    ![Define a filter predicate][StretchWizardImage2a]

2.  **[Select rows to stretch (ストレッチする行の選択)]** ダイアログ ボックスで、**[Choose Rows (行の選択)]** を選択します。

3.  **[名前]** フィールドに、フィルター述語の名前を入力します。

4.  **[Where]** 句で、テーブルの列を選択し、演算子を選択して、値を指定します。

5. **[確認]** をクリックして述語をテストします。述語がテーブルから結果を返す場合、つまり条件を満たす移行する行がある場合は、テストで **[成功]** と表示されます。

    >   [AZURE.NOTE] フィルター クエリを表示するテキスト ボックスは、読み取り専用です。テキスト ボックスのクエリを編集することはできません。

6.  [完了] をクリックして **[テーブルの選択]** ページに戻ります。

ウィザードを終了すると、フィルター関数が SQL Server に作成されます。終了するまでは、**[テーブルの選択]** ページに戻って、フィルター関数やその名前を変更することができます。

![フィルター述語を定義した後の [テーブルの選択] ページ][StretchWizardImage2b]

別の種類のフィルター述語を使用して、移行する行を選択する場合は、次のいずれかの操作を行います。

-   ウィザードを終了し、ALTER TABLE ステートメントを実行してテーブルの Stretch を有効にし、述語を指定します。詳細については、「[テーブルの Stretch Database を有効にする](sql-server-stretch-database-enable-table.md)」を参照してください。  

-   ウィザードを終了してから、ALTER TABLE ステートメントを実行して、述語を指定します。必要な手順については、「[Add a filter predicate after running the Wizard (ウィザードの実行後にフィルター述語を追加する)](sql-server-stretch-database-predicate-function.md#addafterwiz)」を参照してください。

## <a name="Configure"></a>Azure デプロイを構成する

1.  Microsoft アカウントで Microsoft Azure にサインインします。

    ![Azure へのサインイン - Stretch Database ウィザード][StretchWizardImage3]

2.  Stretch Database に使用する既存の Azure サブスクリプションを選択します。

3.  Azure リージョンを選択します。
    -   新しいサーバーを作成すると、このリージョンにサーバーが作成されます。  
    -   選択したリージョンに既存のサーバーが存在する場合は、**[Existing server (既存のサーバー)]** を選択すると、ウィザードに一覧表示されます。

    待機時間を最小限に抑えるために、SQL Server が配置されている Azure リージョンを選択してください。リージョンの詳細については、「[Azure のリージョン](https://azure.microsoft.com/regions/)」を参照してください。

4.  既存のサーバーを使用するか、新しい Azure サーバーを作成するように指定します。

    SQL Server の Active Directory が Azure Active Directory と連合している場合、SQL Server のフェデレーション サービス アカウントを利用し、リモート Azure サーバーと通信することもできます。このオプションの要件に関する詳細については、「[ALTER DATABASE の SET オプション (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)」を参照してください。

	-   **新しいサーバーを作成する**

        1.  サーバー管理者のログイン名とパスワードを作成します。

        2.  SQL Server のフェデレーション サービス アカウントを利用し、リモート Azure サーバーと通信することもできます。

		![新しい Azure サーバーの作成 - Stretch Database ウィザード][StretchWizardImage4]

    -   **既存サーバー**

        1.  既存の Azure サーバーを選択します。

        2.  認証方法を選択します。

            -   **[SQL Server 認証]** を選択した場合、管理者のログインとパスワードを指定します。

            -   SQL Server のフェデレーション サービス アカウントを利用し、リモート Azure サーバーと通信するには、**[Active Directory 統合認証]** を選択します。選択したサーバーが Azure Active Directory と統合されていない場合、このオプションは表示されません。

		![既存の Azure サーバーの選択 - Stretch Database ウィザード][StretchWizardImage5]

## <a name="Credentials"></a>資格情報をセキュリティで保護する
Stretch Database がリモート データベースの接続に使用する資格情報が守るためには、データベース マスター キーを用意する必要があります。

データベース マスター キーが既に存在する場合は、そのパスワードを入力します。

![Stretch Database ウィザードのセキュリティで保護された資格情報ページ][StretchWizardImage6b]

データベースに既存のマスター キーがない場合は、データベース マスター キーを作成するための強力なパスワードを入力します。

![Stretch Database ウィザードのセキュリティで保護された資格情報ページ][StretchWizardImage6]

データベース マスター キーの詳細については、「[CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx)」と「[データベース マスター キーの作成](https://msdn.microsoft.com/library/aa337551.aspx)」を参照してください。ウィザードにより作成される資格情報の詳細については、「[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx)」を参照してください。

## <a name="Network"></a>IP アドレスを選択する
サブネットの IP アドレス範囲 (推奨)、または SQL Server のパブリック IP アドレスを使用して、SQL Server とリモート Azure サーバーの通信を可能にするファイアウォール規則を Azure で作成します。

このページで指定した IP アドレスにより、SQL Server からの受信データ、クエリ、および管理操作が Azure ファイアウォールを通過することを許可するよう Azure サーバーに指示します。このウィザードによって、SQL Server のファイアウォール設定が変更されることはありません。

![Stretch Database ウィザードの IP アドレスの選択ページ][StretchWizardImage7]

## <a name="Summary"></a>概要
入力した値、ウィザードで選択したオプション、Azure の推定コストを確認します。**[完了]** を選択して、Stretch を有効にします。

![Stretch Database ウィザードのまとめのページ][StretchWizardImage8]

## <a name="Results"></a>結果
結果を確認します。

データ移行の状態を監視するには、「[データ移行の監視とトラブルシューティング (Stretch Database)](sql-server-stretch-database-monitor.md)」を参照してください。

![Results page of the Stretch Database wizard][StretchWizardImage9]

## <a name="KnownIssues"></a>ウィザードのトラブルシューティングを行う
**Stretch Database ウィザードが失敗しました。** Stretch Database がサーバー レベルで有効になっていないとき、システム管理者の許可なしでウィザードを実行し、有効にしようとすると、ウィザードは失敗します。ローカル サーバー インスタンスで Stretch Database を有効にするようにシステム管理者に依頼し、その後、ウィザードをもう一度実行します。詳細については、「[Prerequisite: Permission to enable Stretch Database on the server (前提条件: サーバーで Stretch Database を有効にするためのアクセス許可)](sql-server-stretch-database-enable-database.md#EnableTSQLServer)」を参照してください。

## 次のステップ
Stretch Database の追加テーブルを有効にします。データ移行を監視し、Stretch 対応のデータベースとテーブルを管理します。

-   [テーブルの Stretch Database を有効にして](sql-server-stretch-database-enable-table.md)追加テーブルを有効にします。

-   [データ移行の監視とトラブルシューティングを行って](sql-server-stretch-database-monitor.md)、データ移行の状態を確認します。

-   [Stretch Database を一時停止し、再開します。](sql-server-stretch-database-pause.md)

-   [Stretch Database を管理し、問題を解決します。](sql-server-stretch-database-manage.md)

-   [Stretch 対応データベースをバックアップする](sql-server-stretch-database-backup.md)

## 関連項目

[データベースの Stretch Database を有効にする](sql-server-stretch-database-enable-database.md)

[テーブルの Stretch Database を有効にする](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png

<!---HONumber=AcomDC_0615_2016-->