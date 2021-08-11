<template>
  <main>
    <p>{{ question }}</p>
    <p>{{ answer }}</p>
    <p>{{ baseArrays }}</p>
  </main>
</template>

<script>
import aiueoArray from 'assets/js/AiueoArray'
export default {
  data () {
    return {
      baseArrays: aiueoArray,
      question: '',
      answer: []
    }
  },
  computed: {
    // あいうえお配列のjsファイルを呼び出す
    getBaseArray () {
      return aiueoArray
    },
    // あいうえお配列からランダムで（配列の番号を）抽出する
    getRandomArrayNumber () {
      return Math.floor(Math.random() * this.getBaseArray.length)
    }
  },
  mounted () {
    this.importBaseArray()
    this.getQuestion()
    this.getAnswer()
  },
  methods: {
    // computedで呼び出したあいうえお配列をdata変数に格納
    importBaseArray () {
      this.baseArrays = this.getBaseArray
    },
    // computedで抽出した配列番号のあいうえお配列のjaの値をdata変数に格納
    getQuestion () {
      this.question = this.baseArrays[this.getRandomArrayNumber].ja
    },
    getAnswer () {
      this.answer = this.baseArrays.filter((_, index) => index === this.getRandomArrayNumber)
    }
  }
}
</script>
