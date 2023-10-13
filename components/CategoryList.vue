<template>
  <v-list>
    <v-list-group
      v-for="category in categories"
      :key="category.id"
      append-icon="mdi-chevron-right"
    >
      <template v-slot:activator>
        <v-list-item-content @click="toggleCategory(category)">
          <v-list-item-title class="small-text">{{ category.name }}</v-list-item-title>
        </v-list-item-content>
      </template>
      <CategoryList :categories="category.children" />
    </v-list-group>
  </v-list>
</template>

<script>
  import CategoryList from "./CategoryList";

  export default {
    props: {
      categories: {
        type: Array,
      },
    },
    components: {
      CategoryList,
    },
    methods: {
      toggleCategory(category) {
        const selectedCategory = this.findCategoryById(category.id, this.categoryData);
        if (selectedCategory) {
          console.log("Selected Category:", selectedCategory.name);
        } else {
          console.error("Category not found");
        }
      },
      findCategoryById(categoryId, categories) {
        for (const category of categories) {
          if (category.id === categoryId) {
            return category;
          }
          if (category.children && category.children.length > 0) {
            const foundCategory = this.findCategoryById(categoryId, category.children);
            if (foundCategory) {
              return foundCategory;
            }
          }
        }
        return null;
      },
    },
  };
</script>

<style scoped>
  .small-text {
    font-size: 13px !important;
    padding-left: 20px;
    color: var(--color-close-icon);
  }
</style>
