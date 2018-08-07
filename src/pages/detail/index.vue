<template>
  <div>
    <div>
      <video style="width:750rpx;" :src="src" controls></video>
    </div>
    <h2>选集</h2>
    <div>
      <button @click="switchPlay(i)" v-for="(link,i) in linkList" :key="i" :class="currentIndex===i?'zan-btn--danger':''" class="zan-btn zan-btn--small">{{link.title}}</button>
    </div>
  </div>
</template>

<script>
import { post } from "@/api/api";
export default {
  components: {},

  mounted() {
    this.id = this.$root.$mp.query.id;
    this.linkList = [];
    this.getDetail();
  },
  data() {
    return {
      itemList: [],
      filter: [],
      page: 0,
      id: "",
      detail: {},
      linkList: [],
      src: "",
      currentIndex: 0
    };
  },

  methods: {
    switchPlay(i) {
      this.currentIndex = i;
      this.src = this.linkList[i].src;
    },
    async getDetail() {
      this.detail = await post("/detail", { id: this.id });
      this.detail.linkList.forEach(link => {
        let reg = /\.mp4/;
        if (link.match(reg)) {
          this.linkList.push({
            src: link.split("$")[1],
            title: link.split("$")[0]
          });
        }
      });
      console.log(this.detail);
      if (this.linkList.length) {
        this.src = this.linkList[0].src;
        console.log(this.src);
      }
    }
  }
};
</script>

<style>
</style>
