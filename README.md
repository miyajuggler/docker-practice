# docker-practice

## Risk Report

https://whitesource.atlassian.net/wiki/spaces/WDJ/pages/41550144/Risk+Report

なんか色々細かく書いてあるページ。
organization や product とか範囲選択可能

### セキュリティ

組織レベルまたは製品レベルのビューで、ソフトウェアの健全性に関する高レベルのグラフィック解析を表示  
脆弱性スコア, 脆弱なコンポーネントの数, セキュリティの脆弱性の高齢化, 重大度の分布 など

### ライセンスのリスクとコンプライアンス

ライセンス配布の概要、使用されているライセンス、および各ライセンスに関連付けられているライブラリの数

### 品質

古くなったライブラリと習熟度のレベル

### さらなる詳細

- General Overview (総括レポート)

  製品毎に、ライセンス、セキュリティと品質

- Outdated Libraries (古いライブラリ)

  製品/プロジェクトごとの古いライブラリの詳細と古いライブラリごとの推奨アクション

- Use of Different Version of Same Library (同じライブラリの異なるバージョンの使用)

  コードベースに異なるバージョンが含まれているライブラリの表示。 これは、組織全体のマルチバージョンライブラリではなく、同じ製品内でのマルチバージョンの使用のみを示しています（つまり、2 つの別個の出荷可能製品の異なるバージョンのライブラリを使用することはマルチバージョンの使用とみなされません）。

- Policy Violations (ポリシー違反)

  プロジェクトごとの組織ポリシーまたは製品ポリシーに違反するライブラリ、違反の日付、推奨されるアクション。

- High Risk Licenses (高リスクライセンス)

  高リスクライセンスに関連するライブラリ、リスクの性質とスコア、推奨されるアクションの表示。

## UI-ライブラリマッチタイプ

https://whitesource.atlassian.net/wiki/spaces/WSK/pages/2357362810/UI+-+Library+Match+Types

Exact Match（完全一致）

Best (Effort) Match（ベスト（エフォート）一致）

Filename Match（ファイル名の一致）

Suspected Match（疑わしい一致）

## UI - Modified Date in Vulnerability-Based and Library-Based Alerting Mode

https://whitesource.atlassian.net/wiki/spaces/WSK/pages/2352447695/UI+-+Modified+Date+in+Vulnerability-Based+and+Library-Based+Alerting+Mode

## ignore alert report

https://whitesource.atlassian.net/wiki/spaces/WDJ/pages/706904081/Ignored+Alerts+Report

ここで ignore したアラートを復元したりもできる
一応 description に重大度の評価が書いてある。

- Blocker： ソフトウェアが正常に動作しないようにするバグ。
- Critical： 重大なバグには回避策や迅速な修正が含まれている可能性があります。
- Major： メインエリアの彼は、機能が動作しませんではなく、
- Minor： 問題はめったに使用されない領域です

フィルタリングも可能。

## ライブラリの詳細ページを理解する

https://whitesource.atlassian.net/wiki/spaces/WD/pages/33947818/Understanding+the+Library+Details+Page

### バージョンとトレンド

ライブラリの現在のバージョンと古いバージョンを比較

追加機能。影響分析 がきになる。

## ライブラリライセンスの管理

https://whitesource.atlassian.net/wiki/spaces/WD/pages/34111727/Managing+Library+Licenses

ライセンスのオーバーライドについてやり方とか書いてある。

### ライセンス情報の割り当て

ライセンス情報は、コンポーネントごとに識別および割り当てられます。この情報は、いくつかの種類のオープンソース情報リソースに基づくことができます。

1. 埋め込み
   - ライセンステキストファイル
   - コメントコメント
   - ヘッダ
2. マニフェスト
   - Readme ファイル
   - メタデータ（例：pom.xml、package.json）

### ライセンスの選択

WhiteSource が 2 つのライセンス割り当てを検出する場合がある。その時優先ライセンスを選択して

## licenses ダッシュボード

https://whitesource.atlassian.net/wiki/spaces/WDJ/pages/2460188852/Licenses

- Name：ライセンスの名前

- Occurrences（発生数）：組織内の発生数

- Copyright：著作権リスクの測定値である著作権リスクスコア。詳細については、「リスクスコアの帰属とライセンス分析について」の「ライセンスリスクスコア」を参照してください。

- Patent：特許およびロイヤルティのリスクスコアの測定。

| リスクスコア | 説明                                             | リスク |
| ------------ | ------------------------------------------------ | ------ |
| 20           | ロイヤリティフリーで、特定された特許リスクはなし | 低い   |
| 40           | 訴訟がない限り、ロイヤリティフリー               | 低い   |
| 60           | 特許は付与されていません                         | 中     |
| 80           | 特定された特定の特許リスク                       | 高い   |

**Royalty Free**：一部のライセンスは、明示的に特許ライセンスを付与します。ClearBSD など、明示的にしないものもあります。ユーザーが訴えられないことを条件にライセンスがあり、訴えられた場合はライセンスが取り消されます。
