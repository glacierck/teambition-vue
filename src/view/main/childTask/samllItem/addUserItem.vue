<template>
<section class="add-user-box">
    <h3 class="add-head">创建项目</h3>
    <span 
        class="close-icon"
        @click="closeAddData"
    >
        <Icon type="ios-close-empty" size="40" class="close-btn"></Icon>
    </span>
    <div class="add-img">
        <img src="https://dn-st.teambition.net/teambition/images/illustration@3x.2d8d2cc3.png" alt="">
    </div>
    <div class="add-text">为不同的事务建立各自的项目</div>
    <div class="add-input">
        <Input 
            v-model="title"
            class="inps" 
            placeholder="项目名称(必填)" 
            style="width: 240px" 
            :maxlength="15" 
            :autofocus="true"
        />
        <Input 
            v-model="info"
            class="inps" 
            type="textarea" 
            :autosize="true" 
            placeholder="项目简介(选填)" 
            style="width: 240px"
        />
    </div>
    <div>
        <Button 
            class="add-btn" 
            type="primary"
            @click.stop="addUserItem"
        >完成并创建</Button>
    </div>
</section>
</template>
<script>
export default{
    data(){
        return {
            title:'',
            info:''
        }
    },
    methods:{
        closeAddData(){ // 关闭遮罩层和添加框
            this.$store.commit('addShow',{bl:false})
            this.$store.commit('maskShow',{bl:false})
        },
        addUserItem(){ // 用户添加项目
            if(this.title!==''){
                let id = JSON.parse(localStorage.getItem('userId'));
                this.http.postCreat({id,title:this.title,info:this.info}).then(({data})=>{
                    this.http.getItem({userId:id}).then(({data})=>{
                        let list = [...data.doc];
                        this.$store.commit('getAllData',{list});
                    })
                    this.title = '';
                    this.info = '';
                    this.$store.commit('addShow',{bl:false});
                    this.$store.commit('maskShow',{bl:false});
                })
            }
        }
    }
}
</script>
<style>
.add-user-box{
    position: fixed;
    left: 50%;
    top: 70px;
    width: 300px;
    margin-left: -150px;
    background-color: #fff;
    border-radius: 5px;
    z-index: 99;
    box-shadow: 2px 3px 3px 3px rgb(165, 165, 165);
}
.add-head{
    width: 270px;
    margin-left: 15px;
    font: bold 20px/40px "微软雅黑";
    text-align: center;
    border-bottom: 2px solid rgb(173, 171, 171, .4);
}
.close-icon{
    position: absolute;
    top: 0;
    right: 5px;
    color: rgb(0, 0, 0, 0.7);
    padding: 0 10px;
}
.add-img{
    width: 300px;
}
.add-img img{
    width: 160px;
    margin: 15px 70px;
}
.add-text{
    font: 16px/30px "微软雅黑";
    text-align: center;
}
.add-input .inps{
    margin:5px 30px;
}
.add-btn{
    width: 240px;
    margin: 20px 30px;
}
.close-btn{
    cursor: pointer;
}
</style>
