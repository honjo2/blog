---
layout: post
title: "EmberFireが出た"
date: 2013-10-23 13:09
comments: true
categories: EmberFire Firebase EmberJS
---

# EmberFireとは？
- FirebaseをEmberJSで使えるようにするためのライブラリです。
- [Announcing Firebase Bindings for EmberJS](https://www.firebase.com/blog/2013-10-22-firebase-bindings-for-ember.html)

# サンプルを動かす
1. `git clone https://github.com/firebase/emberFire.git`
1. `cd emberFire`
1. `open examples/chat/index.html`

    ![](/images/2013-10-23/emberfirechat.png)

# ソース
```html index.html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>EmberFire Test chat</title>
  <link rel="stylesheet" type="text/css" href="https://www.firebase.com/css/example.css">
  <style>
    button {
      width: 15%;
      font-size: 24px;
    }
    #nameInput {
      width: 20%;
    }
    #messageInput {
      width: 60%;
    }
  </style>
</head>
<body>

  <script type="text/x-handlebars">
    <h2>EmberFire Chat</h2>
    ｛｛outlet｝｝
  </script>

  <!-- データを全て表示 -->
  <script type="text/x-handlebars" data-template-name="index">
    ｛｛#scrolling-div id="messagesDiv" update-when=this｝｝
      ｛｛#each｝｝
        <div><em>｛｛from｝｝</em>: ｛｛msg｝｝</div>
      ｛｛/each｝｝
    ｛｛/scrolling-div｝｝
    <form ｛｛action "sendMessage" on="submit"｝｝>
      ｛｛input id="nameInput" value=from placeholder="Name"｝｝
      ｛｛input id="messageInput" value=msg placeholder="Message..."｝｝
      <button type=submit>Send</button>
    </form>
  </script>

  <script type="text/x-handlebars" id="components/scrolling-div">
    ｛｛yield｝｝
  </script>

  <!-- FireEmberに必要なjs -->
  <script src="https://cdn.firebase.com/v0/firebase.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/handlebars.js/1.0.0/handlebars.min.js"></script>
  <!--<script src="http://builds.emberjs.com/tags/v1.0.0/ember.js"></script>-->
  <script src="http://builds.emberjs.com/ember-latest.js"></script>
  <script src="../../emberfire-latest.js"></script>

  <!-- このアプリ用のjs -->
  <script src="app.js"></script>
</body>
</html>
```

```javascript app.js
App = Ember.Application.create();

// 使用するFirebaseを指定
App.IndexRoute = Ember.Route.extend({
  model: function() {
    return EmberFire.Array.create({
      ref: new Firebase("https://ember-chat.firebaseio-demo.com/")
    });
  }
});

// データを保存
App.IndexController = Ember.ArrayController.extend({
  msg: "",
  from: "Guest" + Math.floor(Math.random() * 100),
  actions: {
    sendMessage: function() {
      this.pushObject({from: this.get("from"), msg: this.get("msg")});
      this.set("msg", null);
    }
  }
});

// HTMLを見やすく
App.ScrollingDivComponent = Ember.Component.extend({
  scheduleScrollIntoView: function() {
    // Only run once per tick, once rendering has completed;
    // avoid flood of scrolls when many updates happen at once
    Ember.run.scheduleOnce("afterRender", this, "scrollIntoView");
  }.observes("update-when.@each"),

  scrollIntoView: function() {
    this.$().scrollTop(this.$().prop("scrollHeight"));
  }
});
```

# データ
- データはこんな感じで保存されています

    ![](/images/2013-10-23/firebasedata.png)

- ちなみにこれはデモ用のデータベースなので誰でも見ることができます
  - <https://ember-chat.firebaseio-demo.com/>