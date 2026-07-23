# NFTT 取引制限リスト

NFTT Market の取引制限機能が参照するポリシーファイルの管理リポジトリです。
編集はCNP運営のみが行います。

| 環境 | ファイル | 状態 |
|---|---|---|
| 開発(beniten.net テスト環境) | `dev/blocked-tokens.json` | テスト用ID入り |
| 本番(cryptoninja.nftt.market) | `prod/blocked-tokens.json` | 空リスト(未運用) |

## 編集ルール

1. `blocked` のコレクションアドレスは**小文字**、Token IDは**文字列**で記載する
2. 編集のたびに `version` を **+1** し、`updatedAt` を更新する
3. 1箇所でも形式が不正な場合、サーバはファイル全体を拒否して直前の状態を維持する
   (安全設計。編集後は必ずJSONとして妥当か確認すること)
4. 反映はサーバ取得+CDNキャッシュ込みで**最大5〜6分**
5. コミットメッセージに「誰を・なぜ」を書く(監査記録を兼ねるため)

## 書式

```json
{
  "version": 2,
  "updatedAt": "2026-07-22T00:00:00Z",
  "chainId": 1,
  "blocked": {
    "0x138a5c693279b6cd82f48d4bef563251bc15adce": ["1234", "5678"]
  },
  "cooldown": {
    "default": { "enabled": false, "days": 7 },
    "collections": {
      "0x138a5c693279b6cd82f48d4bef563251bc15adce": { "enabled": true, "days": 7 }
    }
  }
}
```

- `blocked`: コレクションアドレス → 取引制限するToken IDの配列
- `cooldown`: 購入後の再出品禁止設定(コレクション単位でON/OFF・日数)
