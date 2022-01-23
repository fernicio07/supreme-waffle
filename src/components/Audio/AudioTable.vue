<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mb-5" cols="12">
        <h2 class="headline font-weight-bold mb-3">Audio Server</h2>
      </v-col>

      <v-col cols="6" class="text-right">
        <v-text-field
          label="Search"
          v-model="searchQuery"
          prepend-inner-icon="mdi-magnify"
        />
      </v-col>

      <v-col cols="4" class="text-right">
        <v-select
          :items="items"
          label="Duration"
          clearable
          dense
          outlined
          v-model="duration"
          item-text="duration"
          item-value="duration"
        />
      </v-col>

      <v-col cols="2" class="text-right">
        <v-btn color="#00BB7C">
          <label for="file">Upload a File</label>
          <input type="file" id="file" style="display: none" />
        </v-btn>
      </v-col>

      <v-col cols="12">
        <table>
          <thead>
            <tr>
              <th>File Name</th>
              <th>Duration</th>
              <th>Size</th>
              <th class="actions">Actions</th>
            </tr>
          </thead>

          <tbody>
            <AudioRow
              v-for="item in filteredItems"
              :item="item"
              :key="item.bd"
              @emitTranscribe="transcribe"
            />
          </tbody>
        </table>
      </v-col>

      <AudioTranscribed
        v-for="(itemObj, i) in allItems"
        :itemObj="itemObj.item"
        :itemModel="itemObj.model"
        :key="i"
      />
    </v-row>
  </v-container>
</template>

<script>
import AudioRow from "./AudioRow";
import AudioTranscribed from "./AudioTranscribed.vue";

export default {
  name: "AudioTable",

  components: {
    AudioRow,
    AudioTranscribed,
  },

  data() {
    return {
      searchQuery: "",
      duration: "",
      presentItem: null,
      previousItem: null,
      allItems: [],
      items: [
        {
          name: "Bueller-Life-moves-pretty-fast.wav",
          duration: "0:00:19",
          size: "3.1MB",
          url: "https://static.deepgram.com/examples/Bueller-Life-moves-pretty-fast.wav",
        },
        {
          name: "nasa-spacewalk-interview.wav",
          duration: "0:00:26",
          size: "2.3MB",
          url: "https://static.deepgram.com/examples/nasa-spacewalk-interview.wav",
        },
        {
          name: "deep-learning-podcast-clip.wav",
          duration: "0:00:28",
          size: "2.5MB",
          url: "https://static.deepgram.com/examples/deep-learning-podcast-clip.wav",
        },
      ],
    };
  },

  computed: {
    filteredItems() {
      if (this.searchQuery || this.duration) {
        return this.items
          .filter((item) => {
            return item.name.toLowerCase().includes(this.searchQuery);
          })
          .filter((duration) => duration.duration >= this.duration);
      } else {
        return this.items;
      }
    },
  },

  methods: {
    transcribe(item, model) {
      if (
        this.allItems.length !== 0 &&
        this.allItems[0].item.name === item.name
      ) {
        let record = this.allItems.find((el) => el.model === model);
        if (!record) {
          this.allItems.push({
            item,
            model,
          });
        }
      } else {
        this.allItems = [];
        this.allItems.push({
          item,
          model,
        });
      }

      this.presentItem = item;

      if (this.presentItem !== this.previousItem) {
        this.show = false;
        this.showMeeting = false;
        this.showConver = false;
      }
      this.filename = item.name;
      this.previousItem = item;
      this.presentItem = null;
    },
  },
};
</script>

<style lang="scss" scoped>
.v-application .headline {
  font-family: "Cairo", sans-serif !important;
}

.v-text-field {
  padding-top: 0;
}

table {
  width: 100%;
  text-align: left;

  & th,
  td {
    border-bottom: thin solid rgba(0, 0, 0, 0.12);
    height: 48px;
    padding: 0 16px;
  }

  & .actions {
    text-align: right;
  }
}

.theme--light.v-btn {
  color: #fff;
}

span {
  color: #0072ff;
  cursor: pointer;
  font-weight: 600;

  &:hover {
    color: #4c9cff;
  }
}
</style>
