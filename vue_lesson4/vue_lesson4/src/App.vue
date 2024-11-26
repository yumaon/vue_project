<script setup>
import { ref, watchEffect, watch } from 'vue'
// watchEffectもcomputedと等と同じシステムで動くようになっていて、引数に関数を入れる。
// 引数で入れた関数は、watchEffectが呼び出された時に同時に実行される。
// computedと同じように、その実行中にアクセスしたリアクティブなデータを一時的に監視する。
// なのでcomputedとほどんと同じ動きになる。
const count = ref(0)
watchEffect(() => {
  console.log('watchEffect')
  console.log(count.value)
  // ウォッチャーは副作用を入れることができるため、（以下例）非同期の処理でsetTimeoutとか書くことができる。
  setTimeout(() => {
    // 一秒後に以下が出力される。
    console.log('after 1 second')
  }, 1000)
  count.value = 'hello'
})
// 注意点としてwatchEffectやcomputedは、変更を監視する対象になるのは、関数を実行した時に、アクセスしたリアクティブなデータだけ。
// つまり、データを取得している、読み取っている必要がある。
// 例えば、上記のconsole.log(count.value)をコメントあうとしたら、count.valueの値はセットしてるが、
// 誰もcount.valueの値を読み取っていない、アクセスしていないので、この場合は、count.valueは監視されない。
// そのため、ボタンを押しても、watchEffectは実行されない。
// なので、変更を検知したいといった場合は、データにアクセスすること。

// また、もう一つ注意点として、データが監視されるためには、そのアクセスが同期的、つまり普通に上から順番に実行されるような処理の中で行われている必要がある。
// 例えば、setTimeoutのような非同期の処理の中でcount.valueにアクセスしても正しく監視されない。

// watchもwatchEffectと同じように関数になっているが、watchEffectと違って引数を二つとる。
// 一つ目の引数に監視したいリアクティブなデータを入れる。
// 第二引数にその監視しているデータが更新された時に実行したい処理を関数で書く。
const count2 = ref(0)
const count3 = ref(0)
const count4 = ref(0)
watch(count2, () => {
  console.log('watch')
  console.log(count2.value, count3.value, count4.value)
})
// watchとwatchEffectの違いは何かというと、明示的に監視したいデータを指定するかどうか。
// watchはwatchEffectと違って、内部で使用しているデータ全てを監視するわけではない。
// watchEffectの場合はcount2,3,4全て値が更新されたときに毎回実行される。
// しかし、watchはあくまでも、count2の値が更新された時だけ実行される。
watchEffect(() => {
  console.log('watchEffect')
  console.log(count2.value, count3.value, count4.value)
})
// watchの第二引数の関数には引数を二つとることができる。
// 一つは第一引数で監視しているデータの最新のデータの値が入り、第二引数には、変更前の値が入る。
watch(count2, (newValue, oldValue) => {
  console.log('watch')
  console.log('newValue', newValue)
  console.log('oldValue', oldValue)
})
// また以下のように、第一引数にも関数を入れられるようになっている。
// このように書いた場合は、この関数は、watchEffectと同じような動きをする。（watch(() => {}この部分）
watch(
  () => {
    console.log('watch first argument')
    return count3.value
  },
  (newValue, oldValue) => {
    console.log('watch')
    console.log('newValue', newValue)
    console.log('oldValue', oldValue)
  },
)
// これでwatchEffectと同じような動きをするため、ページをリロードすると、watch first argumentと出力され、count3の値が更新されれても実行される。
// この時に、ついでじゃないけど、第二引数の関数も実行されるようになっている。（newValueとoldValueの出力の部分）
// この時のnewValue,oldValueには、第一引数の返り値が入って、上記の例でいうと、count3のnewとoldが入る
// 注意点として、watchではこの第二引数のnewValueとoldValueが同じ値であったときは、この第二引数の関数は実行されないという性質をもっている。
// 例えば、return 0 みたいな感じにすると返り値が0のまま一定のため、第二引数の関数は実行されない。
// なので、もし毎回関数を実行させたい場合は、必ず監視するデータが返り値に影響するように書くようにすること。

// また、複数のリアクティブなデータを監視したい場合は、第一引数に配列を入れることもできる
watch([count3, count4], (newValue, oldValue) => {
  console.log('watch')
  console.log('newValue', newValue)
  console.log('oldValue', oldValue)
})
// ちなみに出力はこんな感じになる
// watch
// newValue (2) [1, 3]
// oldValue (2) [1, 2]

// また、例えば、リアクティブオブジェクトの特定のプロパティを監視したい場合
const sampleObject = ref({
  a: 0,
})
// このようにしたら、0を指定したのと同じ意味になってしまう。なので、この書き方はできない。
// watch(sampleObject.value.a, (newValue, oldValue) => {
//   console.log('watch sampleObject')
//   console.log('newValue', newValue)
//   console.log('oldValue', oldValue)
// })
// なので、リアクティブオブジェクトのプロパティを監視したい場合は、必ず関数を使用した書き方で指定する必要があったりする
watch(
  () => sampleObject.value.a,
  (newValue, oldValue) => {
    console.log('watch sampleObject')
    console.log('newValue', newValue)
    console.log('oldValue', oldValue)
  },
)
</script>

<template>
  <p>{{ count }}</p>
  <button @click="count++">+1</button>

  <p>{{ count2 }}(count2)</p>
  <p>{{ count3 }}(count3)</p>
  <p>{{ count4 }}(count4)</p>
  <button @click="count2++">+1 count2</button>
  <button @click="count3++">+1 count3</button>
  <button @click="count4++">+1 count4</button>
</template>
