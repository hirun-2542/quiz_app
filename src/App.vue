<script setup>
import { ref, computed } from "vue";

const questions = ref([
    {
        id: 1,
        question: "1. What is 2 + 2?",
        answers: [
            { text: "4", correct: true, selected: null },
            { text: "22", correct: false, selected: null },
            { text: "10", correct: false, selected: null },
            { text: "5", correct: false, selected: null },
        ],
    },
    {
        id: 2,
        question: "2. What is 3 * 3?",
        answers: [
            { text: "33", correct: false, selected: null },
            { text: "9", correct: true, selected: null },
            { text: "6", correct: false, selected: null },
            { text: "0", correct: false, selected: null },
        ],
    },
    {
        id: 3,
        question: "3. What is 4 - 4?",
        answers: [
            { text: "44", correct: false, selected: null },
            { text: "8", correct: false, selected: null },
            { text: "0", correct: true, selected: null },
            { text: "16", correct: false, selected: null },
        ],
    },
    {
        id: 4,
        question: "4. What is 5 / 5?",
        answers: [
            { text: "25", correct: false, selected: null },
            { text: "55", correct: false, selected: null },
            { text: "10", correct: false, selected: null },
            { text: "1", correct: true, selected: null },
        ],
    },
]);

const currentQuestion = ref(0);
const randomQuestion = computed(() => {
    let question = questions.value[currentQuestion.value];
    question.index = currentQuestion.value;
    return question;
});

const scores = computed(() => {
    let score = 0;
    questions.value.forEach((question) => {
        question.answers.forEach((answer) => {
            if (answer.correct && answer.selected) {
                score++;
            }
        });
    });
    return score;
});

const showQuestion = ref(false);

const NextQuestion = () => {
    if (currentQuestion.value < questions.value.length - 1) {
        currentQuestion.value++;
    }
};

const PreviousQuestion = () => {
    if (currentQuestion.value > 0) {
        currentQuestion.value--;
    }
};

const showScore = ref(false);
const selecctAnswer = (answer, index) => {
    questions.value[currentQuestion.value].answers.forEach((ans) => {
        ans.selected = null;
    });
    if (answer.correct) {
        questions.value[currentQuestion.value].answers[index].selected = true;
    } else {
        questions.value[currentQuestion.value].answers[index].selected = false;
    }
};

const Restart = () => {
    currentQuestion.value = 0;
    showScore.value = false;
    showQuestion.value = false;
    questions.value.forEach((question) => {
        question.answers.forEach((answer) => {
            answer.selected = null;
        });
    });
};
</script>

<template>
    <div class="container">
        <h1>Quiz App</h1>
        <div v-if="!showQuestion" class="start">
            <button @click="showQuestion = !showQuestion">Start</button>
        </div>
        <div v-else-if="showScore" class="score">
            <h2>Your Score is {{ scores }} / {{ questions.length }}</h2>
            <button @click="Restart">Restart</button>
        </div>
        <div v-else-if="showQuestion" class="quiz-container">
            <div class="question">
                <div class="header">
                    <h2>{{ randomQuestion.question }}</h2>
                    <button @click="showScore = !showScore">Submit</button>
                </div>

                <div class="btn-grid">
                    <button
                        v-for="(ans, index) in randomQuestion.answers"
                        :key="index"
                        @click="selecctAnswer(ans, index)"
                        class="btn"
                        :style="{
                            background: ans.selected !== null ? '#12def5' : '',
                        }"
                    >
                        {{ ans.text }}
                    </button>
                </div>
            </div>
            <div class="controls">
                <button
                    :style="{
                        visibility: currentQuestion != 0 ? 'visible' : 'hidden',
                    }"
                    class="btn-start"
                    @click="PreviousQuestion"
                >
                    Previous
                </button>
                <button
                    class="btn-end"
                    @click="NextQuestion"
                    :style="{
                        visibility:
                            currentQuestion !== questions.length - 1
                                ? 'visible'
                                : 'hidden',
                    }"
                >
                    Next
                </button>
            </div>
        </div>
    </div>
</template>

<style scoped>
*,
*::before,
*::after {
    box-sizing: border-box;
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    font-weight: normal;
}

.container {
    display: block;
    justify-content: center;
    align-items: center;
    margin: 0;
    padding: 0;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    max-width: 80%;
    width: 800px;
    background-color: antiquewhite;
    border-radius: 5px;
    box-shadow: 0 0 10px 2px;
    padding: 10px;
}

.score {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    font-size: 2rem;
    height: 100%;
}
.header {
    display: flex;
    justify-content: space-between;
    margin-right: 10px;
    align-items: center;
}
.btn-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
    margin: 20px 10px;
}

.btn-grid button {
    padding: 10px;
    border: none;
    border-radius: 10px;
    background-color: #f1f1f1;
    cursor: pointer;
    transition: all 0.3s ease-in-out;
}

.controls {
    display: flex;
    justify-content: space-between;
    margin: 20px 10px;
}

.btn-start {
    justify-content: start;
}

.btn-end {
    justify-content: end;
}

button {
    cursor: pointer;
    font-size: 2rem;
    border: 1px solid white;
    outline: none;
}

button:hover {
    background-color: #b3b2b2aa;
}

.start {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
}
.start button {
    font-size: 4rem;
    padding: 0 20px;
}
</style>
