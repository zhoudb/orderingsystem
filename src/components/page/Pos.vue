<template>
  <div class="pos">
    <el-row>
      <el-col :span="8" id="order-list" class="pos-order">
        <el-tabs stretch>
          <el-tab-pane label="点餐">
            <el-table :data="chooseGoods" boder style="width:100%;">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量" width="50"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="delOrderList(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">添加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="div-total">
              <span>
                <small>数量：</small>
                {{totalNum}}
              </span>
              <span>
                <small>金额：￥</small>
                {{totalPrice}}元
              </span>
            </div>
            <div class="div-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllOrderList">删除</el-button>
              <el-button type="success">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col class="pos-products" :span="16">
        <div class="common-products">
          <div class="title">常用商品</div>
          <div class="common-products-list">
            <ul>
              <li v-for="(goods,index) in commonGoods" :key="index" @click="addOrderList(goods)">
                <span style="display:none;">{{goods.goodsId}}</span>
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs stretch>
            <el-tab-pane label="汉堡">
              <div class="cooklist">
                <ul>
                  <li
                    v-for="(goods,index) in newhamburgerGoods"
                    :key="index"
                    @click="addOrderList(goods)"
                  >
                    <span class="foodImg">
                      <img :src="goods.goodsImg" />
                    </span>
                    <span class="foodName" v-text="goods.goodsName"></span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小吃">小吃</el-tab-pane>
            <el-tab-pane label="饮料">饮料</el-tab-pane>
            <el-tab-pane label="套餐">套餐</el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<<script>
import axios from 'axios';
export default {
    name:'pos',
    data() {
        return {
            chooseGoods:[],
            commonGoods:[
                {
                    goodsId:1,
                    goodsName:'香辣鸡腿堡',
                    price:18
                }, {
                    goodsId:2,
                    goodsName:'田园鸡腿堡',
                    price:15
                }, {
                    goodsId:3,
                    goodsName:'和风汉堡',
                    price:15
                }                
            ],
            hamburgerGoods:[
                {
                    goodsId:1,
                    goodsImg:'hamburger.jpg',
                    goodsName:'香辣鸡腿堡',
                    price:18
                }, {
                    goodsId:2,
                    goodsImg:'hamburger.jpg',
                    goodsName:'田园鸡腿堡',
                    price:15
                }, {
                    goodsId:3,
                    goodsImg:'hamburger.jpg',
                    goodsName:'和风汉堡',
                    price:15
                }, {
                    goodsId:4,
                    goodsImg:'hamburger.jpg',
                    goodsName:'快乐全家桶',
                    price:80
                }                
            ],
            totalNum:0,
            totalPrice:0
        }
    },
    computed: {
        newhamburgerGoods(){
            function getImg(goodsImg){
                return require('@/assets/images/' + goodsImg)
            }
            this.hamburgerGoods.forEach(element => {
                element.goodsImg = getImg(element.goodsImg);
            });
            return this.hamburgerGoods;
        }
    },
    created() {
        // axios.get("https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods")
        // .then(response=>{
        //     console.log(response);
        //     this.commonGoods = response.data;
        // })
        // .catch(error=>{
        //     this.commonGoods = [
        //         {
        //             goodsId:1,
        //             goodsName:'香辣鸡腿堡',
        //             price:18
        //         }, {
        //             goodsId:2,
        //             goodsName:'田园鸡腿堡',
        //             price:15
        //         }, {
        //             goodsId:3,
        //             goodsName:'和风汉堡',
        //             price:15
        //         }
        //     ];
        //     // console.log(error);
        //     // alert('网络错误，不能访问');
        // })
    },
    mounted() {
        var orderHeight = document.body.clientHeight;
        document.getElementById('order-list').style.height = orderHeight + 'px';
    },
    methods:{
        addOrderList(goods){
            //判断商品是否在商品列表中
            let isHave = false;
            for(let i=0;i<this.chooseGoods.length;i++){
                if(this.chooseGoods[i].goodsId == goods.goodsId){
                    isHave = true;
                    break;
                }
            }
            if(isHave){
                let arr = this.chooseGoods.filter(o=>o.goodsId == goods.goodsId);
                // console.log(arr);
                arr[0].count++;
            }else{
                let newGoods = {
                    goodsId:goods.goodsId,
                    goodsName:goods.goodsName,
                    price:goods.price,
                    count:1
                };
                this.chooseGoods.push(newGoods);
            } 
            this.operaTotal();          
        },
        delOrderList(goods){
            var index = this.chooseGoods.findIndex(o=>o.goodsId == goods.goodsId);
            if(index != -1){
                let count = this.chooseGoods[index].count;
                count --;
                if(count == 0){
                    this.chooseGoods.splice(index,1);
                }else{
                    this.chooseGoods[index].count = count;
                }
                this.operaTotal();
            }
        },
        delAllOrderList(){
            this.chooseGoods = [];
            this.totalNum = 0;
            this.totalPrice = 0;
        },
        operaTotal(){
            this.totalNum = 0;
            this.totalPrice = 0;
            if(this.chooseGoods){
                this.chooseGoods.forEach(ele =>{
                    this.totalNum += ele.count;
                    this.totalPrice = this.totalPrice + (ele.price * ele.count);
                })             
            }             
        }
    }    
}
</script>

<style scoped>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.div-btn {
  margin-top: 30px;
}
.title {
  height: 20px;
  border-bottom: 1px solid #03dce6;
  background-color: #f9fafc;
  padding: 10px;
  text-align: left;
}
.common-products {
  height: 200px;
}
.common-products-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e9e5e0;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
  border-radius: 10px;
  cursor: pointer;
}
.o-price {
  color: #58b7ff;
}
.goods-type {
  clear: both;
}
.cooklist ul li {
  list-style: none;
  float: left;
  width: 23%;
  height: auto;
  border: 1px solid #e5e9f2;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  margin: 2px;
  cursor: pointer;
}
.cooklist ul li span {
  font-size: 16px;
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
  border-radius: 10px;
}
.foodImg img {
  width: 100%;
}
.foodName {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.div-total {
  background-color: #fff;
  text-align: right;
  padding: 10px;
  border-bottom: 1px solid #58b7ff;
}
.div-total span {
  margin: 10px;
}
</style>