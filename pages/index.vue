<template>
  <div>
    <p class="question-text" :class="questionClassText">
      したのひらがなからはじまる<br>
      えをえらぼう！<br>
      <span>{{ questionText }}</span>
    </p>
    <ul class="answer-list">
      <li v-for="questionArray in shuffleArray" :key="questionArray.en" :class="questionArray.en">
        <template v-if="questionClassText === questionArray.en">
          <p @click="okAnswer">
            <img :src="require(`~/assets/img/${questionArray.src[randomeAnswerNumber]}.png`)"><br>
            {{ questionArray.list[randomeAnswerNumber] }}
          </p>
        </template>
        <template v-else>
          <p ref="ngAnswers" class="ng" @click="ngAnswer">
            <img :src="require(`~/assets/img/${questionArray.src[randomeAnswerNumber]}.png`)"><br>
            {{ questionArray.list[randomeAnswerNumber] }}
          </p>
        </template>
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
      clickNg: [],
      newRandomArrayNumber: Number,
      randomeAnswerNumber: Number,
      good: ''
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
    okAnswer () {
      this.newRandomArrayNumber = Math.floor(Math.random() * this.getBaseArray.length)
      this.randomeAnswerNumber = Math.floor(Math.random() * 3)
      this.questionText = this.getBaseArray[this.newRandomArrayNumber].ja
      this.questionClassText = this.getBaseArray[this.newRandomArrayNumber].en
      this.notAnswerArray = this.getBaseArray.filter((_, index) => index !== this.newRandomArrayNumber)
      this.sliceArray = this.notAnswerArray.slice(0, 3)
      this.concatArray = this.sliceArray.concat(this.getBaseArray[this.newRandomArrayNumber])
      this.shuffleArray = this.shuffle(this.concatArray).slice(0, 4)
      // answerNgLists.classList.remove('none')
      const ngAns = document.querySelectorAll('.active')
      console.log(ngAns)
      ngAns.forEach(function (target) {
        target.classList.remove('active')
      })
      // document.getElementById('ng').classList.remove('active')
    },
    ngAnswer (e) {
      e.currentTarget.className = 'active'
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.question-text{
  padding: 1em 0;
  font-size: 20px;
  text-align: center;
}
.question-text span{
  display: block;
  padding-top: .5em;
  font-size: 64px;
  text-align: center;
  line-height: 1;
}
.answer-list{
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  width: 100%;
}
.answer-list li{
  width: 45%;
  margin-bottom: 20px;
  padding: 10px;
  border: 1px #efefef solid;
  text-align: center;
}
.answer-list li img{
  object-fit: contain;
  width: auto;
  max-width: 100%;
  height: 200px;
}
.answer-list li p.active{
  position: relative;
}
.answer-list li p.active::after{
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(255, 0, 0, 0.7);
}
</style>
