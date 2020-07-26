<template>
  <div id="tou">
    <header>
      <div style="width:100%;background-color: #636363; overflow: hidden;height: 100%">
        <router-link to="/homepage" id="menu_btn" class="demonstration" style="float:left;padding-top:10px;color:white;margin-left:1%">
          <span>Home</span>
        </router-link>
        <span class="demonstration" style="float:left;padding:5px;color:white;margin-left:2%;width:15%">
                <el-input
                  placeholder="请输入"
                  icon="search"
                  v-model="search"
                  :on-icon-click="seek">
                </el-input>
            </span>
        <span class="demonstration" style="float:right;padding-top:10px;margin-right:1%">
                <el-dropdown trigger="click">
                  <span class="el-dropdown-link" style="color:white">
                    admin<i class="el-icon-caret-bottom el-icon--right"></i>
                  </span>
                  <el-dropdown-menu slot="dropdown">
                    <el-dropdown-item>个人信息</el-dropdown-item>
                    <el-dropdown-item>退出登录</el-dropdown-item>
                  </el-dropdown-menu>
                </el-dropdown>
            </span>
      </div>
    </header>
  </div>

</template>
<script>
  export default {
    name: "tou",
    data() {
      return {
        search: "",
      };
    },
    methods: {
      onSubmit() {
        return false;
      },
      seek() {
        if(this.search)
        {
          this.$router.push({ path: "/list", query: { search: this.search } });
          this.search=""
        }
      }
    }
  };
</script>
<style scoped>
  a {
    color: #000;
  }
  .search-form {
    width: 33%;
  }
  @media screen and (max-width: 800px) {
    .man {
      display: none;
    }
    header .target {
      margin-left: 5px;
    }
    .searchW {
      margin-left: 0px;
    }
    .search-form {
      width: 250px;
    }
    .search_cat {
      left: 157px;
    }
  }
</style>
