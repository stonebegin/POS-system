<template>
  <div class="pos">
    <el-row>
      <el-col :span='7' class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width: 100%">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量" width="50"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <template scope="scope">
                  <el-button type="text" size="small" @click="deleteSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="totalDiv">
              <small>数量：</small>{{totalCount}} <small>金额：</small>¥{{totalMoney}}元
            </div>
            <div class="div-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="deleteAllGoods()">删除</el-button>
              <el-button type="success" @click="checkout()">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">
          </el-tab-pane>
          <el-tab-pane label="外卖">
          </el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="17">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="goods in oftenGoods" :key="goods.goodsId" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">¥{{goods.price}}</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class="cooklist">
                  <li v-for="goods in type0Goods" :key="goods.goodsId" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">{{goods.price}}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <ul class="cooklist">
                <li v-for="goods in type1Goods" :key="goods.goodsId" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">{{goods.price}}</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <ul class="cooklist">
                <li v-for="goods in type2Goods" :key="goods.goodsId" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">{{goods.price}}</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <ul class="cooklist">
                <li v-for="goods in type3Goods" :key="goods.goodsId" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">{{goods.price}}</span>
                </li>
              </ul>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Pos',
  data: function () {
    return {
      totalCount: 0,
      totalMoney: 0,
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: []
    }
  },
  created: function () {
    axios.get('http://jspang.com/DemoApi/oftenGoods.php')
      .then(response => {
        console.log('Response: ', response)
        this.oftenGoods = response.data
      })
      .catch(error => {
        console.log('Error: ', error)
        alert('遇到错误，未能获取数据！')
      })

    axios.get('http://jspang.com/DemoApi/typeGoods.php')
      .then(response => {
        console.log(response)
        this.type0Goods = response.data[0]
        this.type1Goods = response.data[1]
        this.type2Goods = response.data[2]
        this.type3Goods = response.data[3]
      })
      .catch(error => {
        console.log('Error: ', error)
        alert('遇到错误，未能获取到数据! ')
      })
  },
  mounted: function () {
    var orderHeight = document.body.clientHeight
    console.log(orderHeight)
    document.getElementById('order-list').style.height = orderHeight + 'px'
  },
  methods: {
    addOrderList: function (goods) {
      this.totalCount = 0
      this.totalMoney = 0
      // 添加进去的商品是否已经存在
      let isHave = false
      for (let i = 0; i < this.tableData.length; ++i) {
        if (goods.goodsId === this.tableData[i].goodsId) {
          isHave = true
        }
      }
      // 根据isHave来写业务逻辑
      if (isHave) {
        let arr = this.tableData.filter(o => o.goodsId === goods.goodsId)
        arr[0].count++
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        }
        this.tableData.push(newGoods)
      }
      this.getAllMoney()
    },
    deleteSingleGoods: function (goods) {
      if (goods.count === 1) {
        this.tableData.pop()
      } else {
        goods.count--
      }
      // this.totalCount--
      // this.totalMoney -= goods.price
      // this.tableData = this.tableData.filter(o => goods.goodsId !== o.goodsId)
      this.getAllMoney()
    },
    getAllMoney: function () {
      this.totalCount = 0
      this.totalMoney = 0
      if (this.tableData) {
        this.tableData.forEach((element) => {
          this.totalCount += element.count
          this.totalMoney += element.count * element.price
        })
      }
    },
    deleteAllGoods: function () {
      this.tableData = []
      this.totalCount = 0
      this.totalMoney = 0
    },
    // 缺省post结账请求，模拟结账
    checkout: function () {
      if (this.totalCount !== 0) {
        this.tableData = []
        this.totalCount = 0
        this.totalMoney = 0
        this.$message({
          message: '结账成功，会尽快为您制作',
          type: 'success'
        })
      } else {
        this.$message.error('订单为空，请先下单！')
      }
    }
  }
}
</script>
<style scoped>
.pos-order{
  background-color: #F9FaFc;
  border-right: 1px solid #C0CCDA;
  /* margin-left: 8px; */
}
.div-btn{
  margin-top: 10px;
}
.title{
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
  text-align: left;
}
.often-goods-list ul li{
  list-style: none;
  float: left;
  border: 1px solid #F9FaFc;
  padding: 10px;
  margin: 10px;
  background-color:rgb(188, 238, 221);
  cursor: pointer;
}
.o-price{
  color: cornflowerblue;
}
.goods-type{
  clear: both;
}
.cooklist li{
  list-style: none;
  width: 23%;
  border: 1px solid #E5E9F2;
  height: auto;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  cursor: pointer;
}
.cooklist li span{
  display: block;
  float: left;
}
.foodImg{
  width: 40%;
}
.foodName{
  font-size: 16px;
  padding-left: 10px;
  color: brown;
}
.foodPrice{
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.totalDiv{
  background-color: #fff;
  border-bottom: 1px solid #E5E9F2;
}
</style>
