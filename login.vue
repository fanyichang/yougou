<template>
  <div>
    <div id="contain">
        <div class="d1">
            <table>
                <tr>
                    <td class="t1">
                        <router-link to="login">账户密码登录</router-link>
                    </td>
                    <td class="t2" align="right">
                        <router-link to="register">立即注册账户</router-link>
                    </td>
                </tr>
                <tr>
                    <td class="t3" colspan="2">
                        <input @blur="checkUname" id="uname" type="text" v-model="uname" placeholder="请输入用户名" class="uname">
                        <span class="text" v-html="texts"></span>
                    </td>
                </tr>
                <tr>
                    <td class="t4"  colspan="2">
                        <input @blur="checkPwd" type="password" v-model="upwd" placeholder="请输入密码" class="upwd">
                        <span class="pwd" v-html="pwds"></span>
                    </td>
                </tr>
                <tr>
                    <td id="t5" colspan="2" align="right">
                        <a href="javascript:;">忘记密码？</a>
                    </td>
                </tr>
                <tr>
                    <td id="t6" colspan="2">
                        <button @click="mylogin">立即登录</button>
                    </td>
                </tr>
                <tr >
                    <td id="t7" colspan="2">
                        <h6>使用合作网站登录优购</h6>
                    </td>
                </tr>
                <tr >
                    <td id="t8" colspan="2">
                        <!-- <div class="p1" class="weixin"><a href="" ><img src="./img/weixin.png" alt=""></a></div>
                        <div class="p2" class="QQ"><a href="" ><img src="./img/QQ.png" alt=""></a></div>
                        <div class="p3" class="weibo"><a href="" ><img src="./img/weibo.png" alt=""></a></div>
                        <div class="p4" class="zfb"><a href="" ><img src="./img/zfb.png" alt=""></a></div> -->
                        <ul>
                            <li><a href="#wx" class="weixin"><img src="../assets/imgs/login_imgs/weixin.png" alt=""></a></li>
                            <li><a href="javascript:;" class="QQ"><img src="../assets/imgs/login_imgs/QQ.png" alt=""></a></li>
                            <li><a href="javascript:;" class="weibo"><img src="../assets/imgs/login_imgs/weibo.png" alt=""></a></li>
                            <li><a href="javascript:;" class="zfb"><img src="../assets/imgs/login_imgs/zfb.png" alt=""></a></li>
                            <li><a href="javascript:;" class="facebook"><img src="../assets/imgs/login_imgs/Facebook.png" alt=""></a></li>
                        </ul>
                    </td>
                </tr>
            </table>
            <div id="wx" class="sf">
                <button class="close">×</button> 
                <img src="../assets/imgs/login_imgs/weilogin.jpg" alt="">
                <p>请扫码登录</p>
            </div>
        </div>
    </div>
    <div class="footer">
        <p>Copyright © 1982-2019 Yougou Technology Co., Ltd. 陕ICP备09080608号-4 增值电信业务经营许可证：陕 B2-20080203</p>
    </div>
  </div>
</template>
<script>
import Bus from '../api/Bus'
import {mapState, mapMutations} from "vuex"
import { async } from 'q';
export default {
     computed:{
    ...mapState(["username","code","uid"])
  },
    created(){
        // this.login({//给user
        // uname:this.uname,
        // upwd:this.upwd
    //   })
    },
  data(){
    return {
      uname:"",
      upwd:"",
      texts:"",
      pwds:"",
    }
  },
  methods:{
    //    ...mapActions([//去vuex的actions中取出名为login的函数放到此地
    //   "login"//,"logout","registor"
    // ]),
      checkUname(){
        var reg = /^[a-z0-9]{3,12}$/i;
        var u = this.uname.trim();
        if(!reg.test(u)){
            this.texts=`用户名格式为3-12位字母或数字`
        }else{
            this.texts="";
        }
      },
      checkPwd(){
        var reg = /^[a-z0-9]{3,12}$/i;
        var p = this.upwd.trim();
        if(!reg.test(p)){
            this.pwds=`密码格式为3-12位字母或数字`
        }else{
            this.pwds="";
        }
      },
       ...mapMutations(["setCode","setUname","setUid"]),
    mylogin(){
      //1:创建正则表达式 3~12
      var reg = /^[a-z0-9]{3,12}$/;
      //2:获取用户输入 用户名/密码
      var u = this.uname.trim();
      var p = this.upwd.trim();
      // console.log(u+"_"+p);
      //3:验证用户名如果不匹配提示框
      if(!reg.test(u)){
         return;
      }
      //4:验证密码如果不匹配提示框
      if(!reg.test(p)){
         return;
      }
      //5:发送ajax请求完成登录验证
    //   var url = "login";
        // this.login({//给user
        //             uname:u,
        //             upwd:p
        //         })
    var url = "login"
     var obj = {uname:u,upwd:p};
     this.axios.get(url,{params:obj}).then(res=>{
        //    console.log(res)
        Bus.$emit("code",res.data.code,this.uname);
       if(res.data.code==1){
           this.setCode(1)
           this.setUname(res.data.uname)
           this.setUid(res.data.uid)
        this.$router.push("/")
       }else{
        document.getElementById("uname").focus();
        this.uname="";
        this.upwd="";
        this.texts=`用户名或密码错误!`;
       }
     })
     .catch(err=>{//失败回调
       console.log(err)
     })
    },
  },
  mounted(){
      document.getElementById("uname").focus();
  },
   
};
</script>
<style scoped>
#t8 img{width: 30px;height: 30px;}
#contain{
            width:100%;
            height: 553px;
            background-image: url('http://i2.ygimg.cn/pics/shop/cms/image/cms/2018/06/28/ba839750211e4c6485fe16dea496b709.jpg');
            background-size: cover;
            position: relative;
        }
        .d1{
            width: 378px;height: 450px;
            /* margin-top: 70px;
            margin-left: 1200px; */
            position: absolute;
            bottom: 40px;
            right: 100px;
            background: #fff;
            /*background-image: linear-gradient(to top,lightblue,pink);*/
            border-radius: 5px;
        }
        .d1>table{
            width: 100%;
            border-block-color: pink;
            
        }
        .d1>table a{
            text-decoration: none;
            color: black;
            margin: 30px  20px;
        }
        .t1,.t2{
            margin: 20px 5px;
            font-style:inherit;
        }
        .t2{
            align-content: right;
        }
        .uname, .upwd{
            width: 100%;height: 40px;
            border:1px solid #ccc;
            /*border: 0;*/ outline: 0;
            margin-top: 35px;
            padding-left: 65px;
            background-image: url(../assets/imgs/login_imgs/user.png);
            background-position: 10px center;
            background-repeat: no-repeat;
            box-sizing: border-box;
        }
        .upwd{
            background-image: url(../assets/imgs/login_imgs/pwd.png);
            background-repeat: no-repeat;
        }
        .d1>table button{
            width: 290px;height:40px;
            margin-top: 50px;
            background-color: #000;
            color: #fff;
            font-size: 20px;
            border: 1px solid gray;outline: 0;
            border-radius: 3px;
            margin:30px  40px;
            cursor: pointer;
            box-sizing: border-box;
        }
        .d1>table h6{
            margin-top: 10px;
            text-align: center;
            color: #99999999;
            font-size: 12px;
        }
        .p1,.p2,.p3,.p4{
            width: 60px; height:60px ;
            float: left;
            margin-right: 32px;
            margin-top: 20px;
            box-sizing: border-box;
        }
        .t1 :hover{
            background-color: rgba(0, 0, 0, 0.3);
            color: #fff;
        }
        .t2 :hover{
            background-color: rgba(0, 0, 0, 0.3);
            color: #fff;
        }
        .sf{
            background-color: #fff;
            width:270px;height:350px;
            margin: -335px  -312px;
            display: none;
        }
        #wx :target{
            background-color: #fff;
            width:270px;height:350px;
            margin: -335px  -312px;
            display: block;
        } 
        .sf>button{
            background: rgb(233, 192, 217);
            float:right;
            width:20px;height:20px;
        }
        .sf>img{
            margin: 52px  40px;
        }
        .sf>p{
            text-align: center;
        }
        .show{
            display:block;
        }
        .fade{
            display:none;
        }
        .footer{
            width: 100% ;height: 150px;
            margin: 0  auto;
        }
        .footer>p{
            text-align: center;
            font-size: 12px;
        }
        ul>li{
            float: left;
            list-style: none;
        }
        li+li{
            margin-left: 3px;
        }
        .t3,.t4{
            position: relative;
        }
        .text,.pwd{
            position: absolute;
            bottom: -20px;
            left: 0;
            color: red;
            font-size: 13px;
        }
</style>