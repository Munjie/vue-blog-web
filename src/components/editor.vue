<template>
  <div>
    <!--
        @save: 按下 ctrl + s 时触发
        @change: 当值发生改变时 触发
    -->
    <mavon-editor
      ref="markdownEditor"
      @change="markdownChange"
      @save="markdownSave"
      @imgAdd="imgAdd"
      @imgDel="imgDel"
      :ishljs="true"
      v-model="content"
    />
  </div>
</template>
<script>
  import { mavonEditor } from "mavon-editor";
  import "mavon-editor/dist/css/index.css";
  export default {
    name: "editor",
    components: { mavonEditor },
    data() {
      return {
        content: ""
      };
    },
    methods: {
      // 触发change事件
      markdownChange(value, render) {
        this.htmlValue = render;
      },
      // 触发ctrl+s事件
      markdownSave(value, render) {
        this.htmlValue = render;
      },
      /**
       * 图片删除
       */
      // 绑定@imgAdd event
      imgAdd(pos, $file) {
        // 第一步.将图片上传到服务器.
        var formdata = new FormData();
        formdata.append('image', $file);
        this.img_file[pos] = $file;
        this.$http({
          url: '/api/edit/uploadimg',
          method: 'post',
          data: formdata,
          headers: { 'Content-Type': 'multipart/form-data' },
        }).then((res) => {
          let _res = res.data;
          // 第二步.将返回的url替换到文本原位置![...](0) -> ![...](url)
          this.$refs.md.$img2Url(pos, _res.url);
        })
      },
      imgDel(pos) {
        delete this.img_file[pos];
      }
    }
  };
</script>
