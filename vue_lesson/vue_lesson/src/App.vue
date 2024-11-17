<script setup>
import { ref, reactive } from 'vue'

// let a = 1
// let b = 2
// let c = a + b // c = 3

// a = 4
// console.log(c)

const title = ref('Vue.js Course')
let price = ref(9.99)
// refオブジェクトにはvalueというプロパティがあり、price.valueとすることで元の9.99を取得することができる
console.log(price.value)
/**
 * また、リアクティブシステムんい対応している状態のことを形容詞でリアクティブと言ったりする
 */

// refの引数にオブジェクトも入れることができる。
/**
 * この場合も内部のプロパティのstudents,ratingを取得したい場合は、valueの中に実際のデータが入っているので「.value」をつける
 */
const info = ref({
  students: 1000,
  rating: 4,
})
/**
 * 上記のinfoはもともとオブジェクトであるため、リアクティブにするために、わざわざもう一つ新しいオブジェクトを作成してvalueプロパティでアクセスするのは二度手間
 * もともとがオブジェクトのため、そのままリアクティブにすればよい。そのために、reactive関数を使用する。
 */

// リアクティブオブジェクト
const instructor = reactive({
  name: 'yosi',
  age: 25,
  // リアクティブアブジェクトの中にあるオブジェクトはリアクティブオブジェクトになる。
  sns: {
    twitter: '@_sample',
    youtube: '@sample',
  },
  // リアクティブオブジェクトの中に、ref関数が使用されているパターン
  email: ref('sample@sample.com'),
})

// 新しいプロパティを追加する場合は以下
instructor.bio = 'hello'

console.log(instructor)
console.log(info)
// reactiveオブジェクトの場合はvalue無しで取得できる
console.log(instructor.age)
// refオブジェクトの場合はvalueをつけて取得する
console.log(info.value.students)
// リアクティブオブジェクトの中にref関数を使用してるemailプロパティの値を取得するときは以下。refを使用しているがこの場合はvalueを省略できる。
console.log(instructor.email)
/**
 * これは、vue.jsはリアクティブオブジェクトのプロパティのアクセスするときに、毎回そのプロパティがrefオブジェクトがどうかをチェックしているため。
 * refオブジェクトであったら、自動的に内部的にvalueをつけている
 * ただし、値が配列の時は自動的にvalueをつけるという処理は行われない。（例、以下items）
 */
const items = reactive([ref(1), ref(2), ref(3)])
console.log('items', items[0].value) // これで「1」が取得できる

function increment() {
  price.value += 1
  instructor.age += 1
  instructor.bio += '!'
  // ここでもリアクティブオブジェクトの中のrefオブジェクトを更新する際にでもvalueは必要ない
  instructor.email = 'hoge@hoge'
}
// 公式のドキュメントでは、オブジェクトをリアクティブにしたい場合、ref関数とreactive関数ではref関数が推奨されている

// また、refオブジェクトがreacitveオブジェクトの中ではなくて、普通のオブジェクトの中にある場合
// 例、以下courseInfo
/**
 * これらをtemplateからアクセスする際は注意が必要(自動的にvalueがつかない)
 * vue.jsがrefオブジェクトに対してvalueを自動でつける際は、各データの頭しか確認していないため。
 * つまり、一番左だけの値だけを確認し、もうそれ以降、右側のstudentや、sectionのようなデータに対しては、refオブジェクトかどうかは確認していない。
 * 逆を言うと、refオブジェクトのため、valueをつけるという処理はデータの先頭に対してのみ行われる。
 *
 * ではreactiveオブジェクトはどうなのか。
 * reactiveオブジェクトには独自の機能としてプロパティの値がrefオブジェクトであれば、script内、template内でも、valueを自動的につけてくれるという処理がある。
 *
 * 結局、一番左側が、ref、reactiveオブジェクトでもなく、普通のオブジェクトの時だけ、それ以降、valueが自動でつかなくなるという動きになる。
 */
const courseInfo = {
  sections: ref(10),
  language: ref('Japanese'),
}
console.log(courseInfo.sections.value)
console.log(courseInfo.language.value)
</script>
<template>
  <!-- ref関数でtemplateの方では「.value」をつけなくて良い。-->
  <!-- 変数や定数に対しては、自動的にrefオブジェクトがどうかを毎回内部的にチェックしていて、refオブジェクトの場合は「.value」を自動的につける動きになっている-->
  <h1>Title: {{ title }}</h1>
  <h2>Price: ${{ price - 1 }}</h2>
  <button @click="increment">button</button>
  <h2>Students: {{ info.students }}</h2>
  <h2>Instructor age: {{ instructor.age }}</h2>
  <h2>Instructor bio: {{ instructor.bio }}</h2>
  <h2>Instructor twitter: {{ instructor.sns.twitter }}</h2>
  <h2>Instructor youtube: {{ instructor.sns.youtube }}</h2>
  <h2>Instructor email: {{ instructor.email }}</h2>
  <h2>Course Info: {{ courseInfo.sections.value + 1 }}</h2>
</template>
