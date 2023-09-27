<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      <input type="file" ref="fileInput" style="display: none" @change="handleUpload" accept="image/*">
      <div v-if="imageUrl">
        <img :src="imageUrl" alt="avatar" style="width: 100%">
      </div>
      <div v-else>
        <div @click="openFileInput">
          <plus-outlined />
          <div style="margin-top: 8px">上传图片</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { message } from 'ant-design-vue';
import { PlusOutlined } from '@ant-design/icons';
import { PicGo } from 'picgo'

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  components: {
    PlusOutlined,
  },
  data() {
    return {
      imageUrl: '',
    };
  },
  methods: {
    openFileInput() {
      this.$refs.fileInput.click();
    },
    handleUpload(event) {
      const file = event.target.files[0];
      const requireFunc = typeof __webpack_require__ === 'function' ? __non_webpack_require__ : require
      const PicGo = requireFunc('picgo')
      const pgo = new PicGo();

      // 使用VuePicgo上传图片
      pgo.upload([file])
        .then((result) => {
          if (result && result[0] && result[0].imgUrl) {
            this.imageUrl = result[0].imgUrl;
          } else {
            message.error('图片上传失败');
          }
        })
        .catch((error) => {
          message.error('图片上传失败');
        });
    },
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
</style>
