<template>
  <div id="login">
    <div id="login-wrap">
        <ul class="menu-tab">
            <li v-for="item in menuTab" :key="item.id" :class="{'current':item.current}" @click="toggleMeun(item)">{{item.text}}</li>
        </ul>
        <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" size="medium">
            <el-form-item prop="username">
                <label>邮箱</label>
                <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item prop="password">
                <label>密码</label>
                <el-input type="password" v-model="ruleForm.password" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item prop="code">
                <label>验证码</label>
                <el-row :gutter="10">
                    <el-col :span="15">
                        <el-input v-model.number="ruleForm.code" autocomplete="off"></el-input>
                    </el-col>
                    <el-col :span="9">
                        <el-button type="success"  class="block">获取验证码</el-button>
                    </el-col>
                </el-row>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')" class="login-btn block">提交</el-button>
            </el-form-item>
        </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: "login",
  data(){ // 数据绑定
    var validateUsername = (rule, value, callback) => {
        let regEmail = /^[A-Za-z0-9\u4e00-\u9fa5]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/
        if (!value) {
            return callback(new Error('请输入用户名'));
        } else if (!regEmail.test(value)) {
            callback(new Error('邮箱格式错误!'));
        } else {
            callback();
        }
    };
    var validatePassword = (rule, value, callback) => {
        let regPassword = /^[A-Za-z0-9\u4e00-\u9fa5]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/
        if (value === '') {
            callback(new Error('请输入密码'));
        } else {
            if (this.ruleForm.checkPass !== '') {
            this.$refs.ruleForm.validateField('checkPass');
            }
            callback();
        }
    };
    var checkCode = (rule, value, callback) => {
        let regCode = /^[A-Za-z0-9\u4e00-\u9fa5]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/
        if (value === '') {
            callback(new Error('请输入验证码'));
        } else if (value !== this.ruleForm.pass) {
            callback(new Error('两次输入密码不一致!'));
        } else {
            callback();
        }
    };
    return {
        menuTab:[
            {'text':'登录',current:true},
            {'text':'注册',current:false}
        ],
        ruleForm: {
            username: '',
            password: '',
            code: ''
        },
        rules: {
            username: [
                { validator: validateUsername, trigger: 'blur' }
            ],
            password: [
                { validator: validatePassword, trigger: 'blur' }
            ],
            code: [
                { validator: checkCode, trigger: 'blur' }
            ]
        }
    }
  },
  created(){ // 创建完成时

  },
  mounted(){ // 挂在完成后自动执行

  },
  methods:{   // 所有函数方法放在这里
    toggleMeun(data){
        this.menuTab.forEach(elem => {
            elem.current = false;
        });
        data.current = true;
        console.log(data.text);
        console.log(data.current);
    },
    submitForm(formName) {
        this.$refs[formName].validate((valid) => {
            if (valid) {
            alert('submit!');
            } else {
            console.log('error submit!!');
            return false;
            }
        });
    },
    resetForm(formName) {
        this.$refs[formName].resetFields();
    }
  }
};
</script>

<style lang="scss" scoped>
#login {
    height: 100vh;
    background-color:#344a5f;
}

#login-wrap {
    width: 300px;
    margin: auto;
}

.menu-tab {
    text-align: center;
    li{
        display: inline-block;
        width: 88px;
        line-height: 36px;
        font-size: 14px;
        color: #fff;
        border-radius: 2px;
        cursor: pointer;
    }
    .current {
        background-color: rgba(0,0,0,.1);
    }
}
</style>
