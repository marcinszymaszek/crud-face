<template>
  <h2 class="confirmation">
    Congratulations!<br />
    Enjoy the app :)
  </h2>
</template>

<script>
export default {
  name: "Confirmation",
};
</script>

<style scoped lang="scss">
@import "../styles/_mixins.scss";
@import "../styles/_variables.scss";

.confirmation {
  @include center-text;
  @include text-secondary;
  font-size: 30px;
  color: $color-lagoon;
  font-weight: 400;
  margin-top: 7px;
  @include breakpoint(medium) {
    margin-top: 100px;
  }
}
</style>