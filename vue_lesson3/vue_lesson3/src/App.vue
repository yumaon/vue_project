<script setup>
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <!-- イベント修飾子を使用し、キーボードイベントに対して処理をする
       $event（イベントオブジェクトを取得できるもの）には、
       preventDefault()メソッドだったり、stopPropagation()というメソッドがあったりする。
       preventDefault()メソッドは、デフォルトの挙動を防ぐというメソッドになる。 
       stopPropagation()メソッドは、伝播をとめる-->
  <button @click="$event.preventDefault()">button</button>
  <!-- preventDefault()の例 -->
  <!-- 例えば、以下は、vuejs.orgページに遷移するというのが、デフォルトの挙動 -->
  <a href="https://vuejs.org">Vue.js</a>
  <!-- 以下は、デフォルトの挙動を防いだもの。リンクをクリックしても画面遷移しない。 -->
  <a @click="$event.preventDefault()" href="https://vuejs.org">Vue.js</a>

  <!-- stopPropagation()の例 -->
  <!-- 以下は、ボタンを押すと、カウントされる -->
  <p>{{ count }}</p>
  <div @click="count++">
    <button @click="">countBTN</button>
  </div>
  <!-- 以下は、stopPropagationメソッドを使用したもの。こちらはボタンを押下しても、カウントされない。 -->
  <!-- つまり、親要素にclickイベントが発生したことを伝播させない、伝えないという挙動になる。 -->
  <p>{{ count }}</p>
  <div @click="count++">
    <button @click="$event.stopPropagation()">countBTN</button>
  </div>
  <!-- これら二つのメソッドはよく使うもののため、vue.jsは簡単に実行できるシステムを用意している。
       それが、イベント修飾子というものになる。
       以下例 -->
  <a @click.prevent="" href="https://vuejs.org">Vue.js</a>

  <p>{{ count }}</p>
  <div @click="count++">
    <!-- とくに追加で処理を記述しないのであれば、=""は省略可 -->
    <button @click.stop>countBTN</button>
  </div>
  <!-- また、イベント修飾子は複数つけることも可能 -->
  <a @click.prevent.stop href="https://vuejs.org">Vue.js</a>
</template>
