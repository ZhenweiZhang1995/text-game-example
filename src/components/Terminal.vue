<template>
  <div class="terminal-container">
    <div>
      <!-- <img class="logo" src="static/ForstGameLogo.png"> -->
    </div>
      <textarea readonly="true" class="scroll-window">{{ log }}</textarea>
      <!-- <img class="image" src="static/Red-Dragon.png"> -->

    <input v-model="command" class="input" name="command" @keyup="evaluateResponse" autofocus>
    <br>
    <br>
    <button type="button" class = "button" @click="evaluateResponse"> Enter</button>

  </div>
</template>

<script>
import questions from './questions'
import texts from './texts'
export default {
  name: 'terminal',
  data: function () {
    return {
      log: '',
      current_question: 'age',
      history: [
        'Welcome',
        'What is your age?'
      ],
      command: '',
      questions,
      texts
    }
  },
  created: function () {
    this.log = this.history.join('\n')
  },
  methods: {
    evaluateResponse: function (event) {
      let reply
      let question
      // let type
      if (event.type === 'keyup' && event.keyCode !== 13) {
        return false
      }
      this.addToHistory()
      // type = this.questions[this.current_question].type
      // switch (type) {
        // case 'numeric':
        // break
        // case 'selection':
        // break
      // }
      switch (this.current_question) {
        case 'age':
          question = this.questions['age']
          reply = (Number(this.command) < question.criteria) ? this.texts[question.validOptions.below] : this.texts[question.validOptions.above]
          this.current_question = 'path'
          break
        case 'path':
          question = this.questions[this.current_question]
          if (question.validOptions[this.command] !== undefined) {
            reply = this.texts[question.validOptions[this.command]]
            this.current_question = this.command === question.choice[0] ? question.validOptions[this.command] : question.validOptions[question.choice[1]]
          } else {
            reply = ['I dont know what this means.Try something else!'] //, ...this.texts[' ']]
          }
          break
        case 'dragon':
          question = this.questions['dragon']
          if (question[this.command] !== undefined) {
            reply = this.texts[question[this.command]]
            this.current_question = this.command === 'stick' ? question[this.command] : 'cabin'
          } else {
            reply = ['I dont know what this means. Try something else!'] //, ...this.texts[' ']]
          }
          break
        case 'cabin':
          question = this.questions['cabin']
          if (question[this.command] !== undefined) {
            reply = this.texts[question[this.command]]
            this.current_question = this.command === 'knock' ? question[this.command] : 'sword'
          } else {
            reply = ['I dont know what this means. Try something else!'] //, ...this.texts[' ']]
          }
          break
        case 'sword':
          question = this.questions['sword']
          if (question[this.command] !== undefined) {
            reply = this.texts[question[this.command]]
            this.current_question = this.command === 'lady' ? question[this.command] : 'lady'
          } else {
            reply = ['I dont know waht this means. Try something else!']
          }
          break
        case 'lady':
          console.log(question)
          question = this.questions['lady']
          if (question[this.command] !== undefined) {
            reply = this.texts[question[this.command]]
            console.log(question[this.command])
            this.current_question = this.command === 'kill' ? question[this.command] : 'keep'
          } else {
            reply = ['I dont know waht this means. Try something else!']
          }
          break
      }
      this.history = [...this.history, ...reply]
      this.log = this.history.join('\n')
      let scrollwindow = this.$el.querySelector('.scroll-window')
      scrollwindow.scrollTop = scrollwindow.scrollHeight
      this.command = ''
      responsiveVoice.speak(reply, "UK English Female",
          {pitch: 1},{rate:0.5});}
    },
    addToHistory: function () {
      this.history.push('> ' + this.command)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

 .terminal-container {
   text-align: left ;
 }

input[name='command'] {

    margin-top: 1em;
    height: 2em;
    width: 80%;
    border: 1px solid grey;
}

.scroll-window {
  width: 90%;
  height: 28em;
  border: 1px solid grey;
  margin-top: 2em;
  font-size: 100%;
}
 .logo {
   image-orientation: top;
   width: 20%;
 }

h1, h2 {
  font-weight: nomral;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

input{
  font-size: 100%;

}

.button{
  font-size: 100%;
}
</style>
