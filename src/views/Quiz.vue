<template>
  <div class="about">
    <h1>Face Quiz</h1>
    <template v-if="position < sampleSize">
      <div>
        <ProgressBar :progress="progress" />
      </div>
      <div>
        {{ guessedCount }} out of {{ sampleSize }} guessed
      </div>
      <QuizItem
          :profile="currentProfile"
          :all_profiles="profiles"
          @finished="onFinished"
          @answered="onAnswered"
      />
    </template>

    <template v-else>
      Congratulations!<br>
      You guessed {{ guessedCount }} out of {{ sampleSize }} profiles!<br>
      <button @click="restart">Try again</button>
    </template>
  </div>
</template>

<script>
  import _ from 'underscore';
  import QuizItem from '@/components/QuizItem.vue'
  import ProgressBar from '@/components/ProgressBar.vue'

  export default {
    name: 'Quiz',
    components: {
      QuizItem,
      ProgressBar,
    },
    data: function () {
      return {
        position: 0,
        sampleSize: 5,
        guessedCount: 0,
        profiles: [{
          name: 'Ivan Ivanov',
          photo: 'https://i.pravatar.cc/150?img=1',
        }, {
          name: 'Petr Petrov',
          photo: 'https://i.pravatar.cc/150?img=2',
        }, {
          name: 'Alexander Alexandrov',
          photo: 'https://i.pravatar.cc/150?img=3',
        }, {
          name: 'Vasiliy Vasiliev',
          photo: 'https://i.pravatar.cc/150?img=4',
        }, {
          name: 'Roman Romanoff',
          photo: 'https://i.pravatar.cc/150?img=5',
        }]
      }
    },
    computed: {
      randomSample: function () {
        return _.sample(this.profiles, this.sampleSize)
      },
      currentProfile: function() {
        return this.randomSample[this.position];
      },
      progress: function() {
        console.log(`progress++, pos: ${this.position}`);
        const progress = Math.round(this.position / this.sampleSize * 100);
        console.log(`New value: ${progress}`);
        return progress;
      }
    },
    methods: {
      onAnswered: function(correctAnswer) {
        if (correctAnswer) {
          this.guessedCount++;
        }
      },
      onFinished: function() {
        this.position++;
      },
      restart: function() {
        this.position = 0;
      },
    }
  }
</script>
