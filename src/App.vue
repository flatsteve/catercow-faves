<script setup>
import { ref } from "vue";
import api from "./services/api";

/*
Use composition or options api - just remove "setup" to use options api

api is a wrapper around axios that sets the headers and base url so you only need to define the path and http method
/packages will return array of packages for user
/favorites will return array of the users favorite packages (use package_id in the records) - try to transform to a nicer structure to suit easy lookup 
/favorites accepts standard HTTP methods for adding and removing rows - url = /favorites?package_id=eq.${package_id}
*/
</script>

<template>
  <div>
    <div class="header">
      <img src="/catercow-logo.svg" class="logo" />
      <h4>Package Results</h4>
    </div>

    <div class="packages">
      <!-- List of packages indicating which are favorites -->
      <!-- &#128525; = Love heart eyes emoji-->
      <!-- &#129293; = Grey heart emoji -->
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
  /* Todo */
}

/* Can use or delete */
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
