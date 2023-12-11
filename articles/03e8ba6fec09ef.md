---
title: "【Laravel】.env.testingがテスト実行時に使用されない"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Laravel"]
published: true
---

# 概要
Laravelのテスト環境構築時に.env.testingを作成したのですが、テスト用のDBが使用されませんでした。
テストに以下のコードを仕込んで確認したところ、テスト中に.env.testingが使用されていないことを確認し、結構はまったので書き残しておきます。
```:.env.testing
APP_NAME=Laravel-Test
```

```php:確認用テスト
class ExampleTest extends TestCase
{
    /**
     * A basic test example.
     */
    public function test_the_application_returns_a_successful_response(): void
    {
        $response = $this->post('/v3.0/lives');

        // .env.testingで定義したLaravel-Testが出力されるかを確認
        $this->assertEquals(env('APP_NAME'), 'Laravel-Test');
    }
}
```

```:結果
 FAILED  Tests\Feature\ExampleTest > the application returns a successful response                                                                                                                                                                                     
  Failed asserting that two strings are equal.
  -'Laravel'
  +'Laravel-Tes'
```

# 結論
compose.yml内でAPP_ENVを定義していた

原因は不明なのですが、PHPコンテナの環境変数で以下のようにAPP_ENVが定義されており、
削除したところ.env.testingが読み込まれるようになりました。

```php
services:
  php:
    (略)
    environment:
      - APP_ENV=${APP_ENV:-local} // ←これ
```

なぜか解消したのですが、今のところ原因は不明です。
判明次第更新したいと思います。