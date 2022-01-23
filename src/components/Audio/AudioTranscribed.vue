<template>
  <v-col cols="12">
    <h5 class="headline font-weight-bold mb-3">{{ title }}</h5>
    <v-textarea
      v-if="loading"
      outlined
      :label="filename"
      value="Transcribing..."
    />

    <v-textarea v-else outlined :label="filename" :value="transcript" />
  </v-col>
</template>

<script>
export default {
  props: ["itemObj", "itemModel"],
  data() {
    return {
      loading: true,
      transcript: "",
    };
  },
  computed: {
    title() {
      if (this.itemModel === "general") {
        return "General Model";
      } else if (this.itemModel === "meeting") {
        return "Meeting Model";
      } else {
        return "Conversational Model";
      }
    },
    filename() {
      return this.itemObj.name;
    },
  },
  methods: {
    async requestModel(item, model) {
      const requestOptions = {
        method: "POST",
        headers: {
          "content-type": "application/json",
          authorization: "Token 5782fd4fdd3a9a0e30b6dfb81f3a60986943d653",
        },
        body: JSON.stringify({
          url: item.url,
        }),
      };
      const response = await fetch(
        "https://api.deepgram.com/v1/listen?model=" + model,
        requestOptions
      );
      const data = await response.json();
      this.loading = false;
      this.transcript = data.results.channels[0].alternatives[0].transcript;
    },
  },
  mounted() {
    this.requestModel(this.itemObj, this.itemModel);
  },
};
</script>

<style></style>
