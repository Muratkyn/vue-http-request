<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading..</p>
      <p v-if="error">{{ error }}</p>
      <p v-if="!isLoading && !error && results.length <= 0">No data found..</p>
      <ul v-else-if="!isLoading && results && results.length > 0">
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error: null,
    };
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      try {
        fetch('https://vuejs-15be7-default-rtdb.firebaseio.com/surveys.json')
          .then((res) => {
            if (res.ok) {
              console.log('Success!');
              return res.json();
            } else {
              this.error = 'Something went wrong';
            }
          })
          .then((data) => {
            this.isLoading = false;
            const results = [];
            for (const id in data) {
              results.push({
                id: id,
                name: data[id].name,
                rating: data[id].rating,
              });
            }
            this.results = results;
          });
      } catch (error) {
        this.error = 'Something went wrong!';
        console.timeLog('An error occurred!');
      }
    },
  },
  mounted() {
    this.loadExperiences();
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
