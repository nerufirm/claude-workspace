# Expense & Budget Processing Skill (経費処理と予算更新スキル)

## Purpose
フォルダ内の領収書・請求書（画像/PDF）からデータを抽出し、予算スプレッドシート（CSV形式等）を更新、財務チーム向けの報告メールを自動生成する。

## Inputs
- `/expenses/new/` フォルダ内の新しい領収書・請求書ファイル
- `budget_2026.csv`（既存の予算管理ファイル）
- 会社の経費規程（もしあれば `/00_Context/expense-policy.md`）

## Process
1. **データ抽出**: `/expenses/new/` 内の全ファイルをスキャンし、以下の項目を抽出する。
   - 日付、支払先、金額、通貨、カテゴリー（食費、交通費、備品、SaaS等）
2. **検証 (Practice 8)**: 
   - 抽出したデータが不鮮明な場合や、カテゴリーが不明な場合は、勝手に推測せず「VERIFY（要確認）」フラグを立てる。
   - 会社の経費規程に違反する可能性があるものは「POLICY_ALERT」を付与する。
3. **予算反映**: `budget_2026.csv` の内容に基づき、各カテゴリーの残予算を計算し、今回の支出を追加した新しい行を作成する。
4. **アウトプット生成**: 更新された予算データと、財務報告用のメールドラフトを作成する。

## Output
1. **Update Log**: `YYYY-MM-DD_expense_log.md` (処理したファイルの一覧と抽出結果)
2. **Budget Update**: `budget_2026_updated.csv` (元のファイルを上書きせず、別名で保存)
3. **Email Draft**: `finance_report_draft.txt` (財務担当者宛ての報告用)

## Constraints (Practice 17)
- **NO DELETION**: 元の領収書ファイルを削除したり移動したりしない。
- **NO GUESSING**: 金額や日付が1%でも曖昧な場合は、必ずユーザーに確認を求める。
- **CURRENCY**: 通貨が混在している場合は、本日のレート（Web検索を使用）で基本通貨に換算し、換算レートを明記する。
