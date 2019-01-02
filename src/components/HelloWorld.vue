<template>
  <div>
    <span v-text="$t('m.music')"></span>
    <span>{{$t('m.music')}}</span>
    <el-input v-model="dome"></el-input>
    <el-input v-model="b.c"></el-input>
    {{value}}
    {{b}}

    <el-input v-model="object[0].name"></el-input>
    {{object}}
    {{bb}}

    <hr>
    <el-input v-model="objecta[0].name"></el-input>
    {{objecta}}
    {{bb}}

    <p></p>
    <el-button @click="getObj">获取对象</el-button>
    <button @click="changeLangEvent">切换语言</button>
    <button @click="getuser">按钮</button>

    <el-dropdown trigger="click" @command="handleCommand">
      <span class="el-dropdown-link">中文
        <i class="el-icon-arrow-down el-icon--right"></i>
      </span>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item command="zh-CN">中文</el-dropdown-item>
        <el-dropdown-item command="en-US">English</el-dropdown-item>
        <el-dropdown-item command="jp">日本語</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>

    <table class="ntable" v-for="(item,index) in accounting.repay" :key="index">
      <tr>
        <td width="35%">申请人</td>
        <td width="65%">
          <el-input v-model="item.applyPerson" v-if="aisShow"></el-input>
          <div v-else>{{item.applyPerson}}</div>
        </td>
      </tr>
      <tr>
        <td width="35%">申请人</td>
        <td width="65%">
          <el-input v-model="item.applyPerson" v-if="aisShow"></el-input>
          <div v-else>{{item.applyPerson}}</div>
        </td>
      </tr>
      <tr>
        <td width="35%">申请人</td>
        <td width="65%">
          <el-input v-model="item.applyPerson" v-if="aisShow"></el-input>
          <div v-else>{{item.applyPerson}}</div>
        </td>
      </tr>
    </table>
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
      dome:"",
      value:"",
      b:{
        c:1
      },
      bb:{
        
      },
      object:[{
        name:'zhangsan',
        age:45
      }],
      objecta:[{
        name:'lisi',
        age:45
      }],
      user: {
        userName: "tyty11",
        password: "123456"
      },
      accounting: {
        id: null,
        deposit: [
          {
            isPaid: true,
            amount: "",
            paidPerson: "",
            paidDate: "",
            note: ""
          }
        ],
        advancePayment: [
          {
            isPaid: true,
            amount: null,
            paidPerson: null,
            paidDate: null,
            note: null
          }
        ],
        refund: [
          {
            isApplyRefund: true,
            applyPerson: null,
            applyReason: null,
            applyDate: null,
            note: null
          }
        ],
        repay: [
          {
            applyPerson: null,
            applyReason: null,
            applyDate: null,
            note: null
          }
        ],
        invoice: [
          {
            id: null,
            name: null,
            issueDate: null,
            cancelDate: null,
            note: null
          }
        ],
        note: null
      }, // 财务信息
      aisShow: true
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
      if (command === "en-US") {
        this.lang = "en-US";
        this.$i18n.locale = this.lang; //关键语句
      } else if (command === "zh-CN") {
        this.lang = "zh-CN";
        this.$i18n.locale = this.lang; //关键语句
      } else {
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
    },
    getObj(){
      console.info(this.bb)
    }
  },
  created() {
    // this.accounting=Object.assign({},this.accounting)
    this.accounting = {};
  },
  watch:{
    dome(val){
      console.info(val)
      this.value=this.dome
    },
    b:{
      deep:true,
      handler:function (newVal) {
        console.info(newVal)
      }
    },
    object:{
      deep:true,
      handler:function (val,oldVal) {
          this.bb.object=this.object.concat()
        // if (val!==oldVal) {
        //   // this.bb=Object.assign(this.bb,this.object)
        //   console.info(this.bb.object)
        // }
      }
    },
    objecta:{
      deep:true,
      handler:function(){
        this.bb.objecta=this.objecta.concat()
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped  lang="less" >
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
.ntable {
  width: 100%;
  margin: 0 auto;
  margin: 10px 0;
  td {
    padding: 12px 10px 12px 10px;
    border: #e4eef6 1px solid;
    word-break: break-all;
    font-size: 14px;
    line-height: 19px;
    color: #222;
  }
  .tb {
    background: #f2f9fc;
  }
}

table {
  border-spacing: 0;
  border-collapse: collapse;
}
</style>
