<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{ question.question }}</template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
          v-for="(shuffleanswer, index) in shuffledAnswers"
          :key="index"
          >{{ shuffleanswer }}</b-list-group-item
        >
      </b-list-group>
      <b-button
        variant="success"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
        class="btn"
        >Submit</b-button
      >
      <b-button @click="next" variant="primary" href="#" class="btn"
        >Next</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    question: Object, //data apapun yang didatangkan dari parent -> misal app.vue di refrensikan pada props
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    };
  },
  computed: {
    answers() {
      let answers = [...this.question.incorrect_answers];
      answers.push(this.question.correct_answer);
      return answers;
    }
  },
  watch: {
    /* fungsi acak jawaban */
    question: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }

    // {
    //   this.selectedIndex = null;
    //   this.shuffleAnswers();
    // }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    submitAnswer() {
      let isCorrect = false;

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;

      this.increment(isCorrect);
    },
    shuffleAnswers() {
      /* import lodash -> npm install lodash */
      let answers = [
        ...this.question.incorrect_answers,
        this.question.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.question.correct_answer
      );
    },
    answerClass(index) {
      let answerClass = "";
      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        answerClass = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = "incorrect";
      }

      return answerClass;
    }
  }
};
</script>

<style scope>
.list-group {
  margin-bottom: 30px;
}
.list-group-item:hover {
  background: #dcdcdc;
  cursor: pointer;
}
.btn {
  margin: 0 7px;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: lightcoral;
}
</style>
