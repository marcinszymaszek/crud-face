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
    color: $color-secondary;
    padding-right: 0.5rem;
  }
  input {
    flex-basis: 90%;
    height: 2.5rem;
    border: 1px solid rgba(34, 36, 38, 0.15);
    border-radius: 5px;
    &:focus-visible {
      outline: none;
      border-color: $color-blue-light;
    }
    @include breakpoint(medium) {
      flex-basis: 55%;
    }
    @include breakpoint(large) {
      flex-basis: 25%;
    }
  }
}
p {
  color: $color-secondary;
  font-size: 17px;
}

a {
  color: $color-blue-light-2;
  text-decoration: none;
}
</style>