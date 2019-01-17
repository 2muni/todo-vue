<template>
  <div>
    <button :class="{ active: filter == 'all'}" @click="setFilter('all')">All</button>
    <button :class="{ active: filter == 'active'}" @click="setFilter('active')">Active</button>
    <button :class="{ active: filter == 'completed'}" @click="setFilter('completed')">Completed</button>
  </div>
</template>

<script>
export default {
  name: 'todo-filtered',
  props: {
    filter: {
      type: String,
      required: true,
    }
  },
  methods: {
    setFilter(filter) {
      this.$emit('setFilter', filter)
    }
  }
}
</script>

<style lang="scss" scoped>
button {
  font-size: 14px;
  background-color: white;
  appearance: none;
  &:hover {
    background: lightgreen;
  }
  &:focus {
    outline: none;
  }
}
.active {
  background: lightgreen;
}
</style>


