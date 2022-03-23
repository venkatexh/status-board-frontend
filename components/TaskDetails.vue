<template>
  <div class="w-screen h-screen flex items-center justify-center">
    <div
      class="border border-gray-400 flex flex-col shadow-lg w-96 h-76 py-4 px-1 rounded"
    >
      <div class="w-4/5 mx-auto flex flex-col">
        <label class="">Name</label>
        <input
          type="text"
          placeholder="enter task name"
          class="mx-auto outline-none w-full border-b-2 border-zinc-400 mb-2 opacity-60"
          :value="this.task.name"
          @change="handleInputChange"
        />
        <label class="">Description</label>
        <textarea
          cols="2"
          placeholder="enter description"
          class="mx-auto outline-none w-full border-b-2 border-zinc-400 mb-2 opacity-60"
          :value="this.task.desc"
          @change="handleDescChange"
        />
        <label class="">Status</label>
        <select
          class="outline-none cursor-pointer h-8"
          @change="handleStatusChange"
        >
          <option :value="this.task.status" selected disabled hidden>{{this.task.status}}</option>
          <option v-for="item in statuses" :key="item._id">{{ item.name }}</option>
        </select>
        <button
          class="py-2 px-4 bg-black w-full text-white mx-auto mt-2 rounded"
          v-on:click="updateTask"
        >
          Update
        </button>
        <button
          class="py-2 px-4 bg-red w-full bg-red-500 text-white mx-auto mt-2 rounded"
          v-on:click="deleteTask"
        >
          Delete
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Task",
  data() {
    return {
      statuses: [],
      task: {},
      taskName: "",
      taskDesc: "",
      taskStatus: "",
    };
  },
  methods: {
    handleInputChange(e) {
      this.taskName = e.target.value;
    },
    handleDescChange(e) {
      this.taskDesc = e.target.value;
    },
    handleStatusChange(e) {
      this.taskStatus = e.target.value;
    },
    async updateTask() {
      const task = {
        name: this.taskName,
        desc: this.taskDesc,
        status: this.taskStatus,
      };
      await this.$axios.put(`/tasks?taskId=${this.task._id}`, task).then(() => {
        this.$router.push("/");
      });
    },
    async deleteTask(url, config) {
      await this.$axios
        .delete(`/tasks?taskId=${this.task._id}`, {
          data: {
            taskId: this.task._id,
          },
        })
        .then(() => {
          this.$router.push("/");
        });
    },
  },
  async mounted() {
    await this.$axios
      .get(`/tasks?taskId=${this.$route.params.id}`)
      .then((res) => {
        this.task = res.data;
        this.taskName = res.data.name;
        this.taskDesc = res.data.desc;
        this.taskStatus = res.data.status;
      });
    this.statuses = await this.$axios.get("/status").then((res) => res.data);
  },
};
</script>

<style scoped></style>
