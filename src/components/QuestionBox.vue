<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        {{ question.question }}
      </template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']"
          v-for="(answer, index) in answers"
          :key="index"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>
      <b-button variant="success" href="#" class="btn">Submit</b-button>
      <b-button @click="next" variant="primary" href="#" class="btn"
        >Next</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    question: Object, //data apapun yang didatangkan dari parent -> misal app.vue di refrensikan pada props
    next: Function
  },
  data() {
    return {
      selectedIndex: null
    };
  },
  computed: {
    answers() {
      let answers = [...this.question.incorrect_answers];
      answers.push(this.question.correct_answer);
      return answers;
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
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
</style>
