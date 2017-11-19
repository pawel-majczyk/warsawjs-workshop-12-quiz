<template>
<v-dialog v-model="modalVisible">
    <v-btn slot="activator">Dodaj pytanie</v-btn>
  <v-card>
    <v-flex md8 xs10>
      <v-card-title>Do tej pory w quizie mamy {{numQuestions}} pytań, dodaj kolejne.</v-card-title>
      <v-form>
        <v-card-text>
        <v-text-field label="treść pytania" v-model="userQuestion.title" multi-line></v-text-field>
        <v-text-field v-model="userQuestion.answers[0]"></v-text-field>
        <v-text-field v-model="userQuestion.answers[1]"></v-text-field>
        <v-text-field v-model="userQuestion.answers[2]"></v-text-field>
        <v-text-field v-model="userQuestion.answers[3]"></v-text-field>
        <v-text-field v-model="userQuestion.correctAnswerIndex"></v-text-field>
        <label>Która z odpowiedzi jest poprawna?</label>
        <v-radio-group v-model="userQuestion.correctAnswerIndex">
          <template v-for="(answer, index) in userQuestion.answers">
              <v-radio :label="answer" :value="index" :key="index"></v-radio>
            </template>
            </v-radio-group>

        <v-btn @click="addQuestion">Dodaj pytanie</v-btn>
        <v-btn @click="modalVisible = false">Zamknij</v-btn>
        </v-card-text>
        </v-form>
    </v-flex>
  </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "add-question",
  data() {
    return {
      userQuestion: {
        title: "Jakie jest twoje pytanie?",
        answers: ["Odpowiedź A", "Odpowiedź B", "Odpowiedź C", "Odpowiedź D"],
        correctAnswerIndex: 0
      },
      modalVisible: false
    };
  },
  props: ["numQuestions"],
  computed: {
    // normalizeQuestion() {
    //   return {
    //     title: this.userQuestion.title,
    //     answers: this.userQuestion.answers,
    //     correctAnswerIndex: +this.userQuestion.correctAnswerIndex
    //   };
    // }
  },
  methods: {
    addQuestion() {
      this.$emit("new-question", this.userQuestion);
    }
  }
};
</script>

