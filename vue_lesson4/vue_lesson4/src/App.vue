<script setup>
import { ref, watchEffect } from 'vue'
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
</script>

<template>
  <p>{{ count }}</p>
  <button @click="count++">+1</button>
</template>
