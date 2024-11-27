<script setup>
import { ref } from 'vue'

const isRed = ref(true)
const isBgBlue = ref(true)
const className = ref('red')
function toggleClass() {
  // それぞれのvalueを反転させたものにしている。
  isRed.value = !isRed.value
  isBgBlue.value = !isBgBlue.value
  //className.value = 'bg-blue'
}
</script>

<template>
  <!-- HTMLのクラス属性にv-bindをつけた時の動き -->
  <!-- このようにクラスをオブジェクトとして書いて、キーをクラス名を書く。値に、クラスを適用させるかさせないかをtrue,falseで書く -->
  <!-- javascriptの場合はキーにハイフンが入っている場合は、クォーテーションで囲む必要がある。 -->
  <!-- vue.jsではclassの属性値としてこんな分にオブジェクトでjavascriptから操作しやすいように指定することができる。 -->
  <div :class="{ red: isRed, 'bg-blue': isBgBlue }">hello</div>
  <!-- また、オンオフで適用させるクラスを切り替えるのではなく、直接クラスの名前を動的に指定する方法もある -->
  <div :class="[className]">hello</div>
  <!-- その上で、この配列の中に、オブジェクトの指定の仕方を書くこともできる。 -->
  <div :class="[className, { 'bg-blue': isBgBlue }]">hello</div>
  <!-- クラスに関しては、普通の静的なクラスも同じ要素に対して同時につけることもできる -->
  <div class="border" :class="[className, { 'bg-blue': isBgBlue }]">hello</div>
  <button @click="toggleClass">button</button>
</template>
<style>
.red {
  color: red;
}
.bg-blue {
  background-color: blue;
}
.border {
  border: 1px solid red;
}
</style>
