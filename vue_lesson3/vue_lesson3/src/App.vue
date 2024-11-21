<script setup>
import { ref, computed } from 'vue'

const count = ref(0)

const userInput = ref('')

const eventName = 'keyup'

// computedをつかって複雑な指揮を一つにまとめる方法
/**
 * computedは、リアクティブシステムを保ったまま処理を一つにまとめる方法になる。
 */
const score = ref(0)
const evaluation = score.value > 3 ? 'Good' : 'Bad'
// evaluationには評価された結果がはいるためBadが入る。
// リアクティブシステムを保ったまま、式をまとめる方法がcomputedになる
// computedは引数に関数を入れる必要がある。その関数のなかで、まとめたい処理を書く。
const evaluation_computed = computed((value) => {
  console.log('computed', value)
  return score.value > 3 ? 'Good' : 'Bad'
})
// コンソールで表示させるとほぼrefオブジェクトと同じような表示がでる。コンピューテッドレフオブジェクトと言ったりするし、scriptやtempleteでまるでrefオブジェクトかのように扱える。
console.log(evaluation_computed.value)
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

  <!-- キーボードを入力して放したときに発生するイベント -->
  <p>{{ count }}</p>
  <input type="text" @keyup="count++" />
  <!-- 上記のイベントを特定のキーが入力したときに発生させたいといったときに、キー修飾子を使用する -->
  <!-- 以下は、スペースとdeleteキーで反応する -->
  <input type="text" @keyup.space.delete="count++" />

  <!-- ディレクティブの構造（例：v-on:click.prevent="changeData"）
       v-on 名前
       click 引数
       prevent 修飾子
       changeData 値 -->
  <!-- 角括弧（[]）を使用してディレクティブの引数にscriptのデータを指定する -->
  <input type="text" @[eventName].space.delete="count++" />
  <!-- @[eventName]とすることで、keyupの文字列が入りkeyupイベントに対してハンドラが追加されるということになる-->

  <!-- v-modelを使用してinputを単純に扱えるようにする -->
  <p>{{ userInput }}</p>
  <input v-model="userInput" type="text" />

  <!-- computedをつかって複雑な指揮を一つにまとめる方法 -->
  <p>{{ score > 3 ? 'Good' : 'Bad' }}</p>
  <p>{{ evaluation }}</p>
  <!-- この上記のコードは同じ処理ではない。evaluationには評価された結果 -->
  <!-- ではどうやってリアクティブシステムを保ったまま、式をまとめることができるのかというと、それがcomputedになる -->
  <p>{{ evaluation_computed }}</p>
  <p>{{ score }}</p>
  <button @click="score++">+1</button>
</template>
