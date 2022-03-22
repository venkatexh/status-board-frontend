<template>
  <div class="flex flex-row flex-wrap w-screen">
    <Status v-for="item in statuses" :key="item._id" :status="item" :cards="cards" />
    <div class="text-4xl opacity-60 my-2 mx-4 h-4 cursor-pointer" v-on:click="setNewStatus" v-if="!this.newStatusBox">+</div>
    <div v-if="this.newStatusBox" class="w-100 border border-gray-400 shadow-lg px-4 py-2 my-2 rounded h-32">
      <input type="text" placeholder="enter status name" class="mx-auto outline-none w-full border-b-2 border-zinc-400 mb-2" @change="handleInputChange"/>
      <button
        class="py-2 px-4 bg-black w-full text-white mx-auto mt-2 rounded"
        v-on:click="handleStatusCreation"
      >
        Create
      </button>
    </div>
  </div>
</template>

<script>

export default {
  name: "Dashboard",
  data() {
    return {
      statuses: [],
      cards: [],
      newStatusBox: false,
      newStatusName: '',
      newStatusColor: ''
    };
  },
  methods: {
    setNewStatus() {
      this.newStatusBox = true;
    },
    handleInputChange(e) {
      this.newStatusName = e.target.value;
    },
    async handleStatusCreation() {
      const status = {
        name: this.newStatusName,
        color: this.newStatusColor
      }
      await this.$axios.post('/status',status).then (res => {
        this.statuses.push(res.data);
        this.newStatusBox = false;
      })
    }
  },
  async mounted() {
    this.statuses = await this.$axios.get("/status").then(
      (res) => res.data
    );
    this.cards = await this.$axios.get("/tasks/all").then((res) =>
      res.data
    );
  },
};
</script>

<style scoped></style>
