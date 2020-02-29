<template>
  <v-data-table
    v-on:input="notify"
    v-model="selected"
    :headers="headers"
    :items="landscapes"
    :single-select="singleSelect"
    item-key="name"
    show-select
    class="elevation-1"
  />
</template>

<script>
import axios from "axios";
import { bus } from '../main'

export default {
  data() {
    return {
      singleSelect: false,
      headers: [
        {
          text: "Landscape",
          align: "left",
          sortable: false,
          value: "name"
        }
      ],
      landscapes: []
    };
  },
  mounted() {
    axios
      .get("http://localhost:8080/hosts")
      .then(response => {
        this.landscapes = response.data;
      })
      .catch(error => {
        this.error = error;
        this.errored = true;
      })
      .finally(() => (this.loading = false));
  },
  methods: {
    notify (){
          bus.$emit('filterSelection', this.selected);
      }
  }
};
</script>
