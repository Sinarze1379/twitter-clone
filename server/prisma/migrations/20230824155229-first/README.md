# Migration `20230824155229-first`

This migration has been generated by Sinarze1379 at 8/24/2023, 8:22:29 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
ALTER INDEX "public"."Profile_userId_key" RENAME TO "Profile.userId_unique"

ALTER INDEX "public"."User_email_key" RENAME TO "User.email_unique"
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20230824155229-first
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,72 @@
+generator client {
+  provider = "prisma-client-js"
+}
+
+datasource postgresql {
+  provider = "postgresql"
+  url = "***"
+}
+
+model Tweet {
+  id        Int          @id @default(autoincrement())
+  createdAt DateTime     @default(now())
+  content   String?
+  likes     LikedTweet[]
+  author    User?        @relation(fields: [authorId], references: [id])
+  authorId  Int?
+  comments  Comment[]
+}
+
+model User {
+  id         Int          @id @default(autoincrement())
+  email      String       @unique
+  password   String       @default("")
+  name       String?
+  tweets     Tweet[]
+  Profile    Profile?
+  likedTweet LikedTweet[]
+  comments   Comment[]
+  Following  Following[]
+}
+
+model LikedTweet {
+  id      Int      @id @default(autoincrement())
+  tweet   Tweet    @relation(fields: [tweetId], references: [id])
+  likedAt DateTime @default(now())
+  userId  Int?
+  User    User?    @relation(fields: [userId], references: [id])
+  tweetId Int
+}
+
+model Following {
+  id       Int    @id @default(autoincrement())
+  name     String
+  avatar   String
+  followId Int
+  User     User?  @relation(fields: [userId], references: [id])
+  userId   Int?
+}
+
+model Profile {
+  id        Int      @id @default(autoincrement())
+  createdAt DateTime @default(now())
+  bio       String?
+  location  String?
+  website   String?
+  avatar    String?
+  userId    Int?     @unique
+  User      User?    @relation(fields: [userId], references: [id])
+}
+
+model Comment {
+  id        Int       @id @default(autoincrement())
+  createdAt DateTime  @default(now())
+  content   String?
+  Tweet     Tweet?    @relation(fields: [tweetId], references: [id])
+  tweetId   Int?
+  User      User?     @relation(fields: [userId], references: [id])
+  userId    Int?
+  comments  Comment[] @relation("CommentToComment")
+  Comment   Comment?  @relation("CommentToComment", fields: [commentId], references: [id])
+  commentId Int?
+}
```

