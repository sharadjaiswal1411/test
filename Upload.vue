<template>
    <div class="container">
        <div class="row">
            <div class="col-md-8 col-md-offset-2">
                <div class="panel panel-default">
                    <div class="panel-heading">Uploading image using Vue.js + Laravel</div>

                    <div class="panel-body">
                          <div v-if="!image">
                                <h2>Select an image</h2>
                                <input type="file" @change="onFileChange">
                           </div>
                          <div v-else>
                                <img :src="image" />
                                <button @click="removeImage">Remove image</button>
                                <button @click="uploadImage">Upload image</button>
                           </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
      data:function (){ 
                
                return {image:''}

           },
          methods: {
            onFileChange(e) {
              var files = e.target.files || e.dataTransfer.files;
              if (!files.length)
                return;
              this.createImage(files[0]);
            },
            createImage(file) {
              var image = new Image();
              var reader = new FileReader();
              var vm = this;

              reader.onload = (e) => {
                vm.image = e.target.result;
              };
              reader.readAsDataURL(file);
            },
            uploadImage() {
                Axios.post('/api/upload',{image: this.image}).then(response => {
                    //this.$router.push({name: 'Success'}) 
                });
            },
            removeImage: function (e) {
              this.image = '';
            }
          },
          mounted() {
            console.log('Component mounted.')
          }
    }
</script>
