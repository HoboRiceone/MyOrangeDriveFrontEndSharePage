<template>
  <div id="app">
    <div class="loginput">
      <p style="">Please enter the share password:</p>
      <br/>
      <el-input style="width:70%;margin-right:5%;float:left;" v-model="sharepassword" ></el-input>
      <el-button style="" type="primary" @click="download">Download</el-button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data ()
  {
    return{
      sharekey: "",
      sharepassword: "",
      baseurl: "http://34.204.85.79:8081",
      sharefileid: 0,
    };
  },
  created: function () 
  {
    this.sharekey = this.getQueryVariable("key");
  },
  methods:
  {
    getQueryVariable(variable)
    {
       var query = window.location.search.substring(1);
       var vars = query.split("&");
       for (var i=0;i<vars.length;i++) {
               var pair = vars[i].split("=");
               if(pair[0] == variable){return pair[1];}
       }
       return(false);
    },
    download()
    {
      axios.get(this.baseurl+"/shares/"+this.sharekey,{params:{password: this.sharepassword}})
      .then((response) => 
      {
        this.sharefileid = response.data.result_data.file_id;
        axios.get(this.baseurl+'/api/d/'+this.sharefileid)
        .then((response) => 
        {
          var link = document.createElement('a');
          link.download="";
          link.href = response.data.result_data.download_url;
          link.click();
        })
        .catch(function (error) 
        {
          console.log(error.response);
        });
      })
      .catch(function (error) 
      {
        console.log(error.response);
      });
    }

  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin:0;
  padding:0;
  height:100%;
  width:100%;
}
.loginput{
  background-color:white; 
  position:absolute;
  left:50%;
  top:50%;
  width:30%;
  text-align:left;
  padding-top:4%;
  padding-bottom:4%;
  padding-left:3%;
  transform:translate(-50%,-50%);
  border-radius: 6px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04);
  }
</style>
