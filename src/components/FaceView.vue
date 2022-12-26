<template>
  <div class="box face-view">
    <canvas width="200" height="200" :id="face._id"> </canvas>
    <div class="btn-container">
      <button class="btn btn--edit" @click="editFace(face)">Edit</button>
      <button class="btn btn--delete" @click="deleteFace(face._id)">
        Delete
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Isomer from "isomer";

export default {
  name: "FaceView",
  props: ["face", "endpoint"],
  mounted() {
    const brown = [2, 1, 0];
    const pink = [206, 74, 74];

    const iso = new Isomer(document.getElementById(this.face._id));

    const Shape = Isomer.Shape;
    const Point = Isomer.Point;
    const Path = Isomer.Path;
    const Color = Isomer.Color;

    const skinColor = new Color(...this.face.skinColor);
    const hairColor = new Color(...this.face.hairColor);
    const eyeColor = new Color(...brown);
    const mouthColor = new Color(...pink);

    //SKIN
    iso.add(Shape.Prism(new Point(0, 0, 0)), skinColor);
    //HAIR
    const cube = Shape.Prism(Point(0, 0, 1));
    iso.add(
      cube.scale(Point.ORIGIN, 1, 1, 0.4).translate(0, 0, 0.5),
      hairColor
    );
    //EYES
    iso.add(
      new Path([
        Point(0.7, 0.5, 0),
        Point(0.8, 0.5, 0),
        Point(1, 0.7, 0),
        Point(0.9, 0.7, 0),
      ]),
      eyeColor
    );
    iso.add(
      new Path([
        Point(1.2, 0.5, 0),
        Point(1.3, 0.5, 0),
        Point(1.5, 0.7, 0),
        Point(1.4, 0.7, 0),
      ]),
      eyeColor
    );
    //MOUTH
    iso.add(
      new Path([
        Point(0.7, 0.3, 0),
        Point(1, 0.3, 0),
        Point(1.1, 0.4, 0),
        Point(0.8, 0.4, 0),
      ]),
      mouthColor
    );
  },
  methods: {
    deleteFace(faceId) {
      axios.delete(`${this.endpoint}/faces/${faceId}`);
      this.$emit("delete-face", true);
    },
    editFace(face) {
      this.$emit("edit-face", face);
      this.$emit("open-modal", true);
    },
  },
};
</script>

<style scoped lang="scss">
@import "../styles/_mixins.scss";
@import "../styles/variables.scss";

.box {
  @include center-container;
  border-radius: 7px;
  background: white;
  box-shadow: -7px 7px 24px -19px rgba(150, 150, 150, 1);
  &.face-view {
    align-items: center;
  }
  padding: 1rem;
  &--header {
    margin-bottom: 2rem;
  }
  .btn {
    width: 100%;
    &--edit {
      @include basic-button(inverted,$color-blue,$color-blue-dark);
      margin: 1rem 0rem;
    }
    &--delete {
      @include basic-button(inverted,$color-red,$color-red-dark);
      margin-bottom: 1rem;
    }
    @include breakpoint(medium) {
      &--edit,
      &--delete {
        max-width: 9rem;
      }
    }
    @include breakpoint(large) {
      &--edit,
      &--delete {
        max-width: 10rem;
      }
    }
  }
  .btn-container {
    width: 100%;
    @include breakpoint(medium) {
      display: flex;
      gap: 10px;
      align-items: baseline;
      justify-content: center;
    }
    @include breakpoint(medium) {
      gap: 15px;
    }
  }
}
</style>