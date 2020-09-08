<template>
  <div class="text-center" id="app">
    <h1 class="tilt">Word translator</h1>

    <TranslateForm v-on:formSubmit="translateText"></TranslateForm>
    <h1>Translation</h1>

    <TranslateOutput v-text="translatedText"></TranslateOutput>
    <img class="speaker_place" v-on:click="say()" src="./assets/speaker.png" />
  </div>
</template>

<script>
import TranslateForm from "./components/TranslateForm";
import TranslateOutput from "./components/TranslateOutput";
import $ from "jquery";

export default {
  name: "App",
  components: {
    TranslateForm,
    TranslateOutput,
  },
  data: function() {
    return {
      translatedText: "",
    };
  },
  methods: {
    // method for translating the text
    translateText: function(text, sLanguage, tLanguage) {
      this.translatedText = "";
      this.$http
        .get(
          "https://translate.googleapis.com/translate_a/single?client=gtx&sl=" +
            sLanguage +
            "&tl=" +
            tLanguage +
            "&dt=t&q=" +
            encodeURI(text)
        )
        .then((response) => {
          for (let i = 0; i < response.body[0].length; i++) {
            this.translatedText += response.body[0][i][0];
          }
        });
    },
    // method for text to speech of translated text
    say() {
      var text = this.translatedText;
      text = encodeURIComponent(text);
      console.log(text);
      var url =
        "https://api.voicerss.org/?key=db22b01750484775848ef2d9b1f291c2&hl=fr-fr&src=" +
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
body {
  background: #356cb5;
}
.tilt {
  font-size: 70px;
  font-family: "Lucida Sans Unicode", "Lucida Grande", sans-serif;
  margin: 3%;
}
.speaker_place {
  margin-left: 1%;
}
</style>
