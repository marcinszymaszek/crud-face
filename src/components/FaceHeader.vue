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

<style lang="scss">
@import "../styles/_mixins.scss";
@import "../styles/_variables.scss";

h1 {
  color: $color-lagoon;
  text-align: center;
  font-size: 50px;
}

.box {
  @include center-container;
  &--header {
    margin-bottom: 2rem;
  }
}

.btn {
  @include basic-button;
  &--add {
    background: $color-secondary;
  }
}
</style>