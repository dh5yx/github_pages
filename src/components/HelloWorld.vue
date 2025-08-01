<template>
  <div class="hello">
    <button @click="readIcCard">readIcCard读卡</button>
    <div class="order-item" v-for="item in 6" :key="item">
      <!-- 订单顶部 -->
      <div class="order-header">
        <img src="../assets/shop-logo.png" alt="" class="shop-logo" />
        <p class="shop-name">会员购123</p>
        <p class="order-text">已付定金￥14.13元，预计2021.02出荷</p>
      </div>
      <!-- 订单内容 -->
      <div class="content-wrapper">
        <!-- 订单图片 -->
        <div class="item-img">
          <img src="../assets/fufu.png" alt="" class="item-true-img" />
        </div>
        <!-- 订单右侧内容 -->
        <div class="item-content-right">
          <div class="item-name">
            世嘉 VOCALOID 初音未来 雪未来 SNOW MIKU 2012 毛绒 再版 哔哩哔哩独家
          </div>
          <div class="item-skus">雪未来 毛绒</div>
        </div>
      </div>
      <!-- 订单总计 -->
      <div class="total">
        <span class="total-span">一件商品,合计:</span>
        <span class="total-span">￥</span>
        <span class="price">89.00</span>
      </div>
      <!-- 评分 -->
      <div class="mirai-rate">
        <van-rate
          v-model="value"
          :size="18"
          color="#ffd21e"
          void-icon="star"
          allow-half
          void-color="#eee"
        />
      </div>
      <!-- 滑块 -->
      <el-slider v-model="value2"></el-slider>

      <!-- 表格 -->
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column prop="date" label="日期"> </el-table-column>
        <el-table-column prop="name" label="姓名"> </el-table-column>
        <el-table-column prop="address" label="地址"> </el-table-column>
      </el-table>
    </div>
    <!-- 提交订单 -->
    <van-submit-bar :price="3050" button-text="提交订单" style="z-index: 9999">
      <van-checkbox v-model="checked">全选</van-checkbox>
      <template #tip>
        你的收货地址不支持同城送,
        <span>修改地址</span>
      </template>
    </van-submit-bar>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      value: 4.5,
      value2: 60,
      tableData: [
        {
          date: "2016-05-02",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-04",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1517 弄",
        },
        {
          date: "2016-05-01",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1519 弄",
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
        },
      ],
      checked: true,
    };
  },
  methods: {
    //读卡操作
    readIcCard() {
      try {
        //alert('测试');
        let iport = "100"; //读卡器端口，读卡器端口，USB1--100,COM1--0,COM2--1，一般默认为100，打开页面时从后台获取
        let iBaud = "9600"; //波特率，一般默认为9600，打开页面时从后台获取
        let inpara = iport + "|" + iBaud;
        //创建 XMLHttpRequest 对象
        let xhr = new XMLHttpRequest();
        // let url = 'http://localhost:18080/readIcCard';
        let url = "http://localhost:9999/users";
        xhr.open("post", url, true);

        // xhr.setRequestHeader('Content-Type', 'application/text');
        // xhr.setRequestHeader('X-Requested-With', 'XMLHttpRequest');
        // xhr.setRequestHeader('auth', '123');
        xhr.onreadystatechange = function () {
          if (
            xhr.readyState === 4 &&
            xhr.status === 200 &&
            xhr.responseText != "9999"
          ) {
            console.log(xhr.responseText);
            //读卡返回信息，可以用该数据验证测试功能
            //var rcardstr=0|68010027028300000000990101000002000050000000000000000100000100025036680900201007828802D816|0000271000000009|680100FC0000522400005224000052240000522400005224000052240000522400005224000052240000522400005224000052240000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008516|680100FC00005224000052240000522400005224000052240000522400005224000052240000522400005224000052240000522400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
            var rcardstr = xhr.responseText;
            var rcararr = rcardstr.split("|");
            var rkcode = rcararr[0];
            var file1 = rcararr[1]; //读卡成功后发送后台，参数信息文件
            var file2 = rcararr[2]; //读卡成功后发送后台，钱包文件
            var file3 = rcararr[3]; //读卡成功后发送后台，第一套电价文件
            var file4 = rcararr[4]; //读卡成功后发送后台，第二套电价文件
            var file5 = rcararr[5]; //读卡成功后发送后台，电表返写信息文件
            console.log(file1);
            console.log(file2);
            console.log(file3);
            console.log(file4);
            console.log(file5);
            /*调用后端服务，将5个文件（字符串）发送给后台解析，并返回用户信息、写卡信息和购电记录*/
          }
        };
        //为准备发送做准备将要发送的数据作为参数传递给 send() 方法。
        //处理响应当 xhr.readyState === 4 并且 xhr.status === 200 时，请求已成功。使用 xhr.responseText 获取响应数据。
        xhr.send(inpara);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.hello {
  .order-item {
    padding: 0 12px 12px;
    box-sizing: border-box;
    color: #212121;
    margin-top: 10px;
    background: #fff;
    // 订单顶部
    .order-header {
      display: flex;
      align-items: center;
      font-size: 12px;
      height: 36px;
      border-bottom: 1px solid #e7e7e7;
      box-sizing: border-box;
      .shop-logo {
        width: 16px;
        height: 16px;
        margin-right: 6px;
      }
      .shop-name {
        position: relative;
        height: 36px;
        display: flex;
        align-items: center;
        &::before {
          content: " ";
          display: block;
          position: absolute;
          right: -20px;
          width: 12px;
          height: 24px;
          background-image: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNiIgaGVpZ2h0PSIxMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cGF0aCBzdHJva2Utd2lkdGg9IjEuNSIgZD0iTTEgMWw0IDQtNCA0IiBzdHJva2U9IiNDQ0MiIGZpbGw9Im5vbmUiIGZpbGwtcnVsZT0iZXZlbm9kZCIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIiBzdHJva2UtbGluZWpvaW49InJvdW5kIi8+PC9zdmc+);
          background-repeat: no-repeat;
          background-position: 50%;
          background-size: 50%;
        }
      }
      .order-text {
        margin-left: auto;
      }
    }
    // 订单内容
    .content-wrapper {
      padding: 10px 0;
      display: flex;

      // 订单图片
      .item-img {
        width: 80px;
        height: 80px;
        position: relative;
        display: flex;
        align-items: center;
        background: #eee;
        overflow: hidden;
        border-radius: 4px;
        .item-true-img {
          position: absolute;
          top: 50%;
          left: 50%;
          width: 100%;
          min-height: 100%;
          object-fit: cover;
          transform: perspective(1px) translate(-50%, -50%);
        }
      }
      // 订单右侧内容
      .item-content-right {
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        margin-left: 10px;
        .item-name {
          font-size: 14px;
        }
        .item-skus {
          font-size: 12px;
          color: #999;
        }
      }
    }
    // 订单总计
    .total {
      text-align: right;
      .total-span {
        font-size: 12px;
      }
      .price {
        font-weight: bold;
      }
    }
  }
}
</style>
