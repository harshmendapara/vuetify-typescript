<template>
  <v-container>
    <v-layout text-center wrap justify-center>
      <v-flex xs8>
        <v-autocomplete
          v-model="model"
          :items="items"
          :loading="isLoading"
          :search-input.sync="search"
          clearable
          hide-details
          hide-selected
          item-text="name"
          item-value="symbol"
          label="Search for a coin..."
        >
          <template v-slot:no-data>
            <v-list-item>
              <v-list-item-title>
                Search for <strong>Cryptocurrency</strong>
              </v-list-item-title>
            </v-list-item>
          </template>
          <template v-slot:progress>
            <v-progress-circular
              indeterminate
              color="primary"
            ></v-progress-circular>
          </template>
          <template v-slot:selection="{ attr, on, item, selected }">
            <v-chip
              v-bind="attr"
              :input-value="selected"
              color="blue-grey"
              class="white--text"
              v-on="on"
            >
              <v-icon left>
                mdi-bitcoin
              </v-icon>
              <span v-text="item.name"></span>
            </v-chip>
          </template>
          <template v-slot:item="{ item }">
            <v-list-item-avatar
              color="indigo"
              class="text-h5 font-weight-light white--text"
            >
              {{ item.name.charAt(0) }}
            </v-list-item-avatar>
            <v-list-item-content>
              <v-list-item-title v-text="item.name"></v-list-item-title>
              <v-list-item-subtitle v-text="item.symbol"></v-list-item-subtitle>
            </v-list-item-content>
            <v-list-item-action>
              <v-icon>mdi-bitcoin</v-icon>
            </v-list-item-action>
          </template>
        </v-autocomplete>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    isLoading: false,
    items: [],
    model: null,
    search: null,
  }),

  watch: {
    search() {
      this.searchData();
    },
  },
  methods: {
    searchData() {
      let timer = this.searchData.timer;
      if (timer) {
        clearTimeout(timer);
      }
      this.searchData.timer = setTimeout(() => {
        this.isLoading = true;
        fetch("https://api.coingecko.com/api/v3/coins/list")
          .then((res) => res.clone().json())
          .then((res) => {
            this.items = res;
          })
          .finally(() => (this.isLoading = false));
      }, 800);
    },
  },
};
</script>
