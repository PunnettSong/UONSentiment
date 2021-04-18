<template>
  <AddReport v-show="showAddReport" @add-report="addReport" />
  <Reports
    @toggle-reminder="toggleReminder"
    @delete-report="deleteReport"
    :reports="reports"
  />
</template>

<script>
import Reports from '../components/Reports'
import AddReport from '../components/AddReport'

export default {
  name: 'Home',
  props: {
    showAddReport: Boolean,
  },
  components: {
    Reports,
    AddReport,
  },
  data() {
    return {
      reports: [],
    }
  },
  methods: {
    async addReport(report) {
      const res = await fetch('api/reports/create', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(report),
      })

      const data = await res.json()

      this.reports = [...this.reports, data]
    },
    async deleteReport(id) {
      if (confirm('Are you sure?')) {
        const res = await fetch(`api/reports/${id}/delete`, {
          method: 'DELETE',
        })

        res.status === 200
          ? (this.reports = this.reports.filter((report) => report.id !== id))
          : alert('Error deleting report')
      }
    },
    async toggleReminder(id) {
      const reportToToggle = await this.fetchReport(id)
      const updReport = { ...reportToToggle, reminder: !reportToToggle.reminder }

      const res = await fetch(`api/reports/${id}/update`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updReport),
      })

      const data = await res.json()

      this.reports = this.reports.map((report) =>
        report.id === id ? { ...report, reminder: data.reminder } : report
      )
    },
    async fetchReports() {
      const res = await fetch('api/reports')

      const data = await res.json()

      return data
    },
    async fetchReport(id) {
      const res = await fetch(`api/reports/${id}`)

      const data = await res.json()

      return data
    },
  },
  async created() {
    this.reports = await this.fetchReports()
  },
}
</script>
