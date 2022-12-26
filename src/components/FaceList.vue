<template>
  <div>
    <ul class="faces">
      <li
        v-for="(face) in allFaces"
        :key="face._id"
        class="face"
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

<style scoped lang="scss">
@import "../styles/_variables.scss";
@import "../styles/mixins.scss";

.faces {
  padding: 0 0 1rem 0;
  list-style: none;
  @include breakpoint(medium) {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: 2rem;
  }
  @include breakpoint(large) {
    gap: 1rem;
    justify-content: flex-start;
  }
  .face {
    margin-bottom: 1rem;
    @include breakpoint(medium) {
      margin-bottom: 0;
      flex: 0 1 calc(50% - 1em);
    }
    @include breakpoint(large) {
      flex: 0 1 calc(25% - 1em);
    }
  }
}
</style>