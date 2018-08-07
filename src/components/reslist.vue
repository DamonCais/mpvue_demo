<template>
    <div>
        <div v-show="title" class="zan-cell zan-cell--access">
            <div class="zan-cell__bd">{{title}}</div>
            <div class="zan-cell__ft" @click="toUrl">更多</div>
        </div>
        <div class="list">
            <div @click="toDetail(item.detailId)" class="item" v-for="(item,i) in itemList" :key="i">
                <img :src="item.img" class="img" alt="">
                <div class="item-title">{{item.title}}</div>
                <div class="item-subtitle">{{item.subTitle}}</div>
            </div>
        </div>
    </div>

</template>

<script>
export default {
  methods: {
    toDetail(id) {
      console.log(id);
      wx.navigateTo({ url: "../detail/main?id=" + id });
    },
    toUrl() {
      wx.navigateTo({ url: "../list/main?type=" + this.type });
    }
  },

  props: {
    itemList: {
      type: Array
    },
    title: {
      type: String
    },
    type: {
      type: String
    }
  }
};
</script>

<style lang="scss" scoped>
.list {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  .item {
    flex-basis: 33.33%;
    padding: 3px;
    box-sizing: border-box;
    position: relative;
    .img {
      width: 100%;
      height: 300rpx;
    }
    .item-title {
      text-align: center;
      font-size: 14px;
      line-height: 16px;
      height: 32px;
      padding: 0 5px;
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 2;
      overflow: hidden;
      word-wrap: break-word;
    }
    .item-subtitle {
      position: absolute;
      top: 3px;
      left: 3px;
      right: 3px;
      line-height: 16px;
      height: 32px;
      padding: 0 5px;
      text-align: center;

      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 2;
      overflow: hidden;
      word-wrap: break-word;
      font-size: 12px;
      background: #999;
      opacity: 0.7;
    }
  }
}
</style>
