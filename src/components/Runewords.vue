<template>
  <div>
    <div class="rw-Search flex items-center mb-8">
      <label class="text-gold whitespace-nowrap mr-4">{{ "Search" }}</label>
      <input v-model="searchText" type="text" class="rw-Search-input" @input="onSearchInput" />
    </div>

    <div>
      <runewords-table :items="runewordsList" />
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";

import RunewordsTable from "@/components/RunewordsTable.vue";

import runewordsData from "@/data/runewords";
// import store from "@/store";

export default defineComponent({
  name: "Runewords",

  components: {
    RunewordsTable,
  },

  data() {
    return {
      isHelpVisible: false,

      /** @type {RunewordItem[]} */
      runewordsList: [],

      searchText: "",
    };
  },

  created() {
    this.runewordsList = /** @type{RunewordItem[]}*/ (runewordsData.slice());
    this.updateFilter(this.searchText);
  },

  methods: {
    onSearchInput() {
      this.updateFilter(this.searchText);
    },


    /** @param {string} text */
    updateFilter(text) {
      const searchTerm = text.toLowerCase();

      /** @param {RunewordItem} item */
      const matches = (item) => {
        const matchesTitle = item.title.toLowerCase().includes(searchTerm);
        const matchesType = item.ttypes.some((type) => {
          return type.toLowerCase().includes(searchTerm);
        });

        return searchTerm === "" || matchesTitle || matchesType;
      };

      this.runewordsList.forEach((item) => {
        item.filterMatch = matches(item);
      });
    },
  },
});
</script>
