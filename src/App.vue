<template>
  <div class="container">
   <div id="app">
      <file-upload
        title="Drop files here"
        class="file-upload"
        name="file"
        post-action="./post.php"
        put-action="./put.php"
        :extensions="extensions"
        :accept="accept"
        :multiple="multiple"
        :size="size"
        ref="upload"
        :drop="drop" />

  <ul class="list-group file-list">
    <li v-for="(file, index) in files" class="list-group-item">
      <button @click="remove(index)" type="button" class="close" aria-label="Close">
        <span aria-hidden="true">&times;</span>
      </button>
      {{file.name}}
    </li>
  </ul>


  <button
    :disabled="files.length < 1"
    type="button"
    class="btn btn-primary">
    Upload
  </button>
  </div>
  </div>

</template>

<script>
import FileUpload from 'vue-upload-component'




export default {
  name: 'app',
  components: { FileUpload },
  data() {
    return {
      accept: 'image/*',
      size: 1024 * 1024 * 10,
      multiple: true,
      extensions: 'gif,jpg,png',
      // extensions: ['gif','jpg','png'],
      // extensions: /\.(gif|png|jpg)$/i,
      files: [],
      upload: {},
      drop: true,
      auto: false,

    }
  },

  mounted() {
    this.upload = this.$refs.upload;
    this.upload.request = {
      headers: {
        "X-Csrf-Token": "xxxx",
      },
      data: {
        "_csrf_token": "xxxxxx",
      },
    };
    this.files = this.$refs.upload.files;
    console.log('update files');

  },

  filters: {
    formatSize(size) {
      if (size > 1024 * 1024 * 1024 * 1024) {
        return (size / 1024 / 1024 / 1024 / 1024).toFixed(2) + ' TB';
      } else if (size > 1024 * 1024 * 1024) {
        return (size / 1024 / 1024 / 1024).toFixed(2) + ' GB';
      } else if (size > 1024 * 1024) {
        return (size / 1024 / 1024).toFixed(2) + ' MB';
      } else if (size > 1024) {
        return (size / 1024).toFixed(2) + ' KB';
      }
      return size.toString() + ' B';
    }
  },
  methods: {
    remove(index) {
      this.$refs.upload.files.splice(index, 1);
    },
  },
  events: {
    addFileUpload(file, component) {
      console.log('addFileUpload');
      if (this.auto) {
        component.active = true;
      }
    },
    fileUploadProgress(file, component) {
      console.log('fileUploadProgress ' + file.progress);
    },
    afterFileUpload(file, component) {
      console.log('afterFileUpload');
    },
    beforeFileUpload(file, component) {
      console.log('beforeFileUpload');
    },
  }
}
</script>

<style>
body {
  font-size: 16px;
}
#app {
  padding-top: 3rem;
}
.file-uploads {
  width: 100%;
  height: 300px;
  background: #e3e3e3;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer !important;
}
.file-uploads:hover {
  border: 3px dashed #666;
}
.file-uploads span {
  text-transform: uppercase;
  font-weight: bold;
  font-size: 2rem;
  color: #666;
}

.file-list {
  margin-bottom: 1rem;
}
.file-list li {
  padding-right: 50px;
}
li .close {
  position: absolute;
  right: 20px;
}

</style>
