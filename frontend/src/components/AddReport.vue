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

    <div class="form-control">
      <label>File</label>
        <input type="file" id="file" ref="file" item="file" name="file" @change="onFilePicked" />
    </div>

    <div>
      <table id="tbl-data"></table>
    </div>
    <input type="submit" value="Analyse Report" class="btn btn-block" />
  </form>
  
</template>

<script>

import readXlsxFile from 'read-excel-file';
import axios from 'axios';

export default {

  name: 'AddReport',

  data() {
    return {
      name: "",
      day: "",
      url: ""
    }
  },

  methods:{

    onFilePicked(event){
      const files = event.target.files
      const fileReader = new FileReader()
      fileReader.addEventListener('load', () => {
        this.url = fileReader.result
      })
      this.excel = fileReader.readAsDataURL(files[0])
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
        excel: this.excel,
        url: this.url
        
      }

      this.$emit('add-report', newReport)
      //Empty the boxes
      this.name = ''
      this.day = ''
      this.excel = null
      this.url = ''
    },
  }
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

.excel-upload-input{
  display: none;
  z-index: -9999;
}
.drop{
  border: 2px dashed #bbb;
  width: 150px;
  height: 160px;
  line-height: 160px;
  margin: 0 auto;
  font-size: 10px;
  border-radius: 5px;
  text-align: center;
  color: #bbb;
  position: relative;
}
</style>