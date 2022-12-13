<template>
  <div>
    <ul class="faces">
      <li
        v-for="(face, index) in allFaces"
        :key="face._id"
        :class="index % 2 !== 0 ? 'odd' : 'even'"
      >
        <slot :face="face"> </slot>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "FaceList",
  props: ["allFaces"],
};
</script>

<style lang="scss">
@import "../styles/_variables.scss";

.faces {
  padding: 0 0;
  list-style: none;
  .odd {
    background: $color-grey-light;
  }
  .even {
    background: $color-grey-dark;
  }
}
</style>