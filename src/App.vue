<template>
  <div id="app">
    <FaceHeader @open-modal="displayModal" />
    <Info
      v-if="!loading && !endpoint"
      @paste-endpoint="setEndpoint"
      :endpoint="endpoint"
    />
    <Confirmation v-if="!allFaces.length && !loading && endpoint" />
    <div v-if="loading" class="loader"></div>
    <template v-if="allFaces.length && !loading">
      <FaceList :key="componentKey" :all-faces="allFaces">
        <template v-slot="{ face }">
          <FaceView
            :endpoint="endpoint"
            :face="face"
            @edit-face="setEditedFace"
            @open-modal="displayModal"
            @delete-face="refreshFaces"
          />
        </template>
      </FaceList>
    </template>
    <FaceForm
      :endpoint="endpoint"
      :edit-mode="editMode"
      :edited-face="editedFace"
      v-if="isModalVisible"
      @save-face="refreshFaces"
      @edit-mode="handleEdit"
      @close-modal="displayModal"
      @edit-face="setEditedFace"
    />
  </div>
</template>

<script>
import FaceHeader from "./components/FaceHeader.vue";
import Info from "./components/Info.vue";
import Confirmation from "./components/Confirmation.vue";
import FaceList from "./components/FaceList.vue";
import FaceView from "./components/FaceView.vue";
import FaceForm from "./components/FaceForm.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    FaceHeader,
    Info,
    Confirmation,
    FaceList,
    FaceView,
    FaceForm,
  },
  data() {
    return {
      loading: false,
      allFaces: [],
      editedFace: {},
      isModalVisible: false,
      editMode: false,
      renderComponent: true,
      componentKey: 0,
      endpoint: "",
    };
  },
  mounted() {
    try {
      this.endpoint = sessionStorage.getItem("endpoint");
    } catch (err) {
      this.handleErrorMessage(err);
    }
    this.refreshFaces();
  },
  methods: {
    displayModal(e) {
      this.isModalVisible = e;
    },
    handleEdit(e) {
      this.editMode = e;
    },
    setEndpoint(e) {
      try {
        sessionStorage.endpoint = e;
      } catch (err) {
        this.handleErrorMessage(err);
      }
      this.endpoint = e;
      this.refreshFaces();
    },
    refreshFaces() {
      this.loading = true;
      setTimeout(() => {
        axios
          .get(`${this.endpoint}/faces`)
          .then((response) => {
            this.allFaces = response.data;
          })
          .then(() => {
            this.componentKey += 1;
          })
          .then((this.loading = false));
      }, 3000);
    },
    setEditedFace(face) {
      this.editMode = typeof face === "object";
      this.editedFace = face;
    },
    handleErrorMessage(err) {
      err.message.includes("not defined") ||
      err.message.includes("Access is denied")
        ? console.log(
            "Sorry, sessionStorage is not supported or have been disabled"
          )
        : console.log(err.message);
    },
  },
};
</script>

<style lang="scss">
@import "../src/styles/variables";
@import "../src/styles/mixins";

#app {
  font-family: Arial, Helvetica, sans-serif;
  padding: 0 1rem;

  //====== FONTS =======
  h1 {
    color: $color-lagoon;
    text-align: center;
    font-size: 50px;
  }

  h2 {
    text-align: left;
    font-size: 20px;
    width: 100%;
    font-weight: 100;
    margin: 7px 0;
  }

  p {
    color: $color-lagoon-dark;
    font-size: 17px;
  }

  a {
    color: #1264a3;
    text-decoration: none;
  }

  //====== LOADER =======
  .loader {
    border: 10px solid #f3f3f3;
    border-top: 10px solid $color-lagoon;
    border-radius: 50%;
    margin-top: 10rem;
    font-size: 25px;
    animation: spin 2s linear infinite;
    width: 40px;
    height: 40px;
    margin-left: auto;
    margin-right: auto;
  }

  @keyframes spin {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
}
</style>
