<template>
    <div class="left-nav">
        <ul>
            <li>
                <i class="iconfont iconmeishi"></i>  
                <div>商品</div>              
            </li>
            <li>
                <i class="iconfont iconshop"></i>
                <div>店铺</div>                              
            </li>
            <li>
                <i class="iconfont iconsettings"></i> 
                <div>设置</div>                             
            </li>
            <li>
                <i class="iconfont iconpic"></i>
                <div>会员</div>                              
            </li>
            <li>
                <i class="iconfont iconsponsor"></i> 
                <div>统计</div>                             
            </li>
        </ul>
    </div>
</template>
<script>
export default {
   name:"leftNav" 
}
</script>

<style>
.left-nav{
    color:#fff;
    font-size:10px;
    height:100%;
    background-color: #1D8CE0;
    float:left;
    width:5%;
}
.iconfont{
    font-size: 24px;
}
.left-nav ul{
    padding: 0px;
    margin:0px;
}
.left-nav li{
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #6daff1;
    padding: 10px;
    margin:20px 0px;
    padding:20px 0px;    
}
</style>