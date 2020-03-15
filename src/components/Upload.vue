<template>
  <v-container fluid>
    <v-row>
      <v-col>
        <p class="title text-center">Upload a picture of your dog for a rating.</p>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col class="text-center" cols="12" lg="6">
        <input id="file-upload" type="file" hidden @change="selectFile">
        <div v-if="file === null" v-cloak @drop.prevent="dropFile" @dragover.prevent>
          <div id="drop-area">
            <v-row align="center" class="max-height">
              <v-col>
                <v-btn fab color="green" @click="chooseFile">
                  <v-icon color="white">mdi-dog</v-icon>
                </v-btn>
                <p class="pt-4">
                  Drop an image here
                  <br>OR
                  <br>Click to browse files
                </p>
              </v-col>
            </v-row>
          </div>
        </div>
        <img v-if="file !== null" :src="url">
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      file: null,
      url: '',
    };
  },
  methods: {
    dropFile(e) {
      const droppedFile = e.dataTransfer.files[0];
      if (!droppedFile) return;
      this.file = droppedFile;
      this.url = URL.createObjectURL(this.file);
    },
    selectFile(e) {
      const selectedFile = e.target.files[0];
      if (!selectedFile) return;
      this.file = selectedFile;
      this.url = URL.createObjectURL(this.file);
    },
    chooseFile() {
      document.getElementById('file-upload').click();
    }
  }
}
</script>

<style lang="scss" scoped>
#drop-area {
  width: 100%;
  height: 50vh;
  border: 3px dashed #bdbdbd;
  border-radius: 10px;
  p {
    color: #9e9e9e;
  }
}
</style>
