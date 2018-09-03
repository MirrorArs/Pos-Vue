<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border>
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量" width="80"></el-table-column>
              <el-table-column prop="price" label="金额" width="80"></el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <template scope="scope">
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="totalDiv">
              <span><small>数量:</small>{{totalCount}}</span>
              <span><small>金额:</small>{{totalMoney}}</span>
            </div>
            <div class="operate-btn" style="margin-top: 16px">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllGoods()">删除</el-button>
              <el-button type="success" @click="checkout()">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">
            挂单
          </el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="17">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="招牌面食">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                    <div class="foodImg"><img :src="goods.goodsImg"></div>
                    <div class="foodInfo"><span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span></div>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="美味米饭">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                    <div class="foodImg"><img :src="goods.goodsImg"></div>
                    <div class="foodInfo"><span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span></div>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="精品砂锅">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                    <div class="foodImg"><img :src="goods.goodsImg"></div>
                    <div class="foodInfo"><span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span></div>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料小吃">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                    <div class="foodImg"><img :src="goods.goodsImg"></div>
                    <div class="foodInfo"><span class="foodName">{{goods.goodsName}}</span>
                      <span class="foodPrice">￥{{goods.price}}元</span></div>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
  name: "pos",
  data() {
    return {
      tableData: [],
      oftenGoods: [],
      type0Goods:[],
      type1Goods:[],
      type2Goods:[],
      type3Goods:[],
      totalMoney:0,
      totalCount:0
    }
  },
  created(){
    axios.get('typeGoods.json')
    .then(response=> {
      this.oftenGoods = response.data.oftenGoods
      this.type0Goods = response.data.typeGoods[0]
      this.type1Goods = response.data.typeGoods[1]
      this.type2Goods = response.data.typeGoods[2]
      this.type3Goods = response.data.typeGoods[3]
    })
  },
  methods:{
    addOrderList(goods){

      let isHave = false;
      for(let i in this.tableData){
        if(this.tableData[i].goodsId == goods.goodsId){
          isHave = true;
        }
      }
      if(isHave){
        let arr = this.tableData.filter(o =>o.goodsId == goods.goodsId);
        arr[0].count++;
      }else{
        let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
        this.tableData.push(newGoods);
      }
      this.allCount()
    },
    delSingleGoods(goods){
      this.tableData = this.tableData.filter(o=>o.goodsId != goods.goodsId)
      this.allCount()
    },
    delAllGoods(){
      this.tableData = [];
      this.totalMoney = 0;
      this.totalCount = 0;
    },
    checkout(){
      if(this.totalCount != 0){
        this.tableData = [];
        this.totalMoney = 0;
        this.totalCount = 0;
        this.$message.success('结账成功')
      }else{
        this.$message.error('不能空结')
      }
    },
    allCount(){
      this.totalMoney = 0;
      this.totalCount = 0;
      if(this.tableData){
        this.tableData.forEach((ele)=>{
          this.totalCount += ele.count;
          this.totalMoney += ele.price*ele.count
        })
      }
    }
  }
}
</script>
<style scoped lang="scss">
.pos {
  height: 100vh;
  .el-row {
    display: flex;
    .pos-order {background: #f9fafc;border-right: 1px solid #c0ccda;height: 100vh;}
  }
}
.totalDiv{background: #FFF;padding: 10px;display: flex;justify-content: space-around;border-bottom: 1px solid #d3dce6}
.often-goods {
  .title {height: 20px;border-bottom: 1px solid #d3dce6;background: #f9fafc;padding: 10px;text-align: left;}
  .often-goods-list {
    ul {display: flex;flex-wrap: wrap;}
    li {margin: 10px;padding: 10px;border: 1px solid #e5e9f2;background: #fff;cursor: pointer}
    .o-price {color: #58b7ff;}
  }
}
.cookList{
  display: flex;flex-wrap: wrap;
  li{display: flex;margin: 12px;padding: 2px;background: #FFF;border:1px solid #E5E9F2;cursor: pointer}
  li .foodImg{display: flex;align-items: center}
  li img{width: 60px;height: 60px;}
  li .foodInfo{display: flex;justify-content: space-between;flex-direction: column;padding: 0 8px;}
  li .foodName{color:brown;max-width: 8em;overflow:hidden;text-overflow:ellipsis;display:-webkit-box;-webkit-box-orient:vertical;-webkit-line-clamp:2; }
  li .foodPrice{text-align: left}
}
</style>
