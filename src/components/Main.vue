<template>
<b-container>
<div id="upload" border="2 solid black">
  <br>
  <label for="file-upload">
    <b-img fluid  id="img_preview" :src="url" v-if="isFile"></b-img>
    <i class="xi-plus-circle-o xi-4x" id="pl" style="cursor:pointer;" v-if="!isFile"> </i> </label>
  <input type="file" id="file-upload" ref="file" style="display = none;" accept="image/*" v-on:change="preview">

</div> 
<br />

<b-form-group label="변환할 스타일을 골라주세요">
      <b-form-radio-group
        id="radio-group"
        v-model="selected"
        :options="options"
        name="radio-options"
      ></b-form-radio-group>
    </b-form-group>


<b-button variant="primary" v-on:click="submit">변환하기</b-button>

<!--
<div id="changed_img_div">
  
  <img :src="changedImg">


</div>
-->


<b-modal id="modal" ref="modal" hide-footer hide-header hide-header-close centered size="xl" >
  <p v-if="modalif"> 변환완료! </p>
  <b-img :src="changedImg" v-if="modalif" id="changedImg"> </b-img>
  <div class="text-center" v-if="!modalif">
    <br/>
  <b-spinner id="spin" variant="primary" label="Spinning" v-if="!modalif"></b-spinner>
    <br/> <br/>
  </div>

</b-modal>



</b-container>



</template>  

<script>
export default {
  name: 'Main',
  data () {
    return {
      isFile : false,
      url : '',
      changedImg : '',
      radio : 0,
      
      selected : 0,
      options : [
        {text : '미야자키 하야오', value : 0},
        {text : '호소다 마모루', value : 1},
        {text : '파프리카', value : 2},
        {text : '신카이 마코토', value : 3},

      ],
      modalif : false,
    }
  },
  methods : {
    preview : function(e) {
      this.isFile = true;
      console.log(e.target.files);

      this.url = URL.createObjectURL(e.target.files[0]);

      console.log(this.isFile);
      console.log(this.url);

      
    },
    submit : function() {

      this.$refs.modal.show();
      let formData = new FormData();
      let fileData = this.$refs.file.files[0]

      console.log(fileData)
      console.log(this.selected);


      formData.append("file", fileData);
      formData.append("value", this.selected);


      this.$http.post("api/home/send/", formData, { headers : { "Content-Type" : "multipart/form-data"} }).then(response => {

      console.log(response);

      console.log(response.data);

        this.changedImg = 'api/home/img/' + response.data;
        this.modalif = true;
        //this.$refs.modal.hide();
      }).catch(function(e){
        console.log(e);
      });

      console.log('submit');

    }
  }
}
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

#changedImg {
  width : 100%
}

#img_preview {
  width : 100%;
  height: 100%;
  overflow: hidden;
}

#file-upload {
  cursor : pointer;
  display: none;
}

#pl{
  vertical-align: middle;
  
}

#upload {
  margin-left: 25%;
  width : 50%;
  border: 3px solid black;
  border-radius: 10px 10px 10px 10px;
  overflow: hidden;
}

h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

#modal {
  width :fit-content;
}

#spiner {
  margin : 0 auto;
}

</style>
