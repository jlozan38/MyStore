<template>
  <v-container fluid fill-height class="bgimage">
    <v-layout justify-center align-center column pa-5>
      <div class="display-4 font-weight-black white--text text-xs-center">
        Welcome to
      </div>
      <div class="display-4 font-weight-black white--text text-xs-center">
        HungryGhost Records
      </div>
      <v-btn x-large color="blue darken-4" dark to="/Home">
          Enter
      </v-btn>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  name: "Home"
};
</script>

<style lang="scss" scoped>
.bgimage {
  background: url(https://images.unsplash.com/photo-1525522819526-d571040e1607?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80);
  background-size: "cover";
  width: 100%;
  height: 100%;
}
</style>
