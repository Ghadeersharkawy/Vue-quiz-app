<template>
    <div class="quizbox">
        <b-jumbotron>
            <template slot="lead">{{ currentQuestion.question }}</template>

            <hr class="my-4" />
            <b-list-group class="pb-4">
                <b-list-group-item
                    v-for="(answer, index) in shuffeledAnswers"
                    :key="index"
                    @click="selectedAnswer(index)"
                    :class="[
                        !answered && selectedIndex === index
                            ? 'selected'
                            : answered && correctIndex === index
                            ? 'correct'
                            : answered &&
                              selectedIndex == index &&
                              correctIndex !== index
                            ? 'wrong'
                            : ''
                    ]"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>
            <b-button variant="primary" class="mr-3" @click="next()"
                >Next</b-button
            >
            <b-button
                variant="success"
                :disabled="selectedIndex === null || answered"
                @click="submit()"
            >
                Submit
            </b-button>
        </b-jumbotron>
    </div>
</template>
<script>
import _ from 'lodash';
export default {
    name: 'QuizBox',
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data() {
        return {
            selectedIndex: null,
            shuffeledAnswers: [],
            correctIndex: null,
            answered: false
        };
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null;
                this.answered = false;
                this.shuffelAnswers();
            }
        }

        // currentQuestion() {
        //     this.selectedIndex = null;
        //     this.shuffelAnswers();
        // }
    },
    computed: {
        //replaced with shuffleAnswers method to get and shuffle answers

        getAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers];
            answers.push(this.currentQuestion.correct_answer);
            return answers;
        }
    },
    methods: {
        selectedAnswer(index) {
            this.selectedIndex = index;
        },
        shuffelAnswers() {
            let answers = [
                ...this.currentQuestion.incorrect_answers,
                this.currentQuestion.correct_answer
            ];
            this.shuffeledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffeledAnswers.indexOf(
                this.currentQuestion.correct_answer
            );
        },
        submit() {
            let isCorrect = false;
            this.answered = true;
            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true;
            }
            this.increment(isCorrect);
            //this.next();
        }
    }
};
</script>
<style lang="scss" scoped>
.list-group-item {
    background-color: rgba(255, 255, 255, 0.19);
    &:hover {
        background-color: lightblue;
    }
}
.correct {
    background-color: lightgreen;
}
.wrong {
    background-color: red;
}
.selected {
    border: 2px solid #007bff;
}
</style>
