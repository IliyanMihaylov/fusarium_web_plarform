<template>
  <v-container class="grey lighten-5">
    <v-form v-on:submit.prevent="search" ref="form" lazy-validation>
      <v-autocomplete
        label="Search For Fusarium Taxonomy"
        :items="fusariums_list"
        v-model="query"
        filled
        rounded
        outlined
        shaped
      >Search For Fusarium Taxonomy</v-autocomplete>

      <v-btn color="success" class="mr-4" @click="search">Search</v-btn>
      <v-btn color="error" class="mr-4" @click="reset">Clear</v-btn>
    </v-form>
    <p />
    <div v-if="searching">
      <i>Searching...</i>
    </div>
  </v-container>
</template>

<script>
import axios from "axios";
import { bus } from "../main.js";

export default {
  data: () => ({
    query: "",
    results: [],
    components: [],
    errors: [],
    selectedLandscapes: [],
    checkbox: false,
    searching: false,
    fusariums_list: []
  }),
  methods: {
    reset() {
      this.$refs.form.reset();
      bus.$emit("search_reset");
    },

    search() {
      this.searching = true;
      axios
        .get(
          `https://www.ebi.ac.uk/proteins/api/taxonomy/name/${encodeURIComponent(
            this.query
          )}`
        )
        .then(response => {
          this.searching = false;
          this.results = response.data;
          bus.$emit("search_result", response.data);
        })
        .catch(e => {
          this.searching = false;
          this.errors.push(e);
          bus.$emit("search_reset");
        });
    }
  },
  created() {
    axios
      .get("/fusarium_list.json")
      .then(response => {
        this.fusariums_list = response.data.fusariums;
      })
      .catch(e => {
        this.errors.push(e);
      });

    bus.$on("filterSelection", data => {
      this.selectedLandscapes = data;
    });
  }
};
</script>