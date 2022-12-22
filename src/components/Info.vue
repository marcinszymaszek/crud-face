<template>
  <div>
    <p>
      Before using:
      <br />
      1. Go to <a href="https://crudcrud.com/" target="blank"> crudcrud.com</a
      ><br />
      2. Copy the endpoint<br />
      3. Paste it
    </p>
    <div class="endpoint-box">
      <label for="endpoint">Endpoint</label>
      <input
        :value="endpoint"
        type="text"
        @input="$emit('paste-endpoint', $event.target.value)"
        id="endpoint"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "Info",
  props: ["endpoint"],
};
</script>

<style lang="scss">
@import "../styles/mixins";

.endpoint-box {
  display: flex;
  flex-direction: row;
  align-items: center;
  label {
    padding-right: 0.5rem;
  }
  input {
    flex-basis: 90%;
    height: 2.5rem;
    border: solid 1.5px rgb(148, 148, 148);
    border-radius: 5px;
    @include breakpoint(medium) {
      flex-basis: 55%;
    }
    @include breakpoint(large) {
      flex-basis: 25%;
    }
  }
}
</style>