<template>
  <div class="container my-4">

    <div class="card shadow border-0 mb-5">
      <div class="card-header py-2">
        <h2 class="mb-0">{{ judul }}</h2>
      </div>
      <div class="card-body">
        <form ref="formImage">
          <div class="form-group">
            <div class="alert alert-success alert-dismissible fade show" role="alert" v-if="success">
              <span class="svg-icon svg-icon-muted svg-icon-2hx">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none">
                  <path opacity="0.5" d="M12.8956 13.4982L10.7949 11.2651C10.2697 10.7068 9.38251 10.7068 8.85731 11.2651C8.37559 11.7772 8.37559 12.5757 8.85731 13.0878L12.7499 17.2257C13.1448 17.6455 13.8118 17.6455 14.2066 17.2257L21.1427 9.85252C21.6244 9.34044 21.6244 8.54191 21.1427 8.02984C20.6175 7.47154 19.7303 7.47154 19.2051 8.02984L14.061 13.4982C13.7451 13.834 13.2115 13.834 12.8956 13.4982Z" fill="black"/>
                  <path d="M7.89557 13.4982L5.79487 11.2651C5.26967 10.7068 4.38251 10.7068 3.85731 11.2651C3.37559 11.7772 3.37559 12.5757 3.85731 13.0878L7.74989 17.2257C8.14476 17.6455 8.81176 17.6455 9.20663 17.2257L16.1427 9.85252C16.6244 9.34044 16.6244 8.54191 16.1427 8.02984C15.6175 7.47154 14.7303 7.47154 14.2051 8.02984L9.06096 13.4982C8.74506 13.834 8.21146 13.834 7.89557 13.4982Z" fill="black"/>
                </svg>
              </span>
              &nbsp;<strong>Horray!</strong> Image uploaded successfully.
              <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
            </div>
            <div class="alert alert-danger alert-dismissible fade show" role="alert" v-if="error">
              <span class="svg-icon svg-icon-muted svg-icon-2hx">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none">
                  <rect opacity="0.5" x="7.05025" y="15.5356" width="12" height="2" rx="1" transform="rotate(-45 7.05025 15.5356)" fill="black"/>
                  <rect x="8.46447" y="7.05029" width="12" height="2" rx="1" transform="rotate(45 8.46447 7.05029)" fill="black"/>
                </svg>
              </span>
              &nbsp;<strong>Sorry!</strong> Image failed to upload.
              <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
            </div>
            <div class="alert alert-warning alert-dismissible fade show" role="alert" v-if="alertSize">
              <span class="svg-icon svg-icon-muted svg-icon-2hx">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none">
                  <rect opacity="0.3" x="2" y="2" width="20" height="20" rx="10" fill="black"/>
                  <rect x="11" y="14" width="7" height="2" rx="1" transform="rotate(-90 11 14)" fill="black"/>
                  <rect x="11" y="17" width="2" height="2" rx="1" transform="rotate(-90 11 17)" fill="black"/>
                </svg>
              </span>
              &nbsp;<strong>Oops!</strong> Image file is too large.
              <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
            </div>
            <div class="alert alert-warning alert-dismissible fade show" role="alert" v-if="alertType">
              <span class="svg-icon svg-icon-muted svg-icon-2hx">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none">
                  <rect opacity="0.3" x="2" y="2" width="20" height="20" rx="10" fill="black"/>
                  <rect x="11" y="14" width="7" height="2" rx="1" transform="rotate(-90 11 14)" fill="black"/>
                  <rect x="11" y="17" width="2" height="2" rx="1" transform="rotate(-90 11 17)" fill="black"/>
                </svg>
              </span>
              &nbsp;<strong>Oops!</strong> The uploaded file must be an image jpg/jpeg/bmp/png/gif.
              <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
            </div>
            <label class="mb-1">Image</label>
            <input type="file" accept="image/*" @change="previewImage" class="form-control" id="my-file" name="img">
            <p v-if="errors.length" class="mb-0">
              <span class="text-danger" v-for="error in errors">{{ error }}</span>
            </p>
            <div class="border p-2 my-3" v-if="preview">
              <p>Preview Here:</p>
              <template v-if="preview">
                <img :src="preview" class="img-fluid" />
                <p class="mb-0">file name: {{ image.name }}</p>
                <p class="mb-0">size: {{ image.size/1024 }}KB</p>
              </template>
            </div>
          </div>
          <div class="my-2">
            <button class="btn btn-secondary me-2 px-4" @click.prevent="submit">Save</button>
          </div>
        </form>
      </div>
    </div>

    <div class="pb-4">
      <h2 class="mb-0">{{ msg }}</h2>
    </div>
    <div class="row">
      <div class="col-lg-4 col-md-12" v-for='(image, index) in images' :key='image.id'>
        <img
          :src="image.display_url"
          class="w-100 shadow-1-strong rounded mb-4"
        />
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: "Body",
  props: {
    msg: String,
    judul: String,
  },
  data: function() {
    return {
      preview: null,
      image: null,
      images: JSON.parse(localStorage.getItem('images')),
      success: false,
      error: false,
      alertSize: false,
      alertType: false,
      errors: [],
      img: null,
    };
  },
  methods: {
    previewImage: function(event) {
      var input = event.target;
      if (input.files) {
        var reader = new FileReader();
        reader.onload = (e) => {
          this.preview = e.target.result;
        }
        this.image=input.files[0];
        reader.readAsDataURL(input.files[0]);
      }
    },
    reset: function() {
      this.image = null;
      this.preview = null;
    },
    submit() {
      const formData = new FormData();
      formData.append('image', this.image);
      let allowedTypes = ['image/jpg', 'image/jpeg', 'image/webp', 'image/png', 'image/gif'];

      if (this.image) {
        return true;
      }

      this.errors = [];

      if (!this.img) {
        this.errors.push('Please upload an image!');
      }else{
        if (!allowedTypes.includes(this.image.type)) {
          this.alertType = true;
        }else{
          if(this.image.size <= 2000000){
            this.axios.post('https://api.imgbb.com/1/upload?key=aa0c9eeb2900979e6a37ece7f9983eb2',
              formData
            ).then(function (response) {
              var data = response.data.data
              var images = {};
              if (localStorage.getItem('images')) {
                images = JSON.parse(localStorage.getItem('images'));
              }
              images[data.id] = data;
              localStorage.setItem('images', JSON.stringify(images));
              this.reloadCollection();
              if (response.data.status == 200) {
                this.success = true;
              } else {
                this.error = true;
              }
              this.reset();
              this.$refs.formImage.reset();
            }.bind(this));
          }else{
            this.alertSize = true;
          }
        }
      }
    },
    reloadCollection(){
      this.images = JSON.parse(localStorage.getItem('images'))
    }
  }
};
</script>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
