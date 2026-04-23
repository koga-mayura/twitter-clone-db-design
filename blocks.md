# blocks コレクション

## 概要

ユーザー間のブロック関係を管理するトップレベルコレクション。

---

## ドキュメント構造

**パス:** `/blocks/{blockId}`

| フィールド名 | 型 | 用途 |
|---|---|---|
| `blockId` (PK) | string | ドキュメントID（自動生成） |
| `blockerId` | string | ブロックを実行したユーザーのUID |
| `blockedId` | string | ブロックされたユーザーのUID |
| `createdAt` | timestamp | ブロックした日時 |

---

## ドキュメントID（blockId）

Firestoreの自動生成ID。`blockerId` と `blockedId` の組み合わせでクエリして取得する。

---

## セキュリティルール

```javascript
match /blocks/{blockId} {
  allow get: if request.auth.uid == resource.data.blockerId
             || request.auth.uid == resource.data.blockedId;
  allow list: if request.auth.uid == resource.data.blockerId;
  allow create: if request.auth.uid == request.resource.data.blockerId;
  allow delete: if request.auth.uid == resource.data.blockerId;
}
```

- `read`: ブロックした側・された側の両方が取得可能
- `create`: `blockerId` が自分自身のドキュメントのみ作成可能
- `delete`: `blockerId` が自分自身のドキュメントのみ削除可能

---

## 副作用

`block()` 実行時にフォロー関係を両方向削除する（バッチ処理）。

```
/follows で followingId == blockerId && followedId == blockedId → 削除
/follows で followingId == blockedId && followedId == blockerId → 削除
```

---

## 関連サービス

`lib/services/block_service.dart`
