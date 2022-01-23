<template>
  <tr>
    <td>{{ item.name }}</td>
    <td>{{ item.duration }}</td>
    <td>{{ item.size }}</td>
    <td class="actions">
      <v-menu transition="slide-y-transition" bottom>
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            class="mr-3 blue--text text--darken-2 font-weight-bold"
            color="white"
            dark
            v-bind="attrs"
            v-on="on"
          >
            {{ shownItem }}
            <v-icon class="ml-2">mdi-chevron-down</v-icon>
          </v-btn>
        </template>
        <v-list>
          <v-list-item
            v-for="(dItem, i) in dropItems"
            :key="i"
            @click="clickDropItem(dItem.model)"
          >
            <v-list-item-title
              class="blue--text text--darken-2 font-weight-bold"
              >{{ dItem.item }}</v-list-item-title
            >
          </v-list-item>
        </v-list>
      </v-menu>
      <span small @click="download(item)">Download</span>
    </td>
  </tr>
</template>

<script>
export default {
  props: ["item"],
  data() {
    return {
      showItem: "",
      dropItems: [
        {
          item: "General Model",
          model: "general",
        },
        {
          item: "Meeting Model",
          model: "meeting",
        },
        {
          item: "Conversational Model",
          model: "conversationalai",
        },
      ],
    };
  },
  computed: {
    shownItem() {
      if (this.showItem === "") {
        return this.dropItems[0].item;
      } else {
        return this.showItem;
      }
    },
  },
  methods: {
    clickDropItem(model) {
      if (model === "general") {
        this.showItem = this.dropItems[0].item;
      } else if (model === "meeting") {
        this.showItem = this.dropItems[1].item;
      } else {
        this.showItem = this.dropItems[2].item;
      }

      this.$emit("emitTranscribe", this.item, model);
    },
  },
};
</script>

<style lang="scss" scoped>
th,
td {
  border-bottom: thin solid rgba(0, 0, 0, 0.12);
  height: 48px;
  padding: 0 16px;
}

.actions {
  text-align: right;
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
