<template>
   <div class="d-flex flex-wrap gap-4 justify-content-center">
    <div 
      v-for="result in results" 
      :key="result.id" 
      class="card text-success p-3 shadow rounded" 
      style="width: 12rem;"
    >
      <h5 class="card-title fs-6 text-center">{{ result.title }}</h5>
      <img :src="result.image" class="card-img" style="max-width: 100%; height: auto;" />
    </div>

    <p v-if="error" class="bg-danger">{{ error }}</p>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      results: null,
      loading: false,
      error: "",
    };
  },
  methods: {
    getFood() {
      this.loading = true;
      this.results = null;
      this.error = "";
      fetch(`https://api.spoonacular.com/recipes/complexSearch?apiKey=${import.meta.env.VITE_API_KEY}`)
        .then(resp => {
          if (!resp.ok) {
            throw new Error("Failed to fetch data from the API");
          }
          return resp.json();
        })
        .then(data => {
          console.log(data)
          this.results = data.results
        })
        .catch((err) => {
          this.error = err.message;
          console.error(err); 
        })
        .finally(() => {
          this.loading = false;
        });
    },
  }, 
  mounted() {    
    this.getFood();
  },
};
</script>

<style>

body {
  background: rgb(233, 228, 228);
}

</style>