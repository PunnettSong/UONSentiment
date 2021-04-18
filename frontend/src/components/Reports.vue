<template>
  <div :key="report.id" v-for="report in reports">
    <Report
      @toggle-reminder="$emit('toggle-reminder', report.id)"
      @delete-report="$emit('delete-report', report.id)"
      :report="report"
    />
  </div>
</template>

<script>
import Report from './Report'

export default {
  name: 'Reports',
  props: {
    reports: Array,
  },
  components: {
    Report,
  },
  emits: ['delete-report', 'toggle-reminder'],
}
</script>
