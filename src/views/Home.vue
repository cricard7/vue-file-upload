<template>
  <div class="container">
    <div class="large-12 medium-12 small-12 cell">
      <label>File
        <input type="file" id="file" ref="file" v-on:change="handleFileUpload($event)"/>
      </label>
        <button v-on:click="submitFile()">Submit</button>
    </div>


  

  </div>
</template>

<script>
// @ is an alias to /src

//https://serversideup.net/uploading-files-vuejs-axios/
//https://codepen.io/joezimjs/pen/yPWQbd

import papaparse from 'papaparse'

export default {
  name: 'Home',
  components: {
    
  },
  data: function(){
    return{
      file: ''
    }
  },
  methods:{
    handleFileUpload(e){
     
      this.file = this.$refs.file.files[0];

      console.log(this.file)
      console.log(e.target.files[0])

      //https://edwardize.blogspot.com/2018/04/parsinf-csv-file-import-javascript-vuejs.html
      //https://www.joyofdata.de/blog/parsing-local-csv-file-with-javascript-papa-parse/
      

      //have to check the browswer has a FileReader
      if(window.FileReader){
        var reader = new FileReader();
        reader.readAsText(e.target.files[0]);
        // Handle errors load
        reader.onload = function(event) {
          var csv = event.target.result;
          
          papaparse.parse(csv,{
            header: true,
            dynamicTyping: true,
            complete: function(results) {
              
              console.log(results.data)
           } 
          })
          
        };
        reader.onerror = function(evt) {
          if(evt.target.error.name == "NotReadableError") {
            alert("Canno't read file !");
          }
        };

      }

      else {
        alert('FileReader are not supported in this browser.');
      }
     
    }
  },

  mounted: function(){



  }
}
</script>
