<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" :width="450" fixed app>
      <NavMenuComponents
        :category-array-data="categoryData"
        @toggle-second-drawer="toggleSecondDrawer"
        @hidden-menu="hiddenMenu"
      />
    </v-navigation-drawer>
    <!-- Содержимое второй боковой панели -->
    <v-card
      elevation="0"
      class="mx-auto"
      :width="450"
      v-model="secondDrawer"
      v-if="showCard"
    >
      <v-card-text>
        <v-list>
          <v-list-group
            v-for="(child, childId) in categoryData[selectedItemId].children"
            :key="childId"
            append-icon="mdi-chevron-right"
            color="green"
            v-if="
              categoryData[selectedItemId] &&
              Array.isArray(categoryData[selectedItemId].children)
            "
          >
            <template v-slot:activator>
              <v-list-item-content @click="toggleSecondDrawer(childId)">
                <v-list-item-title>{{ child.name }}</v-list-item-title>
              </v-list-item-content>
            </template>
            <CategoryList :categories="categoryData" />
          </v-list-group>
        </v-list>
      </v-card-text>
    </v-card>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-btn
        elevation="2"
        rounded
        large
        color="green"
        class="white--text"
        @click.stop="drawer = !drawer"
      >
        КАТАЛОГ
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>

    <v-footer dark padless :absolute="!fixed" app>
      <v-card-text class="py-2 white--text text-center"
        >&copy; {{ new Date().getFullYear() }}</v-card-text
      >
    </v-footer>
  </v-app>
</template>

<script>
  import axios from "axios";
  import NavMenuComponents from "../components/NavMenuComponents.vue";
  import CategoryList from "../components/CategoryList.vue";

  export default {
    components: {
      NavMenuComponents,
      CategoryList,
    },

    name: "DefaultLayout",
    data() {
      return {
        clipped: false,
        drawer: false,
        fixed: false,
        categoryData: [],
        grandChildrens: [],
        secondDrawer: false,
        selectedItemId: null,
        showCard: false,
      };
    },

    async fetch() {
      try {
        const response = await axios.get("/category_response.json");
        if (response.status === 200) {
          this.categoryData = response.data;
          // Обход категорий
          this.categoryData.forEach((category) => {
            if (Array.isArray(category.children)) {
              // Обход дочерних элементов каждой категории
              category.children.forEach((child) => {
                if (Array.isArray(child.children)) {
                  // child.children доступен для каждого дочернего элемента
                  this.grandChildrens.push(...child.children);
                }
              });
            }
          });
          //console.log(this.grandChildrens);
        } else {
          console.error("Error fetch data");
        }
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },

    methods: {
      toggleSecondDrawer(id) {
        this.selectedItemId = id;
        this.secondDrawer = !this.secondDrawer;
        this.showCard = true;
      },
      hiddenMenu() {
        this.showCard = false;
        this.drawer = !this.drawer;
      },
      expandCard() {
        this.secondDrawer = true;
      },
      collapseCard() {
        this.secondDrawer = false;
      },
    },
  };
</script>

<style scoped>
  .v-application .mx-auto {
    margin-right: auto !important;
    margin-left: 450px !important;
    z-index: 9;
  }
</style>
