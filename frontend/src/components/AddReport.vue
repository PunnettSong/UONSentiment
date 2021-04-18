
<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Report</label>
      <input type="text" v-model="name" name="name" placeholder="Add Report" />
    </div>
    <div class="form-control">
      <label>Day & Time</label>
      <input
        type="text"
        v-model="day"
        name="day"
        placeholder="Add Day & Time"
      />
    </div>
    <!--div class="form-control form-control-check">
      <label>Set to analyse</label>
      <input type="checkbox" v-model="reminder" name="reminder" />
    </div-->

    <div id="app">
      <input type="file" @change="onFileSelected" />
    </div>

    <input type="submit" value="Save Report" class="btn btn-block" />
  </form>
</template>

<script>

import readXlsxFile from 'read-excel-file'
import axios from 'axios'

export default {

  name: 'AddReport',
  data() {
    return {
      name: '',
      day: '',
      reminder: false,
      selectedFile: null
    }
  },
  
  methods: {

    onFileSelected(event) {
      this.selectedFile = event.target.files[0]
    },

    onUpload(){

    },

    onSubmit(e) {
      e.preventDefault()

      if (!this.name) {
        alert('Please add a report')
        return
      }

      const newReport = {
        // id: Math.floor(Math.random() * 100000),
        name: this.name,
        day: this.day,
        reminder: this.reminder,
        selectedFileName: this.selectedFile.name
      }

      /*const fd = new FormData();
      fd.append('file', this.selectedFile, this.selectedFile.name)
      axios.post('', fd)
        .then(res => {
          console.log(res)
        });*/

      this.$emit('add-report', newReport)

      //Empty the boxes
      this.name = ''
      this.day = ''
      this.reminder = false
      this.selectedFileName = ''
    }
  },
}

</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}

.form-control {
  margin: 20px 0;
}

.form-control label {
  display: block;
}

.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}

.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.form-control-check label {
  flex: 1;
}

.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>
