<template>
  <div>
    <span v-text="$t('m.music')"></span>
    
    <span>{{$t('m.music')}}</span>
    <button @click="changeLangEvent">切换语言</button>
    <button @click="getuser">按钮</button>

    <el-dropdown trigger="click" @command="handleCommand">
      <span class="el-dropdown-link">
        中文<i class="el-icon-arrow-down el-icon--right"></i>
      </span>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item command="zh-CN">中文</el-dropdown-item>
        <el-dropdown-item command="en-US">English</el-dropdown-item>
        <el-dropdown-item command="jp">日本語</el-dropdown-item>
        
      </el-dropdown-menu>
    </el-dropdown>
  </div>
</template>

<script>
import { _userinfo } from "../axios/service.js";
import store from "../store/store.js";

export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to Your Vue.js App",
      user: {
        userName: "111",
        password: "pwd123"
      }
    };
  },
  methods: {
    getuser() {
      _userinfo(this.user)
        .then(res => {
          debugger;
          console.info(res.token);
          this.$store.commit("set_token", res.token);
          console.info(store);

          if (store.state.token) {
            alert("成功");
            this.$router.push("/main");
          } else {
            this.$router.replace("/");
          }
        })
        .catch(err => {
          alert("失败");
          console.info(err);
        });
    },

    handleCommand(command) {
        if (command==="en-US") {
          this.lang = "en-US";
            this.$i18n.locale = this.lang; //关键语句
        }else if (command ==="zh-CN") {
          this.lang = "zh-CN";
            this.$i18n.locale = this.lang; //关键语句
        }else{
          this.lang = "jp";
            this.$i18n.locale = this.lang; //关键语句
        }
      },
    /**
     * 切换语言
     */

    changeLangEvent() {
      this.$confirm("确定切换语言吗?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          if (this.lang === "zh-CN") {
            this.lang = "en-US";
            this.$i18n.locale = this.lang; //关键语句
          } else {
            this.lang = "zh-CN";
            this.$i18n.locale = this.lang; //关键语句
          }
        })
        .catch(() => {
          this.$message({
            type: "info"
          });
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
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
</style>
