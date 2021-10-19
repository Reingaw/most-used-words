<template>
  <v-container fluid>
      <v-form>
        <v-file-input
            label="Choose the subtitles"
            outlined
            prepend-icon="mdi-message-text"
            append-outer-icon="mdi-send"
            multiple
            chips
            v-model="files"
            @click:append-outer="processSubtitles"
        />
      </v-form>
      <div class="pills">
          <Pill v-for="word in groupedWords" :key="word.name"
            :name="word.name" :amount="word.amount" />
      </div>
  </v-container>
</template>

<script>
import Pill from "./Pill.vue"

export default {
    components: { Pill },
    data: function() {
        return {
            files: [],
            groupedWords: []
        }
    },
    methods: {
        processSubtitles() {
            const paths = this.files.map(f => f.path);
            window.ipcRenderer.send('process-subtitles', paths)
            window.ipcRenderer.receive('process-subtitles', (res)=> {
                this.groupedWords = res;
            })
        }
    }
}
</script>

<style>
    .pills {
        display: flex;
        flex-wrap: wrap;
    }

</style>