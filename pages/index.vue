<template>
  <div>
    <p :class="questionClass">
      Question：{{ question }}
    </p>
    <ul class="answer-list">
      <!-- <li class="answer">
        {{ answerArray }}
      </li> -->
      <li v-for="questionArray in shuffleArray" :key="questionArray.en" :class="questionArray.en" @click="checkAnswer">
        {{ questionArray.list[randomNumber] }}
      </li>
    </ul>
    <p style="color: red;">
      {{ concatArray }}
    </p>
  </div>
</template>

<script>
import aiueoArray from 'assets/js/AiueoArray'
export default {
  data () {
    return {
      getBaseArray: aiueoArray,
      answerList: [],
      notAnswer: [],
      shuffleArray: [],
      sliceArray: [],
      concatArray: []
    }
  },
  computed: {
    // あいうえお配列を呼び出し
    // getBaseArray () {
    //   return aiueoArray
    // },
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
    },
    concat () {
      const concat = this.importArray()
      return concat
    }
  },
  mounted () {
    this.sliceArray = this.notAnswerArrays.slice(0, 3)
    this.concatArray = this.sliceArray.concat(this.getBaseArray[this.getRandomArrayNumber])
    this.shuffleArray = this.shuffle(this.concatArray).slice(0, 4)
  },
  methods: {
    importArray () {
      this.notAnswer = this.notAnswerArrays
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
    checkAnswer () {
      if (this.questionClass === this.shuffleArray.en) {
        alert('ok')
      } else {
        alert('ng')
      }
    }
  }
}
</script>
