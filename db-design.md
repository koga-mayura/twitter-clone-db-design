# TwitterクローンDB設計書（Firestore + Storage）

##--- usersコレクション

| フィールド名 | 型 | 用途 |
| `userId` | string | ユーザーID（Authのuidと同じ） |
| `displayName` | string | 名前 |
| `handleName` | string | ユーザー名（一意のもの） |
| `bio` | string? | 自己紹介 |
| `photoUrl` | string? | プロフィール画像URL |
| `headPhotoUrl` | string? | ヘッダー画像URL |
| `createdAt` | timestamp | アカウント作成日時 |
| `updatedAt` | timestamp | アカウント更新日時 |

--- users/{userId}/bookmarksサブコレクション

| フィールド名 | 型 | 用途 |
| `bookmarkId` | string | ブックマークのID |
| `tweetId` | string | ブックマークしたツイートのID |
| `createdAt` | timestamp | ブックマークした日時 |
| `updatedAt` | timestamp | 更新日時 |

--- users/{userId}/notificationsサブコレクション

| フィールド名 | 型 | 用途 |
| `notificationId` | string | 通知ID |
| `fromUserId` | string | 通知を送るユーザーのuserId |
| `type` |　string　|　通知の種類（いいね、リプライ、リツイート、引用リツイート、フォロー）|
| `tweetId` | string? | 対象のツイートID（follow通知はnull） |
| `isRead` | boolean | 通知の既読 |
| `createdAt` | timestamp | 通知発生日時 |
| `updatedAt` | timestamp | 更新日時 |
                    

##--- tweetsコレクション

| フィールド名 | 型 | 用途 |
| `tweetId` | string | ツイートID |
| `authorId` | string | 投稿者のuserId |
| `type` | string | 種類（twwet/retweet/引用retweet/reply） |
| `content` | string? | ツイート本文（通常リツイートのみnull） |
| `imageUrls` | array? | 添付画像URLの一覧（最大４枚） |
| `replyTo` | string? | 返信先のtweetId（リプライ時以外はnull） | 
| `retweetOf` | string? | 元ツイートのtweetId（通常ツイート、リプライ時はnull） |
| `status` | string | ツイートの状態（published / draft / deleted） |
| `createdAt` | timestamp | 投稿日時 |
| `updatedAt` | timestamp | 更新日時 |

--- tweets/{tweetId}/likesサブコレクション

| フィールド名 | 型 | 用途 |
| `likeId` | string | いいねのID |
| `likedBy` | string | いいねしたユーザーのuserId |
| `createdAt` | timestamp | いいねした日時 |
| `updatedAt` | timestamp | 更新日時 |

                                            
##--- follows/コレクション

| フィールド名 | 型 | 用途 |
| `followId` | string | フォローId |
| `followingId` | string | フォローした人のuserId |
| `followedId` | string | フォローされた人のuserId |
| `createdAt` | timestamp | フォローした日時 |
| `updatedAt` | timestamp | 更新日時 |

##--- blocksコレクション

| フィールド名 | 型 | 用途 |
| `blockId` | string | ブロックID |
| `blockerId` | string | ブロックしたユーザーのuserId |
| `blockedId` | string | ブロックされたユーザーのuserId |
| `createdAt` | timestamp | ブロックした日時 |
| `updatedAt` | timestamp | 更新日時 |



##---Firebase Storage

| パス | 用途 | 参照元 |
| `users/{userId}/photo` | プロフィール画像 | `users.photoUrl` |
| `users/{userId}/headPhoto` | ヘッダー画像 | `users.headPhotoUrl` |
| `tweets/{tweetId}/images` | ツイート画像 | `tweets.imageUrls` |



