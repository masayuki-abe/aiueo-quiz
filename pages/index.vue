<template>
  <div>
    <p :class="questionClassText">
      Question：{{ questionText }}
    </p>
    <ul class="answer-list">
      <li v-for="questionArray in shuffleArray" ref="answerList" :key="questionArray.en" :class="questionArray.en" @click="checkAnswer">
        {{ questionArray.list[randomeAnswerNumber] }}
      </li>
    </ul>
  </div>
</template>

<script>
import aiueoArray from 'assets/js/AiueoArray'
export default {
  data () {
    return {
      getBaseArray: aiueoArray, // あいうえおの配列
      questionText: '', // 問題になる五十音
      questionClassText: '', // 問題になる五十音と正解を紐付けるためのclass
      notAnswerArray: [],
      sliceArray: [],
      concatArray: [],
      shuffleArray: [],
      newRandomArrayNumber: Number,
      randomeAnswerNumber: Number,
      nogood: true
    }
  },
  computed: {
    // あいうえお配列からランダムで（配列の番号を）抽出する
    getRandomArrayNumber () {
      return Math.floor(Math.random() * this.getBaseArray.length)
    },
    // 抽出した配列番号の配列からjaの値を取得
    question () {
      const question = this.getBaseArray[this.getRandomArrayNumber].ja
      return question
    },
    questionClass () {
      const questionClass = this.getBaseArray[this.getRandomArrayNumber].en
      return questionClass
    },
    // 抽出した配列番号の配列からlistの値を取得
    answerArray () {
      const answerArray = this.getBaseArray[this.getRandomArrayNumber].list
      const randamAnswerArrayNumber = Math.floor(Math.random() * answerArray.length)
      return answerArray[randamAnswerArrayNumber]
    },
    // 抽出した配列番号以外の配列を取得
    notAnswerArrays () {
      const notAnswerArrays = this.getBaseArray.filter((_, index) => index !== this.getRandomArrayNumber)
      return notAnswerArrays
    },
    // 0-2の乱数
    randomNumber () {
      return Math.floor(Math.random() * 3)
    }
  },
  mounted () {
    this.intoData()
    this.intoAnswers()
  },
  methods: {
    // データを取得して代入
    intoData () {
      this.questionText = this.question
      this.questionClassText = this.questionClass
      this.randomeAnswerNumber = this.randomNumber
      this.notAnswerArray = this.notAnswerArrays
    },
    // 初期にマウントするためのメソッド
    intoAnswers () {
      this.sliceArray = this.notAnswerArrays.slice(0, 3)
      this.concatArray = this.sliceArray.concat(this.getBaseArray[this.getRandomArrayNumber])
      this.shuffleArray = this.shuffle(this.concatArray).slice(0, 4)
    },
    // 配列をランダムに並び替えるメソッド
    shuffle (array) {
      for (let i = array.length - 1; i > 0; i--) {
        const r = Math.floor(Math.random() * (i + 1))
        const tmp = array[i]
        array[i] = array[r]
        array[r] = tmp
      }
      return array
    },
    checkAnswer (e) {
      const clickClass = e.currentTarget.getAttribute('class')
      // const answerLists = this.$refs.answerList
      if (this.questionClassText === clickClass) {
        const ngList = document.querySelectorAll('.ng')
        this.newRandomArrayNumber = Math.floor(Math.random() * this.getBaseArray.length)
        this.randomeAnswerNumber = Math.floor(Math.random() * 3)
        this.questionText = this.getBaseArray[this.newRandomArrayNumber].ja
        this.questionClassText = this.getBaseArray[this.newRandomArrayNumber].en
        this.notAnswerArray = this.getBaseArray.filter((_, index) => index !== this.newRandomArrayNumber)
        this.sliceArray = this.notAnswerArray.slice(0, 3)
        this.concatArray = this.sliceArray.concat(this.getBaseArray[this.newRandomArrayNumber])
        this.shuffleArray = this.shuffle(this.concatArray).slice(0, 4)
        ngList.classList.remove('ng')
      } else {
        e.currentTarget.classList.add('ng')
      }
    }
  }
}
</script>
