<template>
    <div>
        <img :src="profile.photo">
        <br>
        {{ profile.name }}
        <br>

        <template v-for="choice in getChoices()">
            <div
                    @click="handleClick(choice.name)"
                    :class="choiceClasses(choice)"
                    :key="choice.name"
            >
                {{ choice.name }}
            </div>

        </template>

        <br>
        <template v-if="answer !== undefined">
            <button @click="handleNext">Next</button>
        </template>
    </div>
</template>

<script>
    import _ from 'underscore';

    function getDefaultData() {
        return {
            answer: undefined,
            correctAnswer: undefined,
            choices: undefined,
        }
    }

    export default {
        name: 'QuizItem',
        props: ['profile', 'all_profiles',],
        data: getDefaultData,
        methods: {
            handleClick: function (name) {
                if (this.answer !== undefined) {
                    return;
                }
                this.answer = name;
                this.correctAnswer = name === this.profile.name;
                this.$emit('answered', this.correctAnswer);
            },
            handleNext: function () {
                this.$emit('finished', this.correctAnswer);
            },
            getChoices: function () {
                // TODO: wtf getChoices is called after clicking?
                if (this.choices) {
                    return this.choices;
                }
                console.log('choices!');
                const name = this.profile.name;
                const allOther = _.filter(this.all_profiles, p => p.name !== name);

                const sample = _.sample(allOther, 4);
                sample.push(this.profile);
                this.choices = _.shuffle(sample);
                return this.choices;
            },
            choiceClasses: function (choice) {
                let classes = ['choice'];
                const answer = this.$data.answer;

                if (answer) {
                    if (choice.name == answer) {
                        if (choice.name === this.$props.profile.name) {
                            classes.push('correct');
                        } else {
                            classes.push('incorrect');
                        }
                    }
                }
                return classes;
            },
        },
        watch: {
            profile: function () {
                this.answer = undefined;
                this.answered = undefined;
                this.choices = undefined;
                // this.$data = getDefaultData();
            }
        }
    }
</script>

<style scoped lang="scss">
    img {
        border-radius: 5%;
        box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.33);
    }

    .choice {
        background: deepskyblue;
        display: inline-block;
        padding: 0.6rem;
        margin: 0.5rem;
        border-radius: 5px;
    }

    .correct {
        font-weight: bold;
        background: limegreen;
    }

    .incorrect {
        background: red;
        font-weight: bold;
    }

    ul {
        list-style: none;
    }
</style>
