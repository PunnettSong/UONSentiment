<template>
  <div class="report">
    <h3 @click="$emit()">
      {{ report.name }}
      <i @click="$emit('delete-report', report.id)" class="fas fa-times"></i>
    </h3>
    <p>{{ report.day }}</p>
  </div>
</template>

<script>
export default {
  name: 'Report',
  props: {
    report: Object,
  },
}
</script>

<style scope>

.fas {
  color: red;
}

.report {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.report.reminder {
  border-left: 5px solid green;
}

.report h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
