<script setup>
  import Question from "../componets/Question.vue";
  import QuizHeader from "../componets/QuizHeader.vue";
  import Result from "../componets/Result.vue";
  import { useRoute } from "vue-router";
  import { ref, computed } from "vue";
  import quizes from "../data/quizes.json";

  const route = useRoute();
  const quizId = parseInt(route.params.id);
  const quiz = quizes.find((q) => q.id === quizId);
  const currentQuestionIndex = ref(0);
  const numberOfCorrectAnswers = ref(0);
  const showResults = ref(false);

  const questionStatus = computed(
    () => `${currentQuestionIndex.value}/${quiz.questions.length}`
  );
  const barPercentage = computed(
    () => `${(currentQuestionIndex.value / quiz.questions.length) * 100}%`
  );

  const onOptionSelected = (isCorrect) => {
    if (isCorrect) {
      numberOfCorrectAnswers.value++;
    }

    if(quiz.questions.length - 1 === currentQuestionIndex.value){
      showResults.value = true;
    }

    currentQuestionIndex.value++;
  };

</script>

<template>
  <div>
    <QuizHeader
      :questionStatus="questionStatus"
      :barPercentage="barPercentage"
    ></QuizHeader>
    <div>
      <Question
        v-if="!showResults"
        :question="quiz.questions[currentQuestionIndex]"
        @selectOption="onOptionSelected"
      ></Question>
      <Result v-else
        :quizQuestionLength="quiz.questions.length"
        :numberOfCorrectAnswers="numberOfCorrectAnswers"
      ></Result>
    </div>
  </div>
</template>
