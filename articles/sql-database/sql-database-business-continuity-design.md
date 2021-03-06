<properties 
   pageTitle="ビジネス継続性のための SQL Database の設計" 
   description="選択するためのガイダンス。このセクションでは、どの BCDR 機能をいつ使用するかを選択する方法についてのガイダンスが提供されます。これには、SQL DB を使用して自動的に取得できることに関する説明が含まれます。"
   services="sql-database" 
   documentationCenter="" 
   authors="carlrabeler" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="05/27/2016"
   ms.author="carlrab"/>

# ビジネス継続性のための設計

ビジネス継続性のためにアプリケーションを設計するには、次の質問に答える必要があります。

1. アプリケーションを障害から保護するために適切なビジネス継続性の機能はどれでしょうか。
2. どのレベルの冗長性とレプリケーション トポロジを使用するでしょうか。

エラスティック プールを使用した場合の復旧戦略の詳細については、「[SQL Database エラスティック プールを使用した、アプリケーションの障害復旧戦略](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)」をご覧ください。

## geo リストアを使用する場合

[geo リストア](sql-database-geo-restore.md)は、データベースがホストされているリージョンでのインシデントのためにデータベースが利用できない場合にも既定の復旧オプションを提供します。SQL Database では、既定ですべてのデータベースにおける基本的な保護が組み込まれています。この保護は、[データベースのバックアップ](sql-database-automated-backups.md)を実行し、それを geo 冗長 Azure Storage (GRS) に保存することで実現します。この方法を選択する場合、特別に構成したり、別のリソースを割り当てる必要はありません。これらのバックアップを使用すると、あらゆるリージョンでgeo リストアのコマンドを使用してデータベースを回復できます。geo リストアを使用したアプリケーションの回復についての詳細は、「[障害からの回復](sql-database-disaster-recovery.md)」セクションをご覧ください。

アプリケーションが次の条件を満たす場合は組み込まれている保護を使用する必要があります。

1. ミッション クリティカルとして考慮されない場合。拘束力のある SLA はないため、24 時間以上のダウンタイムで財務責任が課せられることはありません。
2. データ変更レートが低い場合 (1 時間あたりのトランザクションなど)。RPO が 1 時間でも大規模なデータの損失は発生しません。
3. アプリケーションは費用重視なので、geo レプリケーションの追加コストは正当化できません。 

> [AZURE.NOTE] geo リストアでは、特定のリージョンにコンピューティング容量を事前に割り当て、システム停止時にバックアップからアクティブなデータベースを復元することはありません。サービスでは、そのリージョンの既存のデータベースへの影響を最小限に抑え、容量の要求を最優先する形でgeo リストアのリクエストに関連するワークロードを管理します。そのため、データベースの復旧時間は、同じリージョンで同時に回復するその他のデータベースの量によって異なります。

## アクティブ geo レプリケーションを使用する場合

[アクティブ geo レプリケーション](sql-database-geo-replication-overview.md)を使用すると、プライマリとは異なるリージョンに読み取り可能な (セカンダリ) データベースを作成できます。これにより、データベースには必要なデータとコンピューティング リソースを持たせ、回復後のアプリケーションのワークロードをサポートすることが保証されます。フェールオーバーを使用して、アプリケーションを回復させるには、「[障害からの回復](sql-database-disaster-recovery.md)」セクションをご覧ください。

アプリケーションが次の条件を満たす場合は geo レプリケーションを使用する必要があります。

1. ミッション クリティカルな場合。アグレッシブな RPO と RTO で、拘束力のある SLA がある場合。データや可用性の損失により財務責任が発生します。 
2. データ変更レートが高い場合 (1 分または 1 秒あたりのトランザクションなど)。既存の保護に関連した RPO が 1 時間の場合は、許容できないデータ損失が発生する可能性が高くなります。
3. geo レプリケーションの使用に関連するコストは、潜在的な財務責任とビジネスの関連する損失を大幅に下回る場合。

アクティブ geo レプリケーションを有効にする場合は、[Azure SQL Database の geo レプリケーションの構成](sql-database-geo-replication-portal.md)に関するページをご覧ください。

> [AZURE.NOTE] アクティブ geo レプリケーションでは、セカンダリ データベースへの読み取り専用アクセスもサポートするため、読み取り専用ワークロードに対して追加の容量を提供します。

##フェールオーバーの構成を選択する方法 

ビジネス継続性向けのアプリケーションを設計する際は、いくつかの構成オプションを検討する必要があります。アプリケーションのデプロイメント トポロジと、停止時に最も脆弱になるアプリケーションの部分によって選択肢が異なります。詳細は「[Geo レプリケーションを使用してビジネス継続性を実現するクラウド アプリケーションの設計](sql-database-designing-cloud-solutions-for-disaster-recovery.md)」をご覧ください。

## 次のステップ

- [geo レプリケーションを使用して障害復旧を実現するクラウド ソリューションの設計](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

## その他のリソース

- [SQL Database エラスティック プールを使用した、アプリケーションの障害復旧戦略](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) 

<!---HONumber=AcomDC_0615_2016-->