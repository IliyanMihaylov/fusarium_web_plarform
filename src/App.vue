<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app>
      <v-list dense>
        <v-list-item @click.stop="left = !left">
          <v-list-item-action>
            <v-icon>mdi-exit-to-app</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Help</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <leftMenu />
    </v-navigation-drawer>

    <v-app-bar app color="indigo" dark>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title>Fusarium Taxonomy</v-toolbar-title>
    </v-app-bar>

    <v-content>
      <v-container class="fill-width, fill-height" fluid>
        <v-row justify="center" align="center">
          <v-col class="text-center">
            <search />
          </v-col>
        </v-row>
        <resultList />
      </v-container>
    </v-content>

    <v-footer app color="blue-grey" class="white--text">
      <span>Fusarium Taxonomy</span>
      <v-spacer />
      <span>&copy; 2019</span>
    </v-footer>
  </v-app>
</template>

<script>
import leftMenu from "./components/leftMenu.vue";
import search from "./components/search.vue";
import resultList from "./components/resultList.vue";

export default {
  components: {
    leftMenu,
    search,
    resultList
  },
  props: {
    source: String
  },
  data() {
    return {
      drawer: false,
      drawerRight: true,
      right: false,
      left: true
    };
  }
};
</script>