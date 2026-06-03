# GitBook 運用ガイド

このリポジトリは、GitBook の Git Sync で各ドキュメントサイト（Platform / CMS / Visualizer / Flow、および help-center 多言語版）を管理しています。本 README は、運用判断のために **GitBook のプラン・できること/できないこと・翻訳の課金・その回避策と運用コスト** をまとめたものです。

> ⚠️ 料金・機能は変わりやすいため、**2026 年 6 月時点**の調査値です。発注前に必ず[公式 Pricing](https://www.gitbook.com/pricing) で最新を確認してください。

---

## 1. プラン体系（概要）

GitBook の課金は **「サイト単位の月額」＋「ユーザー（シート）単位の月額」** の合算です。さらに **サイトごとに別契約**（例：CMS と Visualizer を別サイトにすると 2 契約）になる点に注意。

| プラン | 料金（/サイト/月） | シート | 主な位置づけ |
| --- | --- | --- | --- |
| **Free** | $0 | 1 ユーザーのみ | 試用・個人。`gitbook.io` サブドメインのみ |
| **Premium** | $65 + $12/ユーザー | 追加課金 | 独自ドメイン・公開運用の最小ライン |
| **Ultimate** | $249 + $12/ユーザー | 追加課金 | AI アシスタント・認証アクセス・セクション/グループ |
| **Enterprise** | 個別見積（サポートだけで $1,000〜2,000+/月規模） | 個別 | SSO・移行支援・専任サポート |

> シートは「最初の 1 人無料」ではなく、**オーナー以外の共同編集者は全員 $12/月**が発生します。

---

## 2. できること / できないこと（プラン別）

### Free
- ✅ ビジュアルエディタ、カスタムブロック、**GitHub / GitLab Sync**、API Playground、プレビュー、SEO/LLM 最適化、ページビュー無制限
- ❌ **独自ドメイン不可**（`*.gitbook.io` のみ）
- ❌ AI ライティング、❌ アナリティクス、❌ PDF 出力、❌ 翻訳（後述・別課金）、❌ SSO
- ❌ 共同編集（実質 1 名）

### Premium（$65 + $12/user）
- ✅ **独自ドメイン**、リアルタイム共同編集、ブランディング、アナリティクス、フィードバック収集、リダイレクト、**PDF エクスポート**、限定共有リンク、マージルール
- ✅ **AI ライティング/編集**
- ❌ AI アシスタント（読者向けチャット）、❌ 認証アクセス、❌ クロスサイト検索、❌ セクション/グループ

### Ultimate（$249 + $12/user）
- ✅ Premium の全機能 ＋
- ✅ **GitBook AI アシスタント**（読者向け、200 回答/月）、**認証アクセス**（ログイン必須サイト）、**クロスサイト検索**、カスタムフォント、アダプティブコンテンツ無制限
- ✅ **セクション / グループ**（= 1 サイトを `/cms/` `/visualizer/` などのパスで束ねる構成に有用）
- 🧪 GitBook Agent（ベータ）

### Enterprise（個別）
- ✅ **SAML SSO / SCIM**、移行支援（white-glove）、カスタム連携、専任サポート、トレーニング、請求書払い・個別契約

> 本リポジトリで狙っている「`docs.eukarya.io/cms/` のようにパスで複数プロダクトを束ねる」構成は、**セクション/グループ（Ultimate 以上）**が前提になる可能性が高い点に留意。

---

## 3. 翻訳機能は「別課金のアドオン」

GitBook の **AI 翻訳（Translations）** は、プランとは**別の従量アドオン**です（プラン内に含まれない）。

| 項目 | 内容 |
| --- | --- |
| **料金** | **$25 / 月（翻訳 50,000 語まで）**、超過分は **$0.20 / 1,000 語** |
| **再課金** | 変更があったページのみ再翻訳・再課金（未変更分は翌月以降課金されない） |
| **対応言語** | ボタン操作で最大 36 言語に自動ローカライズ可能 |
| **自動更新** | 元言語を更新すると、変更ページだけ翻訳ワークフローが自動再実行 |
| **操作権限** | **組織管理者のみ**が作成・アクセス可能（billable feature のため） |
| **言語あたり** | 重複課金を避けるため「**1 言語につき翻訳ワークフローは 1 つ**」推奨 |

つまり「日本語版を AI 翻訳で自動生成・自動同期」したい場合、**最低 $25/月の追加コスト**（語数次第で増加）がかかります。

---

## 4. 翻訳課金の回避パターンと運用コスト

「GitBook の $25/月アドオンを使わずに多言語を運用する」現実的な選択肢は次の 3 つです。

### パターン A：手動（人手 or AI）翻訳 + Git Sync バリアント ← 本リポジトリの現状方式
- **やり方**: 英語 `help-center/` を別ディレクトリ `help-center-ja/` に複製し、Markdown を翻訳してコミット。GitBook 側で **言語バリアント**として登録。
- **GitBook 追加課金**: $0（翻訳アドオン不要）。ただしバリアント公開・言語スイッチャーは**有料プラン（Premium/Ultimate）側の機能**に依存。
- **運用コスト**:
  - 翻訳作業＝人件費 or 翻訳ツール費（社内/外注）
  - **元言語の更新時に手動で訳を追従**させる手間（差分管理が属人化しやすい）
- **向き**: 更新頻度が低い／訳の品質を人手で担保したいドキュメント。

### パターン B：GitHub Actions + 外部翻訳 API（自動化）
- **やり方**: Git Sync で GitBook → リポジトリへコミット → **GitHub Actions** が変更ページを検知し、**DeepL / OpenAI / Google Cloud Translation** で翻訳 → 翻訳先ディレクトリにコミット → Git Sync で別言語 Space に反映。
- **GitBook 追加課金**: $0(翻訳アドオン不要)。
- **運用コスト**:
  - **翻訳 API 従量課金**（例：DeepL は無料枠あり/超過で従量、OpenAI はトークン課金）
  - **CI/ワークフローの構築・保守**（エンジニア工数）
  - 「変更ページのみ翻訳」する仕組みを入れないと API コストが膨らむ
  - **自動翻訳が人手修正を上書きしない**ためのレビュー/制御フローが必要
- **向き**: 更新が多く、エンジニアリソースがあるチーム。GitBook アドオンより安く回せる可能性が高いが、初期構築と保守が前提。

### パターン C：外部 TMS（Crowdin / Lokalise 等）連携
- **やり方**: Git 連携できる翻訳管理サービスに Markdown を渡し、翻訳メモリ/用語集で管理。翻訳済みを別ディレクトリに戻して Git Sync。
- **運用コスト**: TMS のサブスク費用 ＋ 連携設定。翻訳資産（TM/用語集）の蓄積で長期的に品質・コスト効率が上がる。
- **向き**: 多言語・大規模・継続運用。

### コスト比較（ざっくり）

| 方式 | GitBook 翻訳課金 | 追加で発生する主コスト | 自動追従 | 品質管理 |
| --- | --- | --- | --- | --- |
| GitBook AI 翻訳（純正） | **$25/月〜**（語数従量） | なし | ◎ 自動 | AI 任せ（用語集で補正可） |
| A: 手動 + バリアント | $0 | 翻訳人件費/工数 | ✕ 手動 | ◎ 人手 |
| B: Actions + 翻訳API | $0 | API 従量 + CI 保守工数 | ◯ 半自動 | △ レビュー要 |
| C: 外部 TMS | $0 | TMS サブスク | ◯ | ◎ TM/用語集 |

> 目安：**少量・低頻度なら A**、**頻繁な更新で内製できるなら B**、**本格的な多言語運用なら C or 純正アドオン**。純正 $25/月は「設定の楽さ」を金で買う形で、語数が少なくエンジニア工数を避けたい場合に最もコスパが良いことも多い。

---

## 5. このリポジトリの構成

```
help-center/      英語版ヘルプセンター（原文）
help-center-ja/   日本語版（パターン A：手動翻訳 + Git Sync バリアント）
visualizer/       Visualizer ドキュメント
```

各ディレクトリを GitBook の別 Space として Git Sync しています。詳細な Git Sync 手順は各 Space の設定（Settings → Git Sync）を参照。

---

## 6. LLM 向けテキスト（llms.txt / llms-full.txt）

GitBook で公開したサイトは、LLM / AI エージェントから参照しやすいプレーンテキストを **自動生成・公開**します（Free プランでも利用可能な LLM 最適化機能）。サイト URL の末尾に以下を付けるだけで取得できます。

| ファイル | 内容 | 用途 |
| --- | --- | --- |
| `/llms.txt` | ページ構成・タイトル・各ページへのリンクを並べた **目次/ナビゲーション** | LLM に「どのページがどこにあるか」を渡す。軽量 |
| `/llms-full.txt` | 全ページの本文を連結した **完全版（verbatim）** | RAG・インデックス・全文検索など、本文そのものが必要な用途 |

### 本サイトでの実例

- 目次版: <https://yuya-soneda.gitbook.io/reearth/llms.txt>
- 全文版: <https://yuya-soneda.gitbook.io/reearth/llms-full.txt>

```bash
# 目次（リンク集）を取得
curl -s https://yuya-soneda.gitbook.io/reearth/llms.txt

# 全文を取得して LLM に渡す等
curl -s https://yuya-soneda.gitbook.io/reearth/llms-full.txt
```

> - `llms.txt` は Help Center / Documentation / API Reference / Changelog などのセクションを横断したリンク集、`llms-full.txt` は各記事の本文・手順・テーブル・OpenAPI スキーマまで含む完全テキストです。
> - 多言語バリアントを公開している場合は、言語ごとにそれぞれの URL で取得できます。
> - 公開サイトに対して生成されるため、**非公開（認証必須）サイトの扱いはプラン設定に依存**します。

---

## 7. MCP サーバー

GitBook には MCP（Model Context Protocol）の系統が **2 つ** あります。用途が違うので混同に注意。

### (1) サイト MCP サーバー（公式・自動生成）— 公開ドキュメントを読ませる

公開サイトごとに **MCP サーバが自動生成**され、Claude / Cursor などの AI クライアントから**ドキュメントを読み取り専用で参照**できます。サイト URL のルートに `/~gitbook/mcp` を付けるだけです。

| 項目 | 内容 |
| --- | --- |
| **エンドポイント** | `<サイトURL>/~gitbook/mcp` |
| **本サイトの例** | `https://yuya-soneda.gitbook.io/reearth/~gitbook/mcp`（稼働確認済み） |
| **アクセス** | 読み取り専用（検索・ページ取得）。非表示ページも MCP からは参照可能 |
| **前提条件** | **Site customization → Page actions が有効**であること（無効化すると 404） |
| **プロトコル** | JSON-RPC over HTTP（POST）。GET は 405 が返る = 正常 |

クライアント設定例（Claude Desktop / Cursor などの MCP 設定）:

```json
{
  "mcpServers": {
    "reearth-docs": {
      "url": "https://yuya-soneda.gitbook.io/reearth/~gitbook/mcp"
    }
  }
}
```

> `llms.txt` / `llms-full.txt`（セクション 6）が「テキストを丸ごと渡す」のに対し、サイト MCP サーバは「AI が必要なページを**検索して取りに行く**」用途。大規模ドキュメントでは MCP の方がトークン効率が良い。

### (2) API 用 MCP サーバー（コミュニティ）— GitBook を操作する

GitBook **API をラップしたコミュニティ製 MCP**（例：[gitbook-mcp](https://github.com/lucasbenevinuto/gitbook-mcp)）を使うと、AI エージェントから **Space / ページ / 変更リクエスト / Git Sync の操作**（書き込み系含む）が可能。こちらは公開サイトの参照ではなく、**コンテンツ管理の自動化**向けで、別途 GitBook API トークンが必要です。

| | (1) サイト MCP（公式） | (2) API MCP（コミュニティ） |
| --- | --- | --- |
| 目的 | 公開ドキュメントを **読む** | GitBook を **操作する** |
| 認証 | 不要（公開サイト） | GitBook API トークン |
| 範囲 | 読み取り専用 | 読み書き（管理操作） |
| 用途 | AI への docs 提供 | コンテンツ管理の自動化 |

---

## 8. AI アシスタントを ChatGPT / Claude 等で代替する

GitBook 純正の AI アシスタント（Ultimate 機能）が無くても、**用途によっては ChatGPT / Claude などで代替可能**です。GitBook が出力する LLM 向け入口（セクション 6・7）を使うのがコツで、**追加コスト $0** で運用できます。

### 渡し方別の効き具合

| 方法 | 動くか | 品質 | 備考 |
| --- | --- | --- | --- |
| 普通のページ URL を貼る | ブラウジング有効時のみ | △ | Web 取得設定が前提。取れるのは基本そのページ 1 枚 |
| `llms.txt`（目次）を貼る | ◯ | ◯ | どのページに何があるかを渡せる |
| `llms-full.txt`（全文）を貼る | ◎ | ◎ | **全ドキュメント本文を一括で渡せる**。最も確実 |
| **MCP サーバ**を接続 | ◎ | ◎ | AI が必要ページを検索して取得。純正アシスタントに最も近い |

→ おすすめは **「`llms-full.txt` を貼る」or「MCP を接続する」**。単なる URL 貼り付けより安定します。

```
# ChatGPT / Claude に貼るだけ（全文版）
https://yuya-soneda.gitbook.io/reearth/llms-full.txt
```

### 純正アシスタントとの違い（代替しきれない部分）

| | ChatGPT 等に渡す | GitBook 純正アシスタント |
| --- | --- | --- |
| 対象 | **自分・社内**（手元の AI 上） | **サイト訪問者**（docs 内に埋め込み） |
| 設置 | 不要（各自が貼る/接続） | サイトに組み込み済み |
| 鮮度 | 貼った時点のテキスト | 常に最新（公開内容に追従） |
| 根拠/引用 | モデル任せ（ハルシネーションあり） | 自社 docs に grounding・ページ引用 |
| スコープ | 渡した範囲のみ | docs 全体に限定 |

- **「自分・社内が docs を AI に質問する」用途** → 純正アシスタント不要。`llms-full.txt` 貼り付け or MCP 接続で代替可（$0）。
- **「訪問者向けに docs ページ内でチャットさせたい」用途** → 埋め込みウィジェットなのでリンクでは代替不可（純正＝Ultimate、または自前でチャット UI を実装）。

### 注意点
- ChatGPT は**ブラウジング/検索が無効だとリンクを開けない** → その場合はテキスト貼り付けが確実。
- ページ URL 1 本だと**そのページしか読まない**ことが多い → 全体 Q&A は `llms-full.txt` か MCP を使う。

---

## 9. 他サービスへの移行・並行運用（GitBook をやめる場合）

結論：**コンテンツの本体は Git 上の Markdown なので、移行は十分可能**。ただし GitBook 固有記法とホスティング機能は載せ替えが必要です。ロックインは「中（コンテンツは低・機能は高）」と考えてください。

### 移行しやすい部分（ロックイン低）
- **本文は標準 Markdown** で Git Sync 済み。リポジトリをそのまま別の静的サイトジェネレータ（SSG）や独自実装に渡せる。
- Git が常に正本（source of truth）なので、**GitBook を解約してもコンテンツは手元に残る**。
- → **並行運用が可能**：同じリポジトリを「GitBook（Git Sync）」と「独自サイト（CI でビルド）」の両方に供給し、移行期間中は二重公開できる。

### 載せ替えが必要な部分（ロックイン高）

**(a) GitBook 固有記法** … 標準 Markdown ではないため、移行先の記法へ変換が要る。本リポジトリでの使用状況：

| 記法 | 使用ファイル数 | 移行先での代替 |
| --- | --- | --- |
| `{% hint %}`（注記） | 34 | Docusaurus/Starlight の admonition、MkDocs の `!!! note` 等 |
| `{% stepper %}`/`{% step %}` | 15 | Steps コンポーネント or 番号付きリスト |
| `data-view="cards"`（カード） | 7 | Card コンポーネント or HTML/グリッド |
| `{% content-ref %}`（相互参照） | 5 | 通常のリンクに変換 |
| `{% tabs %}` | 4 | Tabs コンポーネント |
| `{% columns %}` | 3 | グリッド/段組みレイアウト |
| frontmatter `icon` / `layout` | 40 | 移行先のスキーマに合わせて変換/削除 |

→ `{% ... %}` を移行先記法へ置換する**変換スクリプト**を一度書けば、50 ファイル規模なら機械的に処理できる（手作業は表/カードの一部のみ）。

**(b) GitBook がホストしている機能** … これらは自前で再構築が必要：
- レンダリング/ホスティング、**全文検索**、**AI アシスタント**、**MCP サーバ自動生成**（セクション 7）、**`llms.txt` 自動生成**（セクション 6）
- アナリティクス、アクセス制御（認証）、PDF 出力、**多言語バリアント/翻訳**
- → 独自開発なら、検索（Algolia/Pagefind 等）・llms.txt 生成・MCP は個別に実装する

### 移行先の選択肢

| 移行先 | 特徴 | GitBook 記法の変換 |
| --- | --- | --- |
| **Docusaurus** | React/MDX。プラグイン豊富 | admonition/tabs は近い記法あり |
| **Mintlify** | docs 特化・デザイン良。商用 | 独自 MDX コンポーネントへ |
| **Astro Starlight** | 軽量・高速・OSS | admonition/カード対応 |
| **MkDocs (Material)** | Python・シンプル | `!!!` admonition 等へ |
| **独自開発（Next.js + MDX 等）** | 自由度最大・lock-in なし | 変換層もカスタムコンポーネントも自前 |

### 推奨アプローチ
1. **リポジトリを正本に固定**（GitBook は「Git → GitBook」一方向同期に寄せる）。
2. `{% %}` → 移行先記法の**変換スクリプト**を用意。
3. 移行先を **CI で並行ビルド**し、しばらく GitBook と二重公開して検証。
4. 検索・llms.txt・MCP など**ホスト機能の代替**を実装。
5. 問題なければ GitBook の独自ドメインを移行先へ切り替え、解約。

> 要点：**コンテンツ移行は低リスク（Markdown が手元にある）**。コストと工数は主に「固有記法の変換」と「ホスティング機能の再実装」に集中します。

---

## 10. 推奨プラン / 構成判断（本件の結論）

### 要件
`docs.eukarya.io/` をルート（Platform）に、`/cms/` `/visualizer/` `/flow/` を**配下パスで束ねた 1 サイト**として、上部ナビで横断できる構成にしたい。

### 結論：**Ultimate が必要**
- パスで束ねるには **セクション / グループ機能**が必須で、これは **Ultimate（$249/サイト/月〜）以上**の機能（セクション 2 参照）。
- **セクション機能 = 1 サイトに複数 Space をぶら下げる**仕組み。課金は「サイト単位」なので、**Ultimate $249/月は 1 契約で 4 プロダクト全体をカバー**（×4 にはならない）。＋ シート $12/ユーザー/月。

### セクション / グループとは
- **セクション**：別々の Space を 1 サイトの別パートとして束ねる。各セクションに slug（`/cms` 等）が付き、上部ナビに**タブ**表示。デフォルトセクションのみルート `/`。
- **グループ**：複数セクションを 1 つの**ドロップダウン**にまとめ、ナビのタブ乱立を防ぐ（各項目に説明文も付与可）。
- **バリアント（別概念）**：同じ内容の別バージョン（多言語・版違い）。CMS/Visualizer/Flow を分けるのがセクション、その日英を切り替えるのがバリアント。

### プラン別の選択肢

| 案 | 構成 | URL | 月額イメージ | 要件充足 |
| --- | --- | --- | --- | --- |
| **A: Ultimate + セクション** | 1 サイトに 4 Space | `docs.eukarya.io/` `…/cms/` 等 | $249 + シート | ✅ 要件どおり |
| B: Premium ×4 サイト | 各プロダクト独立サイト | `cms.eukarya.io` 等のサブドメイン | $65×4 ≒ $260 + シート | ❌ パス束ね不可・ナビ非統合 |
| C: 段階導入 | 主要1つ有料・残り Free 等 | 混在 | 変動 | ❌ URL/体験がバラつく |

> **「パスで束ねた 1 サイト」という要件を満たすのは A（Ultimate）のみ。** B は同程度の金額でもサブドメイン分割になり要件を満たさない。コスト最適化より要件（URL 構造・ナビ統合）を優先するなら **A 一択**。

---

## 参考リンク

- [GitBook Pricing（公式）](https://www.gitbook.com/pricing)
- [Plans | GitBook Docs](https://gitbook.com/docs/account-management/plans)
- [Translations | GitBook Docs](https://gitbook.com/docs/gitbook-agent/translations)（料金: $25/50,000語、超過 $0.20/1,000語）
- [Localize your docs with variants](https://gitbook.com/docs/guides/content-organization-and-localization/localize-your-docs-with-variants-in-gitbook)
- [Use GitHub Actions to translate GitBook pages](https://gitbook.com/docs/guides/content-organization-and-localization/use-github-actions-to-translate-gitbook-pages)
- [Content variants | GitBook Docs](https://gitbook.com/docs/docs-site/site-structure/variants)
- [MCP servers for published docs | GitBook Docs](https://gitbook.com/docs/ai-and-search/mcp-servers-for-published-docs)（エンドポイント: `<サイトURL>/~gitbook/mcp`）
- [gitbook-mcp（API 操作用コミュニティ MCP）](https://github.com/lucasbenevinuto/gitbook-mcp)
