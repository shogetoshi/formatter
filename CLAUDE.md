# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 概要

このリポジトリは様々なファイル形式のフォーマッターと便利ツールを提供するシェルスクリプト集です。メインのフォーマッターはファイル拡張子を自動判定してそれぞれのフォーマッターを実行します。

## 主要なツール

### フォーマッター系
- `format_python`: blackとisortを使用したPythonコードフォーマッター
- `format_sql`: sqlfluffを使用したSQLフォーマッター（soft/gentle/forcefulモード）
- `format_typescript`: prettierを使用したTypeScriptフォーマッター
- `formatter`: 拡張子に基づいてフォーマッターを自動選択する統合フォーマッター

### データ処理ツール
- `csv2md`: CSVをマークダウンテーブルに変換するPythonスクリプト
- `duckfilter`: DuckDBを使用してCSV/JSON/TSVデータをSQL処理し結果を各種形式で出力
- `jqable`: Python literalをJSONに変換する小さなユーティリティ

## 設定ファイル

- `types.conf`: ファイル拡張子とフォーマッターのマッピング定義
  - 現在の定義: py→format_python, ts→format_typescript

## 使用方法

### formatter（統合フォーマッター）
```bash
./formatter file1.py file2.ts   # 拡張子自動判定
./formatter -t py file1.txt     # タイプ明示指定
```

### 個別フォーマッター
```bash
cat file.py | ./format_python   # 標準入力から
./format_python file.py         # ファイル直接指定
```

### データ処理
```bash
cat data.csv | ./csv2md         # CSV→マークダウン変換
cat data.json | ./duckfilter "select * from JSON where column > 10"  # SQL処理
```

## 依存関係

- Python: black, isort, duckdb
- Node.js: prettier
- その他: sqlfluff

## アーキテクチャ

各フォーマッターは独立したシェルスクリプトまたはPythonスクリプトで、標準入力/出力を基本とするUnixフィルター方式で設計されています。`formatter`が統合エントリーポイントとして機能し、`types.conf`の設定に基づいて適切なフォーマッターを選択します。