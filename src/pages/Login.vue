<template>
<el-row  type="flex"  class="wrapper"  justify="center" align="middle">
        <el-form :model="form" status-icon :rules="rules" ref="form" label-width="auto" class="demo-ruleForm">
            <el-form-item label="账号">
              <el-input v-model="form.username"  autocomplete="no"></el-input>
            </el-form-item>
            <el-form-item label="密码" prop="password">
              <el-input v-model="form.password" :type="InputType" autocomplete="off"></el-input>
              <div class="iconmima" @click="eyeHover(InputType)">
                  <span v-if="InputType == 'password'" class="iconfont iconmima-yanjing-guanbi"></span>
                   <span v-else-if="InputType == 'text'" class="iconfont iconmimayanjing"></span>
              </div>
            </el-form-item>
            <el-form-item style="text-align: center;">
              <el-button type="primary" @click="submitForm">提交</el-button>
              <el-button @click="resetForm">重置</el-button>
            </el-form-item>
        </el-form>
  </el-row>
</template>

<script>
  export default {
    data() {
      var validatePass = (rule, value, callback) => {
      if (value === "") {
        // 密码输入框为空提醒输入密码
        callback(new Error("请输入密码"));
      } 
    };
      return {
        InputType : 'password',
        form : {
            username : "",
            password : "",
        },
        rules: {
          password: [
            { validator: validatePass, trigger: 'blur' }
          ]
        }
      };
    },
    // directives:{
    //  eye : {
    //     inserted : (el,binding) =>{
    //         let sonDom = el.childNodes;
    //         let input = Array.prototype.filter.call(sonDom,function(e){
    //             return e.type == 'password'
    //         })[0];
    //         console.log(binding);
    //    }
    //   }
    // },
    mounted(){
    //    document.onkeydown = (e) => {
    //           var key = window.event.keyCode;
    //           console.log(key);
    //           if(key==13){
    //               this.submitForm();
    //           }
    //    }
       this.KeyDownSend();
    },
    methods: {
      KeyDownSend() {
      document.onkeydown = (e) => {
        //  为了偷懒 直接在document身上绑定了键盘事件
        //  后面再改为表单范围内
        var key = window.event.keyCode;
        if (key == 13) {
          this.submitForm();
           }
         };
      },
      eyeHover(type){
          if(type == 'password'){
               this.InputType = 'text';
          }else{
              this.InputType = 'password';
          }
      },
      submitForm() {
          if(this.form.username == '' || this.form.password == ''){
              this.$message('把信息补充完整');
              return;
          }
          this.axios({
            url : '/login',
              method : "post",
              data : this.form,
          }).then(res=>{
              this.$message.success(res.data.message);
              localStorage.setItem("token",res.data.data.token);
               localStorage.setItem("user_id",res.data.data.user.id);
          })
      },
      resetForm() {
          this.form = {
              username : '',
              password : '',
          }
      }
    }
  }
</script>

<style lang="less" scoped>
.iconmima{
     position: absolute;
    right: -30px;
    top: 0;
}
.el-form-item__content{
    position: relative;
}
.el-form{
    width: 390px;
}
    .wrapper{
        position: absolute;
        width: 100%;
        top: 0;
        bottom: 0;
    }
</style>