<template>
  <v-container fluid>
    <v-form>
      <v-file-input
        labe="Selecione as legendas"
        prepend-icon="mdi-message-text"
        outlined
        append-outer-icon="mdi-send"
        multiple
        chips
        v-model="files"
        @click:append-outer="processSubtitles"
      />
    </v-form>
    <div class="pills">
      <Pill v-for="word in groupeWords" :key="word.name" :name="word.name" :amount="word.amount" />
    </div>
  </v-container>
</template>

<script>
import { ipcRenderer } from "electron";
import Pill from "./Pill";

export default {
  components: {
    Pill
  },
  data: function() {
    return {
      files: [],
      groupeWords: []
    };
  },
  methods: {
    processSubtitles() {
      const paths = this.files.map(x => x.path);
      ipcRenderer.send("process-subtitles", paths);
      ipcRenderer.on("process-subtitles", (event, resp) => {
        this.groupeWords = resp;
      });
    }
  }
};
</script>

<style>
.pills {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
</style>