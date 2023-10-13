<template>
  <div>
    <v-list>
      <v-list-item v-for="(item, i) in items" :key="i">
        <v-list-item-action>
          <img max-height="56" max-width="54" src="../static/images/logo.png" />
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title class="logo-text-style"
            >{{ item.title
            }}<span class="shantell-text">{{ item.endtitle }}</span></v-list-item-title
          >
          <v-list-item-subtitle class="shantell-text"
            >JETKIZY ДОСТАВКА</v-list-item-subtitle
          >
        </v-list-item-content>
        <v-icon class="icon" @click="hiddenMenu" title="Закрыть меню">mdi-close</v-icon>
      </v-list-item>
    </v-list>
    <v-list-group
      v-for="(item, id) in categoryArrayData"
      :key="item.id"
      append-icon="mdi-chevron-right"
      color="green"
    >
      <template v-slot:activator>
        <v-list-item-content @click="toggleSecondDrawer(id)">
          <v-list-item-title>{{ item.name }}</v-list-item-title>
        </v-list-item-content>
      </template>
    </v-list-group>
  </div>
</template>

<script>
  export default {
    props: {
      categoryArrayData: {
        type: Array,
      },
    },

    data() {
      return {
        items: [
          {
            title: "AYANMARKET",
            endtitle: ".kz",
          },
        ],
      };
    },
    methods: {
      toggleSecondDrawer(id) {
        this.$emit("toggle-second-drawer", id);
      },
      hiddenMenu() {
        this.$emit("hidden-menu");
      },
    },
  };
</script>
