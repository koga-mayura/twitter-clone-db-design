# TwitterクローンDB設計書（Firestore + Storage）

--- usersコレクション

| フィールド名 | 型 | 用途 |
| `userId` | string | ユーザーID（Authのuidと同じ） |
| `displayName` | string | 名前 |
| `handleName` | string | ユーザー名（一意のもの） |
| `bio` | string? | 自己紹介 |
| `photoUrl` | string? | プロフィール画像URL |
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
| `toUserId` | string | 通知が届くユーザーのuserId |
| `type` |　string　|　通知の種類（いいね、リプライ、リツイート、引用リツイート、フォロー）|
| `tweetId` | string? | 対象のツイートID（follow通知はnull） |
| `isRead` | boolean | 通知の既読 |
| `createdAt` | timestamp | 通知発生日時 |
| `updatedAt` | timestamp | 更新日時 |
                                            
--- follows/{followId}/コレクション

| フィールド名 | 型 | 用途 |
| `followingId` | string | フォローした人のuserId |
| `followedId` | string | フォローされた人のuserId |
| `createdAt` | timestamp | フォローした日時 |
| `updatedAt` | timestamp | 更新日時 |

--- blocks/{blockId}/blocksコレクション

| フィールド名 | 型 | 用途 |
| `blockId` | string | ブロックID |
| `blockerUserId` | string | ブロックしたユーザーのuserId |
| `blockedUserId` | string | ブロックされたユーザーのuserId |
| `createdAt` | timestamp | ブロックした日時 |
| `updatedAt` | timestamp | 更新日時 |

--- tweetsコレクション

| フィールド名 | 型 | 用途 |
| `tweetId` | string | ツイートID |
| `authorId` | string | 投稿者のuserId |
| `type` | string | 種類（twwet/retweet/引用retweet/reply） |
| `content` | string? | ツイート本文（通常リツイートのみnull） |
| `imageUrls` | array? | 添付画像URLの一覧（最大４枚） |
| `retweetOf` | string? | 元ツイートのtweetId（通常ツイート、リプライ時はnull） | 
| `replyTo` | string? | 返信先のtweetId（リプライ時以外はnull） | 
| `createdAt` | timestamp | 投稿日時 |
| `updatedAt` | timestamp | 更新日時 |

--- tweets/{tweetId}/likesサブコレクション

| フィールド名 | 型 | 用途 |
| `likeId` | string | いいねのID |
| `likedBy` | string | いいねしたユーザーのuserId |
| `createdAt` | timestamp | いいねした日時 |
| `updatedAt` | timestamp | 更新日時 |


---Firebase Storage

| パス | 用途 | 参照元 |
| `users/{userId}/photo` | プロフィール画像 | `users.photoUrl` |
| `tweets/{tweetId}/images` | ツイート画像 | `tweets.imageUrls` |


##参照

  | フィールド | 参照先 | 役割 |                                               
  
  | `tweets.authorId` | `users.userId` | 投稿者の特定 |                     
  | `tweets.retweetOf` | `tweets.tweetId` | 元ツイートの特定 |     
  | `tweets.replyTo` | `tweets.tweetId` | 返信先ツイートの特定 |     
  | `tweets/{tweetId}/likes.likedBy` | `users.userId` |　いいねしたユーザーの特定 | 
  | `users/{userId}/following.followedUserId` | `users.userId` |　フォロー先ユーザーの特定 |                                      
  | `users/{userId}/bookmarks.tweetId` | `tweets.tweetId` |　ブックマーク対象ツイートの特定 |                                  
  | `users/{userId}/blocks.blockedUserId` | `users.userId` |　ブロック対象ユーザーの特定 |                            
  | `users/{userId}/subscriptions.subscribedUserId` | `users.userId` |　通知登録対象ユーザーの特定 |    
  | `users/{userId}/notifications.fromUserId` | `users.userId` |　通知送信者の特定 |
  | `users/{userId}/notifications.tweetId` | `tweets.tweetId` |　通知対象ツイートの特定 |                                              
  | `users.photoUrl` | Firebase Storage | プロフィール画像の参照 |
  | `tweets.imageUrls` | Firebase Storage | ツイート画像の参照 |                   
  
