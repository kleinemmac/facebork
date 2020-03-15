<template>
  <v-container fluid>
    <v-dialog v-model="wrongTypeDialog" width="250">
      <v-card>
        <v-card-title class="headline grey lighten-2" primary-title>Upload Error</v-card-title>
        <v-card-text class="text-center pt-2">
          Wrong file type.
          <br />Please upload an image.
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="wrongTypeDialog = false">Aight</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="notDogDialog" width="250">
      <v-card>
        <v-card-title class="headline grey lighten-2" primary-title>What even is this?</v-card-title>
        <v-card-text class="text-center pt-2">
          Hooman. You had one job.
          <br />Please upload an image of a DOG.
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="notDogDialog = false">Aight</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="modelErrorDialog" width="250">
      <v-card>
        <v-card-title class="headline grey lighten-2" primary-title>Communication Error</v-card-title>
        <v-card-text class="text-center pt-2">
          It looks like we are having trouble connecting to the server.
          <br />Please try again later.
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="modelErrorDialog = false">Aight</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-row>
      <v-col>
        <p class="title text-center">{{ instructionText }}</p>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col class="text-center" cols="12" lg="6">
        <input id="file-upload" type="file" hidden @change="e => uploadFile(e, 'selected')" />
        <div
          v-if="file === null"
          v-cloak
          @drop.prevent="e => uploadFile(e, 'dropped')"
          @dragover.prevent
        >
          <div id="drop-area">
            <v-row align="center" class="max-height">
              <v-col>
                <v-btn fab color="green" @click="chooseFile">
                  <v-icon color="white">mdi-dog</v-icon>
                </v-btn>
                <p class="pt-4">
                  Drop an image here
                  <br />OR
                  <br />Click to browse files
                </p>
              </v-col>
            </v-row>
          </div>
        </div>
        <img id="image" v-if="file !== null" :src="url" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import ml5 from 'ml5';

export default {
  data() {
    return {
      file: null,
      url: "",
      wrongTypeDialog: false,
      notDogDialog: false,
      modelErrorDialog: false,
      dogRatings: [
        "17/10, heckin' great pupper!",
        "27/10 shmackos, great pupper!",
        "15/10, would doggo again!",
        "I give 17 milkbones and 3 long walkies, great pupper!"
      ],
      instructionText: "Upload a picture of your dog for a rating."
    };
  },
  methods: {
    uploadFile(e, method) {
      let file;
      if (method === "dropped") {
        file = e.dataTransfer.files[0];
      } else {
        file = e.target.files[0];
      }
      if (!file || !file.type.includes("image")) {
        this.wrongTypeDialog = true;
        return;
      }
      this.file = file;
      this.url = URL.createObjectURL(this.file);
      const classifier = ml5.objectDetector("cocossd");
      classifier.then(res => {
        res.detect(document.getElementById("image"), (err, results) => {
          let dogExists = results.filter(item => {
            return item.label === "dog";
          });
          if (dogExists.length === 0) {
            this.notDogDialog = true;
            this.file = null;
          } else {
            this.instructionText = this.dogRatings[Math.floor(Math.random() * this.dogRatings.length)];
          }
        });
      }).catch(err => {
        this.modelErrorDialog = true;
        this.file = null;
      });

    },
    chooseFile() {
      document.getElementById("file-upload").click();
    }
  }
};
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
