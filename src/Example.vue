<script setup>
import { ref } from "vue";
import api from "./services/api";

const packages = ref([]);
const favorites = ref({});

Promise.all([api.get("packages"), api.get("favorites")]).then(
  ([{ data: pkgs }, { data: userFavorites }]) => {
    packages.value = pkgs;

    favorites.value = userFavorites.reduce((favorites, record) => {
      favorites[record.package_id] = true;
      return favorites;
    }, {});
  },
);

function setFavorite(id) {
  if (favorites.value[id]) {
    api.delete(`favorites?package_id=eq.${id}`).then(() => {
      delete favorites.value[id];
    });
  } else {
    api
      .post("favorites", {
        user_id: 1,
        package_id: id,
      })
      .then(() => {
        favorites.value[id] = true;
      });
  }
}
</script>

<template>
  <div>
    <div class="header">
      <img src="/catercow-logo.svg" class="logo" />
      <h4>Package Results</h4>
    </div>

    <div class="packages">
      <div v-for="pkg in packages" :key="pkg.id" class="package">
        <div class="package-image">
          <img :src="pkg.image" :alt="pkg.name" />

          <button type="button" class="package-fav" @click="setFavorite(pkg.id)">
            <template v-if="favorites[pkg.id]">&#128525;</template>
            <template v-else>&#129293;</template>
          </button>
        </div>

        <div class="package-details">
          <p>{{ pkg.name }}</p>
          <small>
            {{ pkg.caterer }}
          </small>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.logo {
  width: 100px;
}

.packages {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
}

.package {
  border: 1px solid grey;
  border-radius: 10px;
  overflow: hidden;

  p {
    margin: 0;
  }

  small {
    color: grey;
    display: block;
    margin-top: 5px;
  }

  .package-image {
    position: relative;
  }

  .package-fav {
    background-color: white;
    position: absolute;
    left: 10px;
    top: 10px;
  }

  .package-details {
    padding: 10px;
  }
}
</style>
