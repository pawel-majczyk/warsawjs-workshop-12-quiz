<template>
  <v-app dark>
    <v-content>
      <v-container>
        <v-layout row justify-center>
          <v-flex md6 xs10>
            <img class="logo" src="./assets/logo.png"/>
            <v-card>
              <v-card-title>Odpowiedzi: {{userAnswer  + " : " + currentQuestionIndex + " / " + this.quiz.length}} </v-card-title>
              <v-card-title>{{ actualQuestion.title }} </v-card-title>
              <v-list>
                <v-list-tile v-for="(answer, indexOfAnswer) in actualQuestion.answers"
                :key="indexOfAnswer"
                @click="userChoice(indexOfAnswer)"
                :class="answerCheckResult(indexOfAnswer)"
                >
                  {{ answer + " " }}
              </v-list-tile>
              </v-list>
              <div v-if="correctAnswer && gameIsOver" class="success">
              <v-icon >mdi-xbox-controller</v-icon><h2>Gratulacje, zwyciężyłeś!</h2></div>
              <v-btn v-if="correctAnswer && !gameIsOver" @click="showNextQuestion">Następne Pytanie</v-btn>
              <v-btn v-else @click="reboot">Restart</v-btn>
            </v-card>
          </v-flex>
          <v-flex md3>
            <add-question :numQuestions="this.quiz.length"  @new-question="addNewQuestion($event)"></add-question>
            </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import { quiz } from "./quiz";
import AddQuestion from "./components/AddQuestion.vue";

export default {
  components: { "add-question": AddQuestion },
  data() {
    return {
      currentQuestionIndex: 0,
      quiz: quiz,
      userAnswer: null,
      newQuestion: null
    };
  },
  computed: {
    actualQuestion() {
      return this.quiz[this.currentQuestionIndex];
    },
    correctAnswer() {
      if (this.actualQuestion.correctAnswerIndex === this.userAnswer) {
        return true;
      }
    },
    gameIsOver() {
      if (
        this.currentQuestionIndex + 1 >= this.quiz.length &&
        this.actualQuestion.correctAnswerIndex === this.userAnswer
      ) {
        return true;
      }
    }
  },
  methods: {
    userChoice(indexOfAnswer) {
      if (this.userAnswer === null) {
        this.userAnswer = indexOfAnswer;
        return this.userAnswer;
      }
    },
    answerCheckResult(indexOfAnswer) {
      if (
        this.actualQuestion.correctAnswerIndex === this.userAnswer &&
        this.userAnswer === indexOfAnswer
      ) {
        return "success";
      } else if (this.userAnswer === indexOfAnswer) {
        return "error";
      }
    },
    showNextQuestion() {
      this.userAnswer = null;
      this.currentQuestionIndex += 1;
    },
    reboot() {
      this.currentQuestionIndex = 0;
      this.userAnswer = null;
    },
    addNewQuestion(newQuestion) {
      this.quiz.push(newQuestion);
    }
  },
  created() {
    this.$http.get("https://opentdb.com/api.php?amount=10").then(res => {
      // console.log("response", res.data.results);
      let data = res.data.results;
      console.log(data);
      this.quiz = data.map(question => {
        return {
          title: question.question,
          answers: [...question.incorrect_answers, question.correct_answer],
          correctAnswerIndex: question.incorrect_answers.length
        }
      })
    })
  }
};
</script>

<style>
.success {
  text-align: center;
}
.success h2 {
  padding: 1em;
  line-height: 2em;
}

.logo {
  height: 128px;
}
</style>
