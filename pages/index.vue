<template>
  <div>
    <header>
      <div class="level-toggle-switch" :class="{'hard': hardMode}" @click="levelChange">
        <button><span>れべる</span></button>
        <p v-if="hardMode">
          むずかしい
        </p>
        <p v-else>
          かんたん
        </p>
      </div>
    </header>
    <p class="question-text" :class="questionClassText">
      したのひらがなからはじまる<br>
      えをえらぼう！<br>
      <span>{{ questionText }}</span>
    </p>
    <ul class="answer-list" :class="{'hard': hardMode}">
      <li v-for="questionArray in shuffleArray" :key="questionArray.en" :class="questionArray.en">
        <template v-if="questionClassText === questionArray.en">
          <p class="question-card" @click="okJudgement">
            <img class="question-img" :src="require(`~/assets/img/${questionArray.src[randomeAnswerNumber]}.png`)"><br>
            <span v-if="!hardMode" class="hint-text">{{ questionArray.list[randomeAnswerNumber] }}</span>
          </p>
          <transition
            name="answerCard"
          >
            <div v-if="okFlag" class="answer-card" @click="okAnswer">
              <p class="ok-text">
                ☆せいかい☆
                <span>◎</span>
              </p>
              <p class="ok-image">
                <img :src="require(`~/assets/img/${questionArray.src[randomeAnswerNumber]}.png`)">
                {{ questionArray.list[randomeAnswerNumber] }}
              </p>
              <p class="btn-next">
                つぎのもんだい
              </p>
            </div>
          </transition>
        </template>
        <template v-else>
          <p ref="ngAnswers" class="question-card ng" @click="ngAnswer">
            <img class="question-img" :src="require(`~/assets/img/${questionArray.src[randomeAnswerNumber]}.png`)"><br>
            <span v-if="!hardMode" class="hint-text">{{ questionArray.list[randomeAnswerNumber] }}</span>
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
      okFlag: false,
      hardMode: false
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
    levelChange () {
      this.hardMode = !this.hardMode
    },
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
    okJudgement () {
      this.okFlag = true
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
      const ngAns = document.querySelectorAll('.active')
      ngAns.forEach(function (target) {
        target.classList.remove('active')
      })
      this.okFlag = false
    },
    ngAnswer (e) {
      e.currentTarget.className = 'active'
    }
  }
}
</script>

<style lang="scss" scoped>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  position: relative;
}
header{
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-end;
  .level-toggle-switch{
    display: flex;
    flex-wrap: wrap;
    flex-direction: column;
    align-items: center;
    button{
      position: relative;
      width: 60px;
      height: 30px;
      border: none;
      border-radius: 12px;
      background-color: #57cc99;
      transition: background .4s ease;
      span{
        display: none;
      }
      &::after{
        content: "";
        position: absolute;
        right: 33px;
        top: 3px;
        width: 24px;
        height: 24px;
        border-radius: 100%;
        background-color: #fff;
        transition: right .4s ease;
      }
    }
    p{
      font-size: 12px;
    }
    &.hard{
      button{
        background-color: #e05d5d;
        &::after{
          right: 3px;
        }
      }
    }
  }
}
.question-text{
  padding: .5em 0;
  font-size: 20px;
  text-align: center;
  span{
    display: block;
    padding-top: .2em;
    font-size: 64px;
    text-align: center;
    line-height: 1;
  }
}
.answer-list{
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  width: 100%;
  li{
    width: 45%;
    margin-bottom: 20px;
    padding: 10px;
    border: 1px #efefef solid;
    text-align: center;
    img{
      object-fit: contain;
      width: auto;
      max-width: 100%;
      height: 150px;
    }
    p{
      &.active{
        position: relative;
        &:after{
          content: '';
          position: absolute;
          left: 0;
          top: 0;
          width: 100%;
          height: 100%;
          background-color: rgba(255, 0, 0, 0.7);
        }
      }
    }
    .answerCard-enter-active, .answerCard-leave-active { transition: opacity .5s; }
    .answerCard-enter, .answerCard-leave-active { opacity: 0; }
    .answer-card{
      display: flex;
      flex-wrap: wrap;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      position: absolute;
      left: 0;
      top: 0;
      z-index: 2;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, .7);
      color: #fff;
      .ok-text{
        display: flex;
        flex-wrap: wrap;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        font-size: 24px;
        span{
          color: #80ed99;
          font-size: 32px;
        }
      }
      .ok-image{
        display: flex;
        flex-wrap: wrap;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        margin-bottom: 20px;
        padding: 10px;
        border: 1px #efefef solid;
      }
      .btn-next{
        position: relative;
        padding: .5em 2em;
        border: 1px #efefef solid;
        border-radius: 10px;
        &::before{
          content: '';
          position: absolute;
          top: 1em;
          right: 1em;
          width: .5em;
          height: 2px;
          background-color: #fff;
          transform: rotate(45deg);
        }
        &::after{
          content: '';
          position: absolute;
          top: 1.3em;
          right: 1em;
          width: .5em;
          height: 2px;
          background-color: #fff;
          transform: rotate(-45deg);
        }
      }
    }
  }
  &.hard{
    li{
      &:nth-child(1){
        .question-img{
          filter: blur(0);
          animation: blur1 3s infinite;
        }
      }
      &:nth-child(2){
        .question-img{
          filter: blur(0);
          animation: blur2 3s infinite;
        }
      }
      &:nth-child(3){
        .question-img{
          filter: blur(0);
          animation: blur3 3s infinite;
        }
      }
      &:nth-child(4){
        .question-img{
          filter: blur(0);
          animation: blur1 4s infinite;
        }
      }
    }
  }
}
@keyframes blur1 {
  0%{
    filter: blur(0)
  }
  50%{
    filter: blur(10px)
  }
}
@keyframes blur2 {
  0%{
    filter: blur(0)
  }
  30%{
    filter: blur(0)
  }
  80%{
    filter: blur(10px)
  }
}
@keyframes blur3 {
  0%{
    filter: blur(0)
  }
  20%{
    filter: blur(0)
  }
  70%{
    filter: blur(10px)
  }
}
@keyframes blur4 {
  0%{
    filter: blur(0)
  }
  10%{
    filter: blur(0)
  }
  60%{
    filter: blur(10px)
  }
}
</style>
