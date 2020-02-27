<template>
  <div class="home">
    <!-- Categories -->
    <ApolloQuery :query="categoriesQuery" :variables="{ name }">
      <template v-slot="{ result: { loading, data }, isLoading }">
        <div v-if="isLoading" class="loading apollo">Loading...</div>
        <div v-else>
          <a href="#" @click.prevent="selectCategory('all')" class="link-margin">
            All
          </a>
          <a href="#" @click.prevent="selectCategory('feature')" class="link-margin">
            Featured
          </a>
          <a
            href="#"
            @click.prevent="selectCategory(category.id)"
            class="link-margin"
            v-for="category of data.categories"
            :key="category.id"
          >
            {{ category.title }}
          </a>
        </div>
      </template>
    </ApolloQuery>
    <div v-if="selectedCategory === 'all'">
      <ApolloQuery :query="query" :variables="{ name }">
        <template v-slot="{ result: { loading, data }, isLoading }">
          <div v-if="isLoading" class="loading apollo">Loading...</div>
          <div v-else>
            <div v-for="book of data.books" :key="book.id">
              {{ book.title }}
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>
    <div v-if="selectedCategory === 'feature'">
      <ApolloQuery :query="query" :variables="{ feature: true }">
        <template v-slot="{ result: { loading, data }, isLoading }">
          <div v-if="isLoading" class="loading apollo">Loading...</div>
          <div v-else>
            <div v-for="book of data.featuredByBooks" :key="book.id">
              {{ book.title }}
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>
    <div v-else>
      <ApolloQuery :query="query" :variables="{ id: selectedCategory }">
        <template v-slot="{ result: { loading, data }, isLoading }">
          <div v-if="isLoading" class="loading apollo">Loading...</div>
          <div v-else>
            <div
              v-for="book of data.category.books"
              :key="book.id"
              class="margin-top"
            >
              {{ book.id }} - {{ book.title }}
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>
  </div>
</template>

<script>
import categoryQuery from "@/graphql/queries/Category.gql";
import categoriesQuery from "@/graphql/queries/Categories.gql";
import booksQuery from "@/graphql/queries/Books.gql";
import booksFeaturedQuery from "@/graphql/queries/BooksFeatured.gql";
export default {
  name: "Home",
  data() {
    return {
      booksFeaturedQuery,
      categoryQuery,
      categoriesQuery,
      selectedCategory: "all",
      query: booksQuery
    };
  },
  methods: {
    selectCategory(cat) {
      if (cat === "all") {
        this.query = booksQuery;
      } else if (cat === "feature") {
        this.query = booksFeaturedQuery;
      } else {
        this.query = categoryQuery;
      }
      this.selectedCategory = cat;
    }
  }
};
</script>
<style lang="css" scoped>
.link-margin {
  margin-right: 24px;
}
.margin-top {
  margin-top: 24px;
}
</style>
