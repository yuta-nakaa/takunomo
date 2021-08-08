# アプリ名
  たくのも

  ## アプリの概要

  ・おつまみのレシピを閲覧、投稿するアプリです。
  ・ユーザー登録機能
  ・ログイン機能
  ・おつまみレシピの投稿機能
  ・各レシピにいいね機能
  ・検索機能

  ## DB関連スクリプト

  データベース作成

  ```sql
  CREATE DATABASE takunomo_db;
  CREATE USER takunomo_user IDENTIFIED BY '1234';
  GRANT ALL ON takunomo_db.* TO takunomo_user;

  ```

  テーブル作成(6)

  1.users(ユーザー情報を保存するテーブル)
  2.recipes(レシピを保存するテーブル)
  3.liquors(各レシピに入れる合うお酒の種類のテーブル)
  4.favorites(お気入り機能のテーブル)
  5.reviews(レビューを保存するテーブル)
  6.categories(各お酒のカテゴリーのテーブル)

```sql

CREATE TABLE users (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(50) NOT NULL,
  email VARCHAR(255) NOT NULL UNIQUE KEY,
  password VARCHAR(255) NOT NULL,
  birthday DATE NOT NULL,
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE TABLE recipes (
  id INT PRIMARY KEY AUTO_INCREMENT,
  user_id INT NOT NULL, 
  name VARCHAR(30) NOT NULL,
  ingredients TEXT NOT NULL,
  recipe TEXT NOT NULL,
  food_img VARCHAR(255) NOT NULL,
  cook_time TIME NOT NULL,
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  CONSTRAINT fk_recipes_user_id
    FOREIGN KEY (user_id) 
    REFERENCES  users (id)
    ON DELETE SET NULL ON UPDATE CASCADE
);

CREATE TABLE liquors (
  id INT PRIMARY KEY AUTO_INCREMENT,
  recipe_id INT NOT NULL,
  category_id INT NOT NULL,
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  CONSTRAINT fk_liquors_recipe_id
    FOREIGN KEY (recipe_id) 
    REFERENCES  recipes (id)
    ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT fk_liquors_category_id
    FOREIGN KEY (category_id) 
    REFERENCES  categories (id)
    ON DELETE CASCADE ON UPDATE CASCADE,
);

CREATE TABLE favorites (
  id INT PRIMARY KEY AUTO_INCREMENT,
  user_id INT NOT NULL,
  recipe_id INT NOT NULL,
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  CONSTRAINT fk_favorites_user_id
    FOREIGN KEY (user_id) 
    REFERENCES  users (id)
    ON DELETE SET NULL ON UPDATE CASCADE,
  CONSTRAINT fk_favorites_recipe_id
    FOREIGN KEY (recipe_id) 
    REFERENCES  recipes (id)
    ON DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE reviews (
  id INT PRIMARY KEY AUTO_INCREMENT,
  user_id NOT NULL,
  recipe_id NOT NULL,
  review TEXT NOT NULL
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  CONSTRAINT fk_reviews_user_id
    FOREIGN KEY (user_id) 
    REFERENCES  users (id)
    ON DELETE SET NULL ON UPDATE CASCADE,
  CONSTRAINT fk_reviwes_recipe_id
    FOREIGN KEY (recipe_id) 
    REFERENCES  recipes (id)
    ON DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE categories (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(50) NOT NULL
);
