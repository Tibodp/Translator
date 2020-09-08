<template>
  <div id="translateForm">
    <form class="form-inline" v-on:submit="formSubmit">
      <!-- textarea to write the text that you want to translate -->
      <textarea
        rows="10"
        class="field space"
        type="text"
        v-model="textToTranslate"
        placeholder="Enter a word or text"
      />
      <!-- speaker icon -->
      <img
        class="speaker_place"
        v-on:click="say()"
        src="../assets/speaker.png"
      />
      <br />
      <!-- audio for text to speech -->
      <audio src hidden></audio>
      <br />
      <!-- select box for source language -->
      <div class="form-group">
        <label>From:</label>
        <select class="space form-control" v-model="sLanguage">
          <option value="nl">Dutch</option>
          <option value="fr">French</option>
          <option value="es">Spanish</option>
          <option value="en">English</option>
          <option value="ja">Japanese</option>
          <option value="ru">Russian</option>
          <option value="de">German</option>
        </select>
      </div>

      <!-- selectbox for tager language -->
      <div class="form-group">
        <label>To:</label>
        <select class="space form-control" v-model="tLanguage">
          <option value="fr">French</option>
          <option value="nl">Dutch</option>
          <option value="es">Spanish</option>
          <option value="en">English</option>
          <option value="ja">Japanese</option>
          <option value="ru">Russian</option>
          <option value="de">German</option>
        </select>
      </div>
      <br />
      <br />
      <!-- choose file for .txt files -->
      <label>Drop your .txt file here</label>
      <input
        class="space form-control"
        type="file"
        ref="myFile"
        @change="selectedFile"
      />
      <br />
      <!-- translate button -->
      <input class="btn btn-primary" type="submit" value="Translate" />
    </form>
  </div>
</template>

<script>
import $ from "jquery";
export default {
  name: "translateForm",
  data() {
    return {
      textToTranslate: "",
      tLanguage: "",
      sLanguage: "",
    };
  },
  // Set default languages in the select box
  created() {
    this.sLanguage = "nl";
    this.tLanguage = "fr";
  },
  // Method for submitting the form
  methods: {
    formSubmit(e) {
      this.$emit(
        "formSubmit",
        this.textToTranslate,
        this.sLanguage,
        this.tLanguage
      );
      e.preventDefault();
    },
    // Method for selecting a file

    selectedFile() {
      console.log("selected a file");
      console.log(this.$refs.myFile.files[0]);

      let file = this.$refs.myFile.files[0];
      if (!file || file.type !== "text/plain") return;

      let reader = new FileReader();
      reader.readAsText(file, "UTF-8");
      reader.onload = (evt) => {
        this.textToTranslate = evt.target.result;
        console.log(this.textToTranslate);
      };
      reader.onerror = (evt) => {
        console.error(evt);
      };
    },
    // Method for speaking out the text or word
    say() {
      var text = this.textToTranslate;
      text = encodeURIComponent(text);
      console.log(text);
      var url =
        "https://api.voicerss.org/?key=db22b01750484775848ef2d9b1f291c2&hl=en-us&src=" +
        text;
      $("audio")
        .attr("src", url)
        .get(0)
        .play();
    },
  },
};
</script>

<style>
.field {
  width: 30%;
}
.space {
  margin: 1%;
}
.speaker_place {
  margin-top: -25%;
}
</style>
