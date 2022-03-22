<template>
  <nuxt-link :to="`/task/${task._id}`" >
    <div
      class="w-100 border border-gray-400 shadow-lg px-4 py-2 my-2 rounded"
      draggable="true"
      @dragstart="startDrag($event, task)"
    >
      {{ task.name }}
    </div>
  </nuxt-link>
</template>

<script>
export default {
  name: "Card",
  props: {
    task: Object,
  },
  methods: {
    startDrag(evt, task) {
      evt.dataTransfer.dropEffect = "move";
      evt.dataTransfer.effectAllowed = "move";
      evt.dataTransfer.setData("itemID", task._id);
    },
  },
};
</script>

<style scoped></style>
