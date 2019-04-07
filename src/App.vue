<template>
  <div id="app">
    <Blog/>
  </div>
</template>

<script>
import Blog from "./pages/Blog.vue";

export default {
  name: "app",
  components: {
    Blog
  }
};
</script>

<style lang="scss">
@import "normalize-scss";
@include normalize();
@font-face {
  font-family: "Stag Book";
  src: url("./assets/fonts/stag-book.otf") format("opentype");
}
@font-face {
  font-family: "Stag Medium";
  src: url("./assets/fonts/stag-medium.otf") format("opentype");
}
@font-face {
  font-family: "Stag Semibold";
  src: url("./assets/fonts/stag-semibold.otf") format("opentype");
}

@font-face {
  font-family: "Stag Book Italic";
  src: url("./assets/fonts/stag-bookitalic.otf") format("opentype");
}

body {
  background-color: #f2f2f2;
}

ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
}
a {
  text-decoration: none;
}

h1 {
  font-weight: normal;
}

#app {
  font-family: "Stag Book", Helvetica, Arial, sans-serif;
  text-align: center;
  color: #454545;
  font-size: 17px;
}

.container {
  padding-left: 130px;
  padding-right: 130px;
  max-width: 940px;
  margin: 0 auto;
}

.btn {
  font-family: "Stag Medium";
  width: 140px;
  height: 45px;
  text-align: center;
  background-color: #f1c40f;
  border-radius: 4px;
  border: none;
  transition: all 0.1s ease-in-out;
  cursor: pointer;
  font-size: 17px;
  color: #454545;
  &:hover {
    background-color: #e2bb1f;
  }
}
</style>
