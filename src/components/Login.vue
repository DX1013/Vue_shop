<template>
    <div class="login_container">
        <div class="login_box">
            <!-- 头像区域 -->
            <div class="avatar_box">
                <img src="../assets/logo.png" >
            </div>

            <!-- 表单验证区域 -->
            <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
                <!-- 用户名 -->
                <el-form-item prop="username">
                    <el-input v-model="loginForm.username" prefix-icon="el-icon-user-solid"></el-input>
                </el-form-item>
                <!-- 密码 -->
                <el-form-item prop="password">
                    <el-input type="password" v-model="loginForm.password" prefix-icon="el-icon-lock"></el-input>
                </el-form-item>

                <!-- 按钮区域 -->
            <el-form-item class="btns">
                <el-button type="primary" @click="login">登录</el-button>
                <el-button type="info" @click="resetLoginForm">重置</el-button>
            </el-form-item>
            </el-form>

            
        </div>
        
    </div>
</template>


<script>
export default {
    data(){
        return{
            // 登录表单数据绑定对象
            loginForm:{
                username:'admin',
                password:'123456'
            },
            // 此处为表单验证对象
            loginFormRules:{
                // 检验用户名是否合法
                username:[
                    { required: true, message: '请输入您的用户名', trigger: 'blur' },
                    { min: 1, max: 15, message: '用户名长度为 1 到 15 个字符', trigger: 'blur' }
                ],
                // 检验用户密码是否合法
                password:[
                    { required: true, message: '请输入您的密码', trigger: 'blur' },
                    { min: 6, max: 20, message: '密码长度为 6 到 20 位合法密码', trigger: 'blur' }
                ]
            }
        };
    },
    methods: {
        // 点击重置按钮重置登录表单
        resetLoginForm(){
            // console.log(this);
            this.$refs.loginFormRef.resetFields();
        },
        login(){
            this.$refs.loginFormRef.validate(async valid=>{
                // console.log(valid);
                if(!valid) return;
                const {data:res } = await this.$http.post("login",this.loginForm);
                if(res.meta.status !== 200)return this.$message.error("登录失败");
                this.$message.success("登陆成功");
                // 1.将登陆成功之后的 token 保存于客户端的 sessionStorage 中
                //   1.1 项目中除了登录之外的其他 API 接口，必须在登陆之后才能访问
                //   1.2 token 只应该在当前页面网站打开期间生效，所以将 token 保存在 sessionStorage 中
                window.sessionStorage.setItem("token", res.data.token);
                // 2. 通过编程式导航跳转到后台主页，路由地址是 /home
                this.$router.push("/home");
            });
        }
    }
}
</script>

<style>
.login_container{
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    background-color: #2b4b6b;
}

.login_box{
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50px;
    transform: translate(-50%,50%);
}

.avatar_box{
        width: 130px;
        height: 130px;
        padding: 5px;
        background-color: #fff;
        border: 2px solid #999;
        border-radius: 50%;
        box-shadow: 0 0 10px #999;
        position: absolute;
        left: 50%;
        transform: translate(-50%,-50%);
    
    }

.avatar_box>img{
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;
}

.login_form{
    width: 100%;
    padding: 0 25px;
    box-sizing: border-box;
    position: absolute;
    bottom: 0%;
}

.btns{
    display: flex;
    justify-content: flex-end;
}

</style>
