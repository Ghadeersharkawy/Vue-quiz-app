<template>
    <div id="app">
        <b-container>
            <AppHeader :numCorrect="numCorrect" :numTotal="numTotal" />
            <QuizBox
                v-if="questions.length"
                :currentQuestion="questions[index]"
                :next="next"
                :increment="increment"
            />
        </b-container>
    </div>
</template>

<script>
import AppHeader from './components/AppHeader';
import QuizBox from './components/QuizBox';
export default {
    name: 'app',
    components: {
        AppHeader,
        QuizBox
    },
    data() {
        return {
            questions: [],
            index: 0,
            numCorrect: 0,
            numTotal: 0
        };
    },
    methods: {
        next() {
            this.index++;
        },
        increment(isCorrect) {
            if (isCorrect) {
                this.numCorrect++;
            }
            this.numTotal++;
        }
    },
    mounted: function() {
        fetch(
            'https://opentdb.com/api.php?amount=10&category=32&type=multiple',
            { method: 'get' }
        )
            .then(response => response.json())
            .then(jsonData => {
                this.questions = jsonData.results;
            });
    }
};
</script>

<style></style>
