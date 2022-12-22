<template>
  <div>
    <transition name="fade" appear>
      <div class="modal-overlay" @click="closeModal"></div>
    </transition>
    <transition name="pop" appear>
      <div class="modal">
        <div class="modal-content">
          <div class="icon-close" @click="closeModal">
            <span>&times;</span>
          </div>
          <form>
            <h2 for="skin-colors">Skin color</h2>
            <v-select
              :options="skinColors"
              option-text="label"
              option-value="value"
              v-model="formValues.skinColor"
              :clearable="false"
              class="chooser"
              @input="setSkinColor($event.value)"
            >
            </v-select>
            <h2 for="hair-colors">Hair color</h2>
            <v-select
              :options="hairColors"
              option-text="label"
              option-value="value"
              v-model="formValues.hairColor"
              :clearable="false"
              class="chooser"
              @input="setHairColor($event.value)"
            >
            </v-select>
            <input
              type="submit"
              class="btn btn--save"
              value="Save"
              @click="saveFace(editedFace?._id)"
            />
          </form>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import axios from "axios";
import vSelect from "vue-select";
import "vue-select/dist/vue-select.css";

export default {
  name: "FaceForm",
  components: {
    vSelect,
  },
  data() {
    return {
      formValues: {
        skinColor: {
          value: null,
          label: null,
        },
        hairColor: {
          value: null,
          label: null,
        },
      },
      skinColors: [
        {
          value: "255,204,153",
          label: "white",
        },
        {
          value: "148,89,44",
          label: "dark",
        },
      ],
      hairColors: [
        {
          value: "240,234,48",
          label: "blond",
        },
        {
          value: "0,0,0",
          label: "black",
        },
      ],
    };
  },
  props: ["editedFace", "editMode", "endpoint"],
  mounted() {
    if (this.editMode && this.editedFace) {
      this.formValues.skinColor.value = this.editedFace.skinColor;
      this.formValues.skinColor.label =
        this.editedFace.skinColor[0] === 255 ? "white" : "dark";
      this.formValues.hairColor.value = this.editedFace.hairColor;
      this.formValues.hairColor.label =
        this.editedFace.hairColor[0] === 240 ? "blond" : "black";
    }
  },
  methods: {
    closeModal() {
      this.$emit("edit-face", null);
      this.$emit("edit-mode", false);
      this.$emit("close-modal", false);
    },
    saveFace(editedFaceId) {
      if (editedFaceId) {
        axios.put(`${this.endpoint}/faces/${editedFaceId}`, {
          skinColor: this.formValues.skinColor.value,
          hairColor: this.formValues.hairColor.value,
        });
      } else {
        axios.post(`${this.endpoint}/faces`, {
          skinColor: this.formValues.skinColor.value,
          hairColor: this.formValues.hairColor.value,
        });
      }
      this.$emit("save-face", true);
      this.closeModal();
    },
    setSkinColor(value) {
      this.formValues.skinColor.label =
        value === "255,204,153" ? "white" : "dark";
      this.formValues.skinColor.value = this.convertToNumericArray(value);
    },
    setHairColor(value) {
      this.formValues.hairColor.label =
        value === "240,234,48" ? "blond" : "black";
      this.formValues.hairColor.value = this.convertToNumericArray(value);
    },
    convertToNumericArray(str) {
      return str.split(",").map((element) => {
        return Number(element);
      });
    },
  },
};
</script>

<style lang="scss">
@import "../styles/_mixins.scss";
@import "../styles/variables.scss";

.modal {
  position: absolute;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  text-align: center;
  width: 85%;
  height: fit-content;
  z-index: 999;
  transform: none;
  @include breakpoint(medium) {
    width: 60%;
  }
  @include breakpoint(large) {
    width: 30%;
  }
  .modal-content {
    .icon-close {
      cursor: pointer;
      margin-left: auto;
      margin-right: 0;
      width: fit-content;
      font-weight: 300;
      font-size: 30px;
      color: #707070;
    }
    background-color: #fefefe;
    margin: auto;
    padding: 20px;
    border: 1px solid #888;
    border-radius: 10px;
    form {
      @include center-container;
      .chooser {
        width: 100%;
        margin-bottom: 0.5rem;
        .vs__search {
          min-height: 2.5rem;
        }
      }
    }
  }
}

.modal-overlay {
  content: "";
  position: absolute;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 998;
  background: #2c3e50;
  opacity: 0.6;
  cursor: pointer;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s linear;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.pop-enter-active,
.pop-leave-active {
  transition: transform 0.4s cubic-bezier(0.5, 0, 0.5, 1), opacity 0.4s linear;
}

.pop-enter,
.pop-leave-to {
  opacity: 0;
  transform: scale(0.3) translateY(-50%);
}

.btn {
  @include basic-button;
  &--save {
    @include breakpoint(medium) {
      max-width: 9rem;
    }
    @include breakpoint(large) {
      max-width: 10rem;
    }
    margin-bottom: 0.5rem;
    margin-top: 1.5rem;
    background: $color-main;
  }
}

h2 {
  @include text-secondary;
  text-align: left;
  margin: 7px 0;
}
</style>