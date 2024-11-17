<script setup>
import { ref } from 'vue'

const count = ref(2)
const count2 = ref(3)

// v-htmlを使用してHTMLとして出力する
const message = ref('<h1>Hello<h1>')

// v-bindを使用して属性値を指定する
const vueURL = ref('https://vue.js.org')
const vueId = ref('vue-link')

// numberボタンを押されるとカウントされるナンバー
const number = ref(0)

/**
 * イベントオブジェクトの取得方法
 * クリックイベントのようなブラウザが発生させているイベントはそのイベントが発生した時に同時に
 * イベントオブジェクトという、そのイベントの情報を持ったオブジェクトを生成している。
 * vue.jsではそれを取得できるようになっている。
 *
 * まずメソッドハンドラに関しては、（例で、countUp関数を使用）
 * function countUp(event) {
 *   number.value++
 * }
 * こんな感じで、第一引数に自動で入るようになっている。
 */

const pushNunBtn = () => {
  number.value++
}
function countUp(event) {
  //number.value++
  console.log(event)
  // 上記で表示されたもので、例えば「clientX、clientY」はボタンがクリックされた時のマウスの一を取得するもの
  number.value = event.clientX
}
</script>

<template>
  <div>{{ count + count2 }}</div>
  <div>{{ count > 3 ? 'yes' : 'no' }}</div>
  <!-- v-textは、<div>{{ count }}</div>と同じ -->
  <div v-text="count"></div>

  <!-- v-htmlを使用してHTMLとして出力する -->
  <!-- ただし、セキュリティ的な観点で、webページを自由に書き換えられてしまうため、使い方には注意。ユーザーからもらったデータではない、信頼できるデータを指定するようにする。 -->
  <div v-html="message"></div>

  <!-- v-bindを使用して属性値を指定する -->
  <a v-bind:id="vueId" v-bind:href="vueURL">Vue.js</a>
  <!-- v-bind:idやv-bind:~という記述は省略できる。(推奨)-->
  <a :id="vueId" :href="vueURL">Vue.js</a>
  <!-- undefinend,nullで属性を消すこともできる -->
  <a :id="undefined" :href="null">Vue.js</a>

  <!-- Boolean属性に対してv-bindを利用する -->
  <!-- disabledはboolean属性の一つで、値をとらなくても動作する。（disabledがあると無効化になる） -->
  <button disabled>buttom</button>
  <!-- Boolean属性にはfalse,undefined,0,-0,0n,NaN を入れたときに属性がつかなくなる。-->
  <button :disabled="false">button</button>

  <!-- v-bindで一度に複数の属性を指定する -->
  <a v-bind="{ id: vueId, href: vueURL }">Vue.js</a>

  <!-- v-onを用いてクリックなどのイベント発生時に特定の処理をする -->
  <p>{{ number }}</p>
  <button v-on:click="number++">number</button>
  <!-- 上記の「v-on」を省略する記述 -->
  <!-- 処理を直接書くパターンのハンドラーのことをインラインハンドラーという -->
  <button @click="number++">number</button>
  <!-- 関数を使用したもの -->
  <!-- 関数自体を書くハンドラーのことを、メソッドハンドラーという -->
  <button @click="pushNunBtn">number</button>
  <button @click="countUp">number</button>

  <!-- インラインハンドラーではどうやってイベントオブジェクトにアクセスすることができるのか -->
  <button @click="number = $event.clientX">X軸表示</button>
  <!-- $eventにイベントオブジェクトが格納されている -->
</template>
