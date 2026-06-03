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

## 参考リンク

- [GitBook Pricing（公式）](https://www.gitbook.com/pricing)
- [Plans | GitBook Docs](https://gitbook.com/docs/account-management/plans)
- [Translations | GitBook Docs](https://gitbook.com/docs/gitbook-agent/translations)（料金: $25/50,000語、超過 $0.20/1,000語）
- [Localize your docs with variants](https://gitbook.com/docs/guides/content-organization-and-localization/localize-your-docs-with-variants-in-gitbook)
- [Use GitHub Actions to translate GitBook pages](https://gitbook.com/docs/guides/content-organization-and-localization/use-github-actions-to-translate-gitbook-pages)
- [Content variants | GitBook Docs](https://gitbook.com/docs/docs-site/site-structure/variants)
