<template>
  <div>
    <div class="task"
      draggable
      @dragstart="pickUpTask($event, taskIndex, columnIndex)"
      @click="goToTask(task)"
      @drop.stop="moveTaskOrColumn($event, column.tasks, columnIndex, taskIndex)"
      @dragover.prevent 
      @dragenter.prevent
    >
      <span class="w-full flex-no-shrink font-bold">
        {{ task.name }}
      </span>
      <p v-if="task.description" class="w-full flex-no-shrink mt-1 text-sm">
        {{ task.description }}
      </p>
    </div>
  </div>
</template>

<script>
  import movingTasksAndColumnMixin from '@/mixins/movingTasksAndColumnMixin'

  export default {
    mixins: [movingTasksAndColumnMixin],
    props: {
      task: {
        type: Object,
        required: true
      },
      taskIndex: {
        type: Number,
        required: true
      },
    },
    methods: {
      pickUpTask (e, taskIndex, fromColumnIndex) {
        e.dataTransfer.effectAllowed = 'move'
        e.dataTransfer.dropEffect = 'move'

        e.dataTransfer.setData('from-task-index', taskIndex)
        e.dataTransfer.setData('from-column-index', fromColumnIndex)
        e.dataTransfer.setData('type', 'task')
      },
      goToTask (task) {
        this.$router.push({ name: 'task', params: { id: task.id } })
      }
    }
  }
</script>

<style lang="css">

.task {
  @apply flex items-center flex-wrap shadow mb-2 py-2 px-2 rounded bg-white text-grey-darkest no-underline;
  cursor: pointer;
}

</style>