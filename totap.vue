<template>
    <div class="toTop" @click="topShow">
        <a href="javascript:;"><img v-show="isShow" src="../assets/imgs/top.png" alt=""><img></a>
    </div>
</template>
<script>
export default {
    data(){
        return {
            isShow:false,
        }
    },
    // scroll  滚动窗口至文档中的特定位置。
    mounted(){ window.addEventListener('scroll',this.handleScroll,true)  },
    destroyed() {
    window.removeEventListener('scroll', this.handleScroll); //离开页面需要移除这个监听的事件
    },
    methods:{
        handleScroll(){
            // 要获取当前页面的滚动条纵坐标位置，用：document.documentElement.scrollTop;
            var scrollTop=document.documentElement.scrollTop || document.body.scrollTop;
            // console.log(scrollTop)
            if(scrollTop>=500){
                this.isShow=true;
            }else{
                this.isShow=false;
            }
        },
        topShow(){
            var  scrollTop=document.documentElement.scrollTop || document.body.scrollTop;
            var  timer = setInterval(() => {
            scrollTop-=50;
            // console.log(scrollTop)
            if (scrollTop < -30) {
                // console.log(scrollTop)
                clearInterval(timer)
                return;
            }
            window.scroll(0,scrollTop)
            //document.documentElement.scrollTop 获取当前页面的滚动条纵坐标位置
            }, 30)

        }
    }
}
</script>
<style scoped>
.toTop{
    position: fixed;
    bottom: 100px;
    right: 0px;
}
</style>