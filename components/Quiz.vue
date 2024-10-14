<template>
  <div class="quiz-container">
    <div v-if="!quizStarted">
      <h1>Satisfaction Survey</h1>
      <p>Please answer 5 questions about your experience.</p>
      <button @click="startQuiz">Start Survey</button>
    </div>
    <div v-else-if="currentQuestionIndex < questions.length">
      <h1>Satisfaction Survey</h1>
      <h2>Question {{ currentQuestionIndex + 1 }} of {{ questions.length }}</h2>
      <p>{{ questions[currentQuestionIndex].question }}</p>
      <div class="rating">
        <button 
          v-for="rating in 5" 
          :key="rating" 
          @click="submitAnswer(rating)"
          :class="{ selected: answers[currentQuestionIndex] === rating }"
        >
          {{ rating }}
        </button>
      </div>
      <p class="rating-label">1 - Very Unsatisfied, 5 - Very Satisfied</p>
    </div>
    <div v-else>
      <h2>Survey Completed!</h2>
      <div class="summary">
        <h3>Summary of Your Responses:</h3>
        <ul>
          <li v-for="(question, index) in questions" :key="index">
            <strong>{{ question.question }}</strong>: {{ answers[index] }}
          </li>
        </ul>
        <p>Average Rating: {{ averageRating.toFixed(1) }}</p>
      </div>
      <button @click="restartQuiz">Take Survey Again</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const questions = [
  { question: "How satisfied are you with our product?" },
  { question: "How would you rate our customer service?" },
  { question: "How likely are you to recommend us to others?" },
  { question: "How satisfied are you with the value for money of our product/service?" },
  { question: "How would you rate your overall experience with our company?" }
];

const quizStarted = ref(false);
const currentQuestionIndex = ref(0);
const answers = ref([]);

const startQuiz = () => {
  quizStarted.value = true;
  currentQuestionIndex.value = 0;
  answers.value = [];
};

const submitAnswer = (rating) => {
  answers.value[currentQuestionIndex.value] = rating;
  currentQuestionIndex.value++;
};

const restartQuiz = () => {
  startQuiz();
};

const averageRating = computed(() => {
  if (answers.value.length === 0) return 0;
  const sum = answers.value.reduce((acc, val) => acc + val, 0);
  return sum / answers.value.length;
});
</script>

<style scoped>
.quiz-container {
  background-color: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  width: 100%;
  text-align: center;
}

h1, h2, h3 {
  color: #333;
  margin-bottom: 1rem;
}

p {
  margin-bottom: 1rem;
}

.rating {
  display: flex;
  justify-content: center;
  margin-bottom: 1rem;
}

.rating button {
  width: 40px;
  height: 40px;
  margin: 0 0.5rem;
  font-size: 1rem;
  border: 1px solid #007bff;
  background-color: white;
  color: #007bff;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s;
}

.rating button:hover, .rating button.selected {
  background-color: #007bff;
  color: white;
}

.rating-label {
  font-size: 0.8rem;
  color: #666;
}

button {
  display: inline-block;
  width: auto;
  min-width: 200px;
  padding: 0.75rem 1.5rem;
  margin: 0.5rem 0;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
  font-size: 1rem;
}

button:hover {
  background-color: #0056b3;
}

.summary {
  text-align: left;
  margin-bottom: 1rem;
}

.summary ul {
  list-style-type: none;
  padding: 0;
}

.summary li {
  margin-bottom: 0.5rem;
}
</style>