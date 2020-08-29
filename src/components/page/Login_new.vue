<template>
    <div class="homeBox">
        <ul class="tyg-div">
            <li>让</li>
            <li>
                <div style="margin-left:20px;">测</div>
            </li>
            <li>
                <div style="margin-left:40px;">试</div>
            </li>
            <li>
                <div style="margin-left:60px;">变</div>
            </li>
            <li>
                <div style="margin-left:80px;">得</div>
            </li>
            <li>
                <div style="margin-left:100px;">轻</div>
            </li>
            <li>
                <div style="margin-left:120px;">松</div>
            </li>
        </ul>
        <div style="width:32%;height: auto;margin-left: 30%">
            <div class="title0">自动化测试平台</div>
            <div class="title1">项目管理、接口管理、用例管理、测试报告、任务设置</div>
            <div class="lun-container">
                <div class="carouse" id="carouse" style="transform-style: preserve-3d">
                    <div class="pic1"><img src="../../../src/assets/img/page1_0.png" alt="pic1" ></div>
                    <div class="pic2"><img src="../../../src/assets/img/page1_1.png" alt="pic2"  ></div>
                    <div class="pic3"><img src="../../../src/assets/img/page1_2.png" alt="pic3"  ></div>
                </div>
            </div>
            <img class="img-login" src="../../../src/assets/img/page1_3.jpg"/>
        </div>
        <div class="ms-login">
            <div class="ms-title">用户登录</div>
            <el-form :model="ruleForm" :rules="rules" ref="ruleForm" class="ms-content">
                <el-form-item prop="username">
                    <el-input v-model="ruleForm.username" placeholder="请输入用户名" @focus="clearValidate('username')">
                        <el-button slot="prepend" icon="el-icon-lx-people"></el-button>
                    </el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input type="password" placeholder="请输入密码" v-model="ruleForm.password"
                              @keyup.enter.native="submitForm('ruleForm')" @focus="clearValidate('password')">
                        <el-button slot="prepend" icon="el-icon-lx-lock"></el-button>
                    </el-input>

                </el-form-item>

                <el-form-item label="记住我" class="remember_me" size="mini">
                    <el-switch v-model="ruleForm.remember_me"></el-switch>
                    <el-link type="info" :underline="false" class="register_link" href="/register">没有账号？点击注册</el-link>
                </el-form-item>

                <div class="login-btn">
                    <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
                </div>
                <p class="login-tips" v-show="err_info">{{ err_msg }}</p>
            </el-form>
        </div>
    </div>
</template>

<script>
    /* eslint-disable */
    import { login } from '../../api/api';

    export default {
        data: function() {
            return {
                passwordType: 'password',
                ruleForm: {
                    username: '',
                    password: '',
                    remember_me: false
                },
                err_info: false,    // 是否显示错误提示
                err_msg: '',         // 具体错误提示
                rules: {
                    username: [
                        { required: true, message: '请输入用户名', trigger: 'blur' }
                    ],
                    password: [
                        { required: true, message: '请输入密码', trigger: 'blur' }
                    ]
                }
            };
        },
        methods: {
            submitForm(formName) {
                // alert(1111);
                this.$refs[formName].validate((valid) => {
                    // alert("valid: " + valid);
                    if (valid) {
                        // localStorage.setItem('ms_username',this.ruleForm.username);
                        // this.$router.push('/');
                        var that = this;
                        login(this.ruleForm)
                            .then((response) => {
                                // console.log(response);
                                //本地存储用户信息
                                // cookie.setCookie('username', response.data.username, 1);
                                // cookie.setCookie('user_id', response.data.user_id, 1);
                                // cookie.setCookie('token', response.data.token, 1);

                                // 使用浏览器本地存储保存token
                                if (that.remember_me) {
                                    // 记住登录
                                    sessionStorage.clear();
                                    localStorage.token = response.data.token;
                                    localStorage.user_id = response.data.user_id;
                                    localStorage.username = response.data.username;
                                } else {
                                    // 未记住登录
                                    localStorage.clear();
                                    sessionStorage.token = response.data.token;
                                    sessionStorage.user_id = response.data.user_id;
                                    sessionStorage.username = response.data.username;
                                }

                                that.$router.push({ name: 'index' });
                            })
                            .catch(error => {
                                // console.log(error);
                                // if("non_field_errors" in error && error.status_code === 400) {
                                if ('non_field_errors' in error) {
                                    // that.err_msg = error.non_field_errors[0];
                                    that.err_msg = '用户名或密码错误';
                                }

                                if (error.response) {
                                    that.err_msg = '服务器异常';
                                } else if (error.request) {
                                    // that.err_msg = error.message;
                                    that.err_msg = '网络异常';

                                }
                                that.err_info = true;
                            });

                    } else {
                        // console.log('error submit!!');
                        this.err_msg = '参数有误';
                        this.err_info = true;
                        return false;
                    }
                });
            },
            clearValidate(prop_value) {
                this.$refs['ruleForm'].clearValidate(prop_value);
            }
        }}


</script>

<style scoped>
    .homeBox {
        position: fixed;
        width: 100%;
        height: 100%;
        top: 0px;
        background-color: #191c2c;
    }
    .img-login {
        margin-top: -35%;
        width: 100%;
        height: auto;
    }

    .title0 {
        position: absolute;
        top: 10%;
        left: -41px;
        width: 100%;
        text-align: center;
        color: #2ec0f6;
        font-size: 40px;
        height: 70px;
        line-height: 70px;
        /*<!--margin: -300px 0 0 0;-->*/
        z-index: 1000;
    }

    .title1 {
        position: absolute;
        top: 16%;
        left: -41px;
        width: 100%;
        text-align: center;
        color: #eaeaea;
        font-size: 20px;
        height: 70px;
        line-height: 70px;
        /*<!--margin: -300px 0 0 0;-->*/
        z-index: 1000;
        margin-top: 25px;
    }

    .tyg-div {
        color: #2ec0f6;
        z-index: -1000;
        float: left;
        position: absolute;
        left: 5%;
        top: 20%;
        font-size: 30px;
        list-style-type: none
    }

    .lun-container {
        width: 210px;
        height: 140px;
        position: relative;
        font-size: 32px;
        color: #FFFFFF;
        text-align: center;
        line-height: 90px;
        margin: 200px auto;
        margin-bottom: 0px;
        margin-top: 48%;
        perspective: 1000px;
        z-index: 1000;
    }

    .carouse {
        transform-style: preserve-3d;

    }

    .carouse div {
        display: block;
        position: absolute;
        width: 140px;
        height: 90px;
    }

    .carouse .pic1 {
        transform: rotateY(0deg) translateZ(160px);
    }

    .carouse .pic2 {
        transform: rotateY(120deg) translateZ(160px);
    }

    .carouse .pic3 {
        transform: rotateY(240deg) translateZ(160px);
    }


    @keyframes to-scroll1 {
        0% {
            transform: rotateY(0deg);
        }

        33% {
            transform: rotateY(-120deg);

        }
        66% {
            transform: rotateY(-240deg);

        }
        100% {
            transform: rotateY(-360deg);

        }

    }

    .carouse {
        animation: to-scroll1 10s ease infinite;
        /*animation-fill-mode: both;*/
    }
    .ms-title{
        width:100%;
        line-height: 50px;
        text-align: center;
        font-size:25px;
        /* color: rgb(43, 155, 33); */
        color: #E4E7ED;
        border-bottom: 1px solid #ddd;
    }
    .ms-login{
        position: absolute;
        left:50%;
        top:50%;
        width:350px;
        margin:-100px 0 100px 380px;
        border-radius: 5px;
        background: rgba(255,255,255, 0.3);
        overflow: hidden;
    }
    .ms-content{
        padding: 30px 30px;
    }
    .login-btn{
        text-align: center;
    }
    .login-btn button{
        width:100%;
        height:36px;
        margin-bottom: 10px;
    }
    .login-tips{
        font-size:12px;
        line-height:30px;
        color:#F56C6C;
    }
    .register_link{
        padding-left: 70px;
        /* color: #E4E7ED; */
    }
    .remember_me >>> .el-form-item__label {
        /* color: #409EFF; */
        color: #E4E7ED;
    }

</style>
