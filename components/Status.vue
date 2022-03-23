<template>
  <div class="w-96 mx-4 my-4">
    <div class="flex">
      <div class="bg-gray-400 text-white px-2 rounded-lg">{{ status.name }}</div>
      <div class="mx-2">
        {{ cards.filter((card) => card.status === status.name).length }}
      </div>
    </div>
    <div
      class="h-72 my-4"
      @drop="onDrop($event, status.name)"
      @dragover.prevent
      @dragenter.prevent
    >
      <Card
        v-for="item in cards"
        v-if="item.status === status.name"
        :key="item._id"
        :task="item"
      />
      <div class="opacity-60 cursor-pointer" v-on:click="newTask(status._id)">
        + New
      </div>
      <div
        v-if="newTaskBox"
        class="border border-gray-400 flex flex-col shadow-lg py-4 px-1 rounded"
      >
        <input
          type="text"
          placeholder="enter task name"
          class="mx-auto outline-none w-4/5 border-b-2 border-zinc-400 mb-2"
          @change="handleInputChange"
        />
        <textarea
          cols="2"
          placeholder="enter description"
          class="mx-auto outline-none w-4/5 border-b-2 border-zinc-400 mb-2"
          @change="handleDescChange"
        />
        <button
          class="py-2 px-4 bg-black w-4/5 text-white mx-auto mt-2 rounded"
          v-on:click="createNewTask"
        >
          Create
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Status",
  data() {
    return {
      newTaskBox: false,
      newTaskName: "",
      newTaskDesc: "",
    };
  },
  props: {
    status: Object,
    cards: Array,
  },
  methods: {
    async onDrop(evt, status) {
      const itemID = evt.dataTransfer.getData("itemID");
      const item = this.cards.find((item) => item._id === itemID);
      item.status = status;
      await this.$axios.put(`/tasks?taskId=${item._id}`, {
        status: `${status}`,
      });
    },
    newTask(id) {
      if (id === this.status._id) {
        this.newTaskBox = true;
      }
    },
    handleInputChange(e) {
      this.newTaskName = e.target.value;
    },
    handleDescChange(e) {
      this.newTaskDesc = e.target.value;
    },
    async createNewTask() {
      const newTask = {
        name: this.newTaskName,
        desc: this.newTaskDesc,
        status: this.status.name,
      };
      await this.$axios.post(`/tasks`, newTask).then((res) => {
        this.cards.push(res.data);
      });
      this.newTaskBox = false;
    },
  },
};
</script>

<style scoped></style>
