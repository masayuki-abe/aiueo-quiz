<template>
  <main>
    <p>{{ question }}</p>
    <p>{{ answer }}</p>
    <p>{{ answerArray }}</p>
  </main>
</template>

<script>
import aiueoArray from 'assets/js/AiueoArray'
export default {
  data () {
    return {
      question: '',
      answer: []
    }
  },
  computed: {
    // あいうえお配列を呼び出し
    getBaseArray () {
      return aiueoArray
    },
    // あいうえお配列からランダムで（配列の番号を）抽出する
    getRandomArrayNumber () {
      return Math.floor(Math.random() * this.getBaseArray.length)
    },
    // 抽出した配列番号の配列を取得
    answerArray () {
      const answerArray = this.getBaseArray[this.getRandomArrayNumber]
      return answerArray
    }
    // computedで抽出した配列番号のあいうえお配列のjaの値をdata変数に格納
    // getQuestion () {
    //   this.question = this.baseArrays[this.getRandomArrayNumber].ja
    //   return this.question
    // },
    // getAnswer () {
    //   this.answer = this.baseArrays.filter((_, index) => index === this.getRandomArrayNumber)
    //   return this.answer
    // }
  }
}
</script>
