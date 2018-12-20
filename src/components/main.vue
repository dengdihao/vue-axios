<template>
  <div>
    main
    <img :src="imgpath" alt>
    <input type="file" @change="changeImage($event)" accept="image/gif,image/jpeg,image/jpg,image/png">
    <button @click="logout">登出</button>
    <button @click="logout">登出</button>
  </div>
</template>


<script>
import { _logoutuser } from "../axios/service.js";
import store from "../store/store.js";
export default {
  data() {
    return {
      imgpath: require('../assets/logo.png')
    };
  },
  methods: {
    logout() {
      console.info(11);
      _logoutuser()
        .then(res => {
          // debugger
          console.info(res);
          alert("登出成功");
          this.$router.push("/");
          this.$store.commit("del_token", localStorage.getItem("token"));
        })
        .catch(err => {
          console.info(err);
        });
    },
    changeImage(e) {
      var file = e.target.files[0];
      console.info(file)
      var reader = new FileReader();
      var that = this;
      reader.readAsDataURL(file);
      // console.info(res)
      reader.onload = function(e) {
        that.imgpath = this.result;
      };
    }
  }
};
</script>