<template>
    <div class="quizbox">
        <b-jumbotron>
            <template slot="lead">{{ currentQuestion.question }}</template>

            <hr class="my-4" />
            <b-list-group class="pb-4">
                <b-list-group-item
                    v-for="(answer, index) in getAnswers"
                    :key="index"
                    @click="selectedAnswer(index)"
                    :class="selectedIndex === index ? 'selected' : ''"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>
            <b-button variant="primary" class="mr-3" @click="next()"
                >Next</b-button
            >
            <b-button variant="success" @click="submit(selectedIndex)"
                >Submit</b-button
            >
        </b-jumbotron>
    </div>
</template>
<script>
import _ from 'lodash'
export default {
    name: 'QuizBox',
    props: {
        currentQuestion: Object,
        next: Function
    },
    data() {
        return {
            selectedIndex: null,
            shuffeledAnswers: []
        };
    },
    watch: {
        currentQuestion() {
            this.selectedIndex = null;
            this.shuffelAnswers();
        }
    },
    computed: {
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
            this.shuffeledAnswers = _.shuffel(answers);
        },
        submit(selectedIndex) {
            if (
                this.getAnswers[selectedIndex] ===
                this.currentQuestion.correct_answer
            ) {
                console.log('yaaaay Correct!!!');
                this.next();
            }
        }
    }
};
</script>
<style lang="scss" scoped>
.list-group-item {
    background-color: rgba(255, 255, 255, 0.18823529411764706);
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
    border: 2px solid blue;
}
</style>
