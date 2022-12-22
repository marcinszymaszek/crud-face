<template>
  <div class="box box--header">
    <h1>CrudFace</h1>
    <button @click="$emit('open-modal', true)" class="btn btn--add">
      Add Face
    </button>
  </div>
</template>

<script>
export default {
  name: "FaceHeader",
};
</script>

<style scoped lang="scss">
@import "../styles/_mixins.scss";
@import "../styles/_variables.scss";

h1 {
  color: $color-lagoon;
  text-align: center;
  font-size: 50px;
  margin-bottom: 1rem;
}

.box {
  @include center-container;
  &--header {
    margin-bottom: 2rem;
    @include breakpoint(medium) {
      align-items: start;
    }
  }
}

.btn {
  @include basic-button;
  &--add {
    background: $color-secondary;
    @include breakpoint(medium) {
      width: 9rem;
    }
    @include breakpoint(large) {
      width: 10rem;
    }
  }
}
</style>