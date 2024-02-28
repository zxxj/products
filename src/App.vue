<script setup>
import { QRCode } from "ant-design-vue";
import { Button, Input, message, Card} from "ant-design-vue"
import { ref } from "vue";

const isActive = ref(1)
const loading = ref(false)
const tipLoading = ref(false)
const loadingZ = ref(false)
const flag = ref(false)
const qrcode = ref(false)
const imgs = ref(['/cod1.png', '/cod2.png'])
const currentIndex = ref(0)
const title = ref('提示')
const title2 = ref("微信")

const click = (index) => {
  isActive.value =index
}

const btnClick = (index) => {
  loading.value = true
  if(isActive.value === 1) title2.value = '微信'
  if(isActive.value === 2) title2.value = '支付宝'
  if(isActive.value === 3) title2.value = '花呗'

  setTimeout(() => {
    tipLoading.value = true
  }, 1000)
}

const reload = () => {
  location.reload()
}
const tipClose = () => {
  setTimeout(() => {
    loading.value = false
    tipLoading.value = false
    message
    .loading({
        content: '正在充值中，请稍等',
        duration: 2.5,
        style: {
          marginTop: '45vh',
        }
      })
    .then(
      () => message.success({
        content: '充值成功',
        duration: 2.5,
        style: {
          marginTop: '45vh',
        }
        },
      ),
      () => {},
      isActive.value = 1,
    )

    .then(() => reload())
  },2000)
}

const imgchange = () => {
  setTimeout(() => {
    currentIndex.value = 1
    tipClose()
  },2000)
}
const tipBtnClick = () => {
  loadingZ.value = true
  isActive.value = null
  title.value = '支付'

  setTimeout(() => {
    loadingZ.value = false
    flag.value = true
    qrcode.value = true

    imgchange()
  },1000)
}
</script>

<template>


  <div class="box">
    <div class="loader" :class="loadingZ === true ? 'z999' : 'z0'" v-if="loading"></div>
    <div class="wenzi" v-if="loading">数据加载中</div>
    <div class="card">
      <div class="top">
        <Input default-value="充值中心" style="width: 160px; background-color: #eae8eb; font-size: 30px; border: unset; font-weight: bold;"/>
      </div>
      <div class="bottom">
        <div class="item">
          <div class="left">
            应付金额
          </div>
          <div class="right">
            <Input default-value="1888888.0元" style="color: red; font-size: 20px;"/>
          </div>
        </div>

        <div class="item">
          <div class="left">
            角色昵称
          </div>
          <div class="right">
            <Input default-value="" style="color: red; font-size: 20px;"/>
          </div>
        </div>
      </div>

        <!-- 支付方式 -->
        <div class="pay">
          <div class="title">支付方式</div>
          <div class="wx img" @click="click(1)">
            <img  src="./assets/wx.jpg" alt="">
            <div v-if="isActive === 1" class="active">
              <div class="s"></div>
            </div>
          </div>
          <div class="zfb img"  @click="click(2)">
            <img src="./assets/zfb.webp" alt="">
            <div v-if="isActive === 2" class="active">
              <div class="s"></div>
            </div>
          </div>
          <div class="hb img"  @click="click(3)">
            <img src="./assets/hb.webp" alt="">
            <div v-if="isActive === 3" class="active">
              <div class="s"></div>
            </div>
          </div>
          <Button class="btn" @click="btnClick">确认支付</Button>
        </div>


      <div class="tishikapian" v-if="tipLoading">
        <Card :title="title" style="width: 100%; height: 100%;">
          <template #extra><a style="position: relative; z-index: 100;" href="#" @click="tipClose">X</a></template>
          <p v-if="isActive === 1" style="font-size: 25px;  ">在您的设备上未检测到微信，是否使用二维码进行支付？</p>
          <p v-if="isActive === 2" style="font-size: 25px;  ">在您的设备上未检测到支付宝，是否使用二维码进行支付？</p>
          <p v-if="isActive === 3" style="font-size: 25px;  ">在您的设备上未检测到花呗，是否使用二维码进行支付？</p>

          <p v-if="loadingZ === false && flag" style="margin-bottom: 100px;">
            <img :src="imgs[currentIndex]" style="width: 300px; height: 300px;">
            <!-- <QRCode value="http://www.antdv.com" size="350" status="loading" /> -->
          </p>
          <p v-if="loadingZ === false && flag" style="position: absolute; bottom:100px; font-size: 25px;">请使用手机{{title2}}的“扫一扫”功能进行扫码支付</p>
          <Button v-if="isActive" style="position: absolute; bottom:30px; right: 40px; background-color: #eccc6b; width: 150px; height: 50px;" @click="tipBtnClick">确认</Button>
        </Card>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.z0 {
  z-index: 0;
}

.z999 {
  z-index: 999;
}

.wenzi {
  position: absolute;
  font-size: 16px;
  font-weight: bold;
}
.loader {
  position: absolute;
  --d:60px;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  color: #25b09b;
  box-shadow: 
    calc(1*var(--d))      calc(0*var(--d))     0 0,
    calc(0.707*var(--d))  calc(0.707*var(--d)) 0 1px,
    calc(0*var(--d))      calc(1*var(--d))     0 2px,
    calc(-0.707*var(--d)) calc(0.707*var(--d)) 0 3px,
    calc(-1*var(--d))     calc(0*var(--d))     0 4px,
    calc(-0.707*var(--d)) calc(-0.707*var(--d))0 5px,
    calc(0*var(--d))      calc(-1*var(--d))    0 6px;
  animation: l27 1s infinite steps(8);
}
@keyframes l27 {
  100% {transform: rotate(1turn)}
}
.box {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
  box-sizing: border-box;
  background-color: #f5f5f5;

  .card {
    display: flex;
    flex-direction: column;
    width: 900px;
    height: 600px;
    .top {
      height: 80px;
      background-color: #eae8eb;
      text-align: center;
      line-height: 80px;
      font-size: 30px;
      border-top-left-radius: 20px;
      border-top-right-radius: 20px;
      font-weight: bold;
    }

    .bottom {
      padding: 40px;
      flex: 1;
      background-color:#fff;

      .item {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 25px;
        border-bottom: 2px solid #eee;
        padding: 15px 0;
      }
    }

    .pay {
        display: flex;
        justify-content: space-between;
        align-items: center;
        background-color: #eae8eb;
        height: 200px;
        border-bottom-left-radius: 20px;
        border-bottom-right-radius: 20px;
        padding-left: 40px;
        .title {
          font-size: 20px;
        }

        .btn {
          width: 150px;
          height: 50px;
          margin-right: 40px;
          background-color: #eccc6b;
        }

        .img {
          cursor: pointer;
          position: relative;
          width: 130px;
          height: 100px;
          border-radius: 5px;
          img {
            width: 100%;
            height: 100%;
            border-radius: 5px;
          }
        }
      }

  .tishikapian {
    position: absolute;
    width: 900px;
    height: 600px;
  }
  }


}

.active {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  border-radius: 5px;

  .s {
    position: absolute;
    bottom: 0;
    right: 0;
    width: 0;
    height: 0;
    border: 10px solid;
    border-color: rgba(0, 0, 0, 0) green green rgba(0, 0, 0, 0);
    border-radius: 5px;
  }
}
:deep(.ant-card-head-title) {
  text-align: center !important;
  font-size: 30px !important;
}

:deep(.ant-card-extra) {
  text-align: center !important;
  font-size: 20px !important;
  a {
    color: gray !important;
  }
}

:deep(.ant-card-body) {
  position: relative;
  margin-top: -50px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
}
</style>
