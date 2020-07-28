<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="6" xl="6">
        <v-row align="center" justify="center">
          <v-col cols="12">
            <v-text-field v-model="url" label="URL" @keyup="getData"></v-text-field>
          </v-col>
          <v-col cols="8">
            <v-text-field v-model="query" @keyup="getData" label="Query"></v-text-field>
          </v-col>
          <v-col cols="4">
            <v-btn color="primary" block @click="clear">Bersihkan</v-btn>
          </v-col>
          <v-col v-if="query !== '' && count !== 0" cols="12">
            <v-btn
              color="primary"
              class="mr-2"
              @click="move(detail.first)"
              :disabled="detail.page === 1"
            >First</v-btn>
            <v-btn
              color="primary"
              class="mr-2"
              @click="move(detail.next)"
              :disabled="detail.next === null"
            >Next</v-btn>
            <v-btn
              color="primary"
              class="mr-2"
              @click="move(detail.prev)"
              :disabled="detail.prev === null"
            >Previous</v-btn>
            <v-btn
              color="primary"
              class="mr-2"
              @click="move(detail.last)"
              :disabled="detail.next === null"
            >Last</v-btn>
            <span>Halaman: {{ detail.page }}</span>
          </v-col>
        </v-row>
      </v-col>
      <v-col cols="12" md="6" xl="6">
        <v-list>
          <v-list-item v-for="list in lists" :key="list.text">
            <v-list-item-content>
              <v-list-item-subtitle v-html="list.text"></v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "search-form",
  data() {
    return {
      url: "https://www.youtube.com/watch?v=klnvttPfOUM",
      query: "",
      lists: [],
      detail: "",
    };
  },
  computed: {
    api() {
      return `https://cari-teks-video-api.vercel.app/api/search?url=${this.url}&q=${this.query}`;
    },
    count() {
      if (this.lists.details) {
        return 0;
      } else {
        return this.lists.length;
      }
    },
  },
  methods: {
    getData() {
      this.$http.get(this.api).then((response) => {
        this.lists = response.data.data;
        this.detail = response.data;
      });
    },
    move(url) {
      this.$http.get(url).then((response) => {
        this.lists = response.data.data;
        this.detail = response.data;
      });
    },
    clear() {
      this.lists = [];
      this.detail = "";
      this.query = "";
    },
  },
};
</script>