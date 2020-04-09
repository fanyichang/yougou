<template>
  <div class="min">
    <div class="banner" @mouseenter="stop" @mouseleave="start">
      <div class="btn-left" @click="move(1)"></div>
      <ul class="lunbotu" :class="ulClass" style="width: 5950px;" :style="ulStyle">
        <li v-for="(img,i) of imgs" :key="i">
          <router-link to="list">
            <img :src="img.src" alt />
          </router-link>
        </li>
        <li>
          <router-link to="list">
            <img :src="imgs[0].src" alt />
          </router-link>
        </li>
      </ul>
      <ul class="ul-idxs">
        <li v-for="(img,idx) of imgs" :key="idx" :class="idx==i?'active':''" @click="moveTo(idx)"></li>
      </ul>
      <div class="btn-right" @click="move(-1)"></div>
    </div>
  </div>
</template>
<script>
export default {
    data(){
    return {
      innerWidth:1190,
      ulClass:{ hasTrans:true },
      i:0,
      imgs:[
        {
          src:"https://i2.ygimg.cn/pics/shop/cms/home/2019/10/30/89320191030T170713.jpg",
        },
        { 
          src:"http://pic.90sjimg.com/design/00/07/29/69/5666757345e09.jpg",
        },
        { 
          src:"http://img.zcool.cn/community/01a39258eddb07a8012049ef53b617.jpg@1280w_1l_2o_100sh.jpg",
        },
        { 
          src:"https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3886846014,833878687&fm=26&gp=0.jpg",
        },
        
      ],
      canClick:true,
      timer:null
    }
  },
  created(){
    // window.addEventListener("resize",()=>{
    //   this.innerWidth=window.innerWidth;
    // })
    this.start();
  },
  methods:{
    stop(){
      clearInterval(this.timer);
    },
    start(){
      this.timer=setInterval(()=>{
        this.move(1);
      },3000)
    },
    move(i){
      if(this.canClick){
        this.canClick=false;
        if(i==-1&&this.i==0){
          this.ulClass.hasTrans=false;
          setTimeout(()=>{
            this.i=this.imgs.length;
            setTimeout(()=>{
              this.ulClass.hasTrans=true;
              this.i+=i;
              setTimeout(()=>{
                this.canClick=true;
              },200)
            },50)
          },50)
        }else if(i==1&&this.i==this.imgs.length-1){
          this.i+=i;
          setTimeout(()=>{
            this.ulClass.hasTrans=false;
            setTimeout(()=>{
              this.i=0;
              setTimeout(()=>{
                this.ulClass.hasTrans=true;
                setTimeout(()=>{
                  this.canClick=true;
                })
              },50)
            },50)
          },200)
        }else{
          this.i+=i;
          setTimeout(()=>{
            this.canClick=true;
          },300)
        }
      }
    },
    moveTo(i){
      if(this.canClick){
        this.i=i;
        this.canClick=false;
        setTimeout(()=>{
          this.canClick=true;
        },300)
      }
    }
  },
  computed:{
    ulStyle(){
    //   var width=this.innerWidth*(this.imgs.length+1)+"px";
      var marginLeft=-this.i*this.innerWidth+"px";  
      return { marginLeft }
    }
  }

};
</script>
<style scoped>
img {
  width: 1190px;
  height: 400px;
}
.min {
  width: 1190px;
  margin: 0 auto;
}
.banner {
  width: 1190px;
  height: 400px;
  overflow: hidden;
  position: relative;
}
.hasTrans {
  transition: all 0.5s linear;
}
.banner .lunbotu > li {
  float: left;
}
.btn-left,
.btn-right {
  width: 60px;
  height: 100%;
  position: absolute;
  top: 0;
  background-repeat: no-repeat;
  background-position: center;
  cursor: pointer;
}
.btn-left {
  left: 20px;
  background-image: url(../assets/imgs/left_ar.png);
}
.btn-right {
  right: 20px;
  background-image: url(../assets/imgs/right_ar.png);
}
.btn-left:hover,
.btn-right:hover {
  background-size: 40px 70px;
}
.ul-idxs {
  width: 100px;
  margin: 0 auto;
  position: absolute;
  left: 50%;
  margin-left: -50px;
  bottom: 25px;
}
.ul-idxs > li {
  float: left;
  width: 10px;
  height: 10px;
  background-color: #ffffff;
  border-radius: 5px;
  margin: 0 5px;
  cursor: pointer;
}
.ul-idxs > li.active {
  background-color: blue;
}
</style>

