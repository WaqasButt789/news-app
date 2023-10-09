<template>
  <q-page padding>
    <q-input
      v-model="searchTerm"
      label="Search News"
      outlined
      @keyup.enter="searchNews"
    />
    <q-list bordered v-if="articles.length > 0">
      <q-item
        clickable
        v-for="(article, index) in articles"
        :key="index"
        @click="viewArticleDetails(index)"
      >
        <q-item-section>
          <q-item-label :lines="1">{{ article.title }}</q-item-label>
          <q-item-label caption lines="2">{{ article.description }}</q-item-label>
        </q-item-section>
      </q-item>
    </q-list>
    <q-else>
      <q-card class="q-ma-md">
        <q-card-section class="text-h6 text-center">No articles found.</q-card-section>
      </q-card>
    </q-else>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: '',
      articles: [],
    };
  },
  methods: {
    async searchNews() {
      try {
        const apiKey = 'YOUR_API_KEY'; // Replace with your News API key
        const response = await fetch(
          `https://newsapi.org/v2/everything?q=${this.searchTerm}&apiKey=${apiKey}`
        );
        const data = await response.json();
        this.articles = data.articles;
      } catch (error) {
        console.error('Error fetching news:', error);
      }
    },
    viewArticleDetails(index) {
      const selectedArticle = this.articles[index];
      this.$router.push({ name: 'article-details', params: { article: selectedArticle } });
    },
  },
};
</script>

<style scoped>
/* Add your custom styles here */
</style>

