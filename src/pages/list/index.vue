<template>
  <div>
    <div class="card" v-for="(item, index) in items" :key='index'>
      <div @click='onItemClick(item.url)'>
        <image class="image" mode="aspectFill" :src="item" />
        <div class="title">{{item.title}}</div>
      </div>
    </div>
  </div>
</template>

<script>
import Constant from '@/utils/constant.js'
export default {
  data() {
    return {
      items: [],
      hidden: false,
      mCurrentPage: 0,
      mUrl: [],
      mDesc: [],
      mWho: [],
      mTimes: [],
      mTitles: [],
    }
  },
  mounted() {
    this.init(this.mCurrentPage + 1)
  },
  onReachBottom() {
    this.hidden = false
    this.init(this.mCurrentPage + 1)
  },
  methods: {
    init(targetPage) {
      const that = this

      wx.showToast({
        title: '加载中',
        icon: 'loading'
      });

      wx.request({
        // url: Constant.GET_MEIZHI_URL + targetPage,
        url: 'http://localhost:3000',
        header: {
          "Content-Type": "application/json"
        },
        success: function (res) {
          that.items = res.data.data.list
          wx.hideToast();
        }
      });

    },

    onItemClick(url) {
      const that = this
      wx.previewImage({
        current: url,
        urls: that.mUrl
      })
    },
  },
}
</script>

<style scoped>
.card {
  border: 2px solid #ffffff;
  border-radius: 5px;
  background-color: #ffffff;
  box-shadow: 0px 5px 1px #cccccc;
  margin: 8px;
  position: relative;
}

.loadmore {
  border: 0px solid #ffffff;
  border-radius: 5px;
  background-color: #ffffff;
  box-shadow: 0px 5px 1px #cccccc;
  margin: 8px;
}

.image {
  width: 100%;
  height: 240px;
}

.title {
  padding: 14px;
  font-size: 14px;
}
</style>
