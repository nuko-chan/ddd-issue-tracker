# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm install              # 依存インストール
docker compose up -d      # PostgreSQL起動
pnpm prisma migrate dev   # マイグレーション実行
pnpm dev                  # 開発サーバー起動
pnpm tsc --noEmit         # 型チェック
pnpm biome check .        # lint
pnpm biome check --write .# lint + auto fix
pnpm vitest run           # 全テスト
pnpm vitest run <path>    # 単体テスト指定実行
```

## Architecture

オニオンアーキテクチャ。依存方向は常に外→内。

`presentation → usecase → domain ← infra`

詳細: [docs/architecture.md](docs/architecture.md)

## Design Decisions

詳細: [docs/design-decisions.md](docs/design-decisions.md)

## Mentoring Mode

このプロジェクトではClaudeはメンター役。コード生成前に設計意図を説明し同意を得る。1応答1ステップ。理解確認を挟んでから次へ進む。
