<template>
  <div>
    <div>
      <el-row>
        <el-col :span="12"> <el-input
          placeholder="请输入标题"
          v-model="input"
          clearable>
        </el-input>
        </el-col>
        <el-col :span="12"><el-button type="primary" @click="submit">发布文章</el-button></el-col>
      </el-row>
    </div>
    <mavon-editor
      ref="md"
      @change="change"
      @save="save"
      @imgAdd="imgAdd"
      @imgDel="imgDel"
      :ishljs="true"
      v-model="content"
      style="min-height: 600px"
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
        content: '',
        html:'',
        input: ''
      };
    },
    methods: {
      // 所有操作都会被解析重新渲染
      change(value, render){        //value为编辑器中的实际内容，即markdown的内容，render为被解析成的html的内容
        this.html = render;
      },
      // 触发ctrl+s事件
      save(value, render) {
        this.html = render;
      },
      /**
       * 图片删除
       */
      // 绑定@imgAdd event
      imgAdd(pos, $file){
        // 第一步.将图片上传到服务器.
        var formdata = new FormData();
        formdata.append('file', $file);
        this.$axios({
          url: '/article/upload',
          method: 'post',
          data: formdata,
          headers: { 'Content-Type': 'multipart/form-data' },
        }).then(res => {
          // 第二步.将返回的url替换到文本原位置![...](./0) -> ![...](url)
          /**
           * $vm 指为mavonEditor实例，可以通过如下两种方式获取
           * 1. 通过引入对象获取: `import {mavonEditor} from ...` 等方式引入后，`$vm`为`mavonEditor`
           * 2. 通过$refs获取: html声明ref : `<mavon-editor ref=md ></mavon-editor>，`$vm`为 `this.$refs.md`
           */
          console.log(res);
          this.$refs.md.$img2Url(pos,res.data.data)
        })
      },

      imgDel(pos) {
        delete this.img_file[pos];
      },
      // 提交
      submit(){
        var data = new FormData();
        data.append("content",this.html)
        data.append("title",this.input)
        data.append("author",this.content)
        this.$axios({
          url: '/article/release',
          method: 'post',
          data: data,
          headers:{
            'Content-type': 'application/x-www-form-urlencoded'
          },
        }).then(res => {
          console.log(res.data);
        })
      }
    }
  };
</script>

<style scoped>
  .el-row {
    margin-bottom: 20px;
  &:last-child {
     margin-bottom: 0;
   }
  }
  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #6eafdb;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
</style>
