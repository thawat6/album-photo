<template>
  <div>
    <v-row>
      <v-col cols="12">
        <v-autocomplete
          v-model="selectAlbum"
          clearable
          :items="albums"
          item-text="title"
          item-value="id"
          label="Album Title"
          @change="selectedAlbum()"
        ></v-autocomplete>
      </v-col>
    </v-row>
    <Photo :items="photos" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectAlbum: "",
      albums: [],
      photos: [],
      page: 1,
      perpage: 5,
      search: "",
      albumId: null,
    };
  },
  mounted() {
    this.$axios
      .$get(`https://jsonplaceholder.typicode.com/albums`)
      .then((res) => {
        this.albums = res;
      });
    this.$axios
      .$get(`https://jsonplaceholder.typicode.com/photos`)
      .then((res) => {
        this.photos = res;
      });
  },
  methods: {
    selectedAlbum() {
      let albumId = ``;
      if (this.selectAlbum) {
        albumId = `?albumId=${this.selectAlbum}`;
      }
      this.$axios
        .$get(`https://jsonplaceholder.typicode.com/photos${albumId}`)
        .then((res) => {
          this.photos = res;
        });
    },
  },
};
</script>

<style>
</style>