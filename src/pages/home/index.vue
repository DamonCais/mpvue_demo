<template>
  <div>
    <reslist :itemList="moveList" title="电影" type="movie" />
    <reslist :itemList="tvList" title="电视剧" type="tv" />
    <reslist :itemList="varietyList" title="综艺" type="variety" />
    <reslist :itemList="animationList" title="动漫" type="animation" />
  </div>
</template>

<script>
import { post } from "@/api/api";
import reslist from "@/components/reslist";
export default {
  components: {
    reslist
  },
  mounted() {
    this.getMovie();
    this.getTv();
    this.getVariety();
    this.getAnimation();
  },
  data() {
    return {
      itemList: [],
      moveList: [],
      tvList: [],
      varietyList: [],
      animationList: []
    };
  },
  methods: {
    async getMovie() {
      let filter = [
        "剧情片",
        "动作片",
        "喜剧片",
        "爱情片",
        "科幻片",
        "恐怖片",
        "战争片"
      ];
      let page = 0;
      this.moveList = await this.getList(filter, page);
    },
    async getTv() {
      let filter = ["国产剧", "港台剧", "欧美剧", "日韩剧"];
      let page = 0;
      this.tvList = await this.getList(filter, page);
    },
    async getVariety() {
      let filter = ["综艺"];
      let page = 0;
      this.varietyList = await this.getList(filter, page);
    },
    async getAnimation() {
      let filter = ["动漫"];
      let page = 0;
      this.animationList = await this.getList(filter, page);
    },
    getList(filter, page) {
      return new Promise((resolve, reject) => {
        post("/list", { filter, page }).then(res => {
          res.forEach(element => {
            let arr = element.name.split(" ");
            if (arr.length === 2) {
              element.title = arr[0];
            } else {
              element.title = arr.slice(0, arr.length - 1).join(" ");
            }
            element.subTitle = arr[arr.length - 1];
          });
          resolve(res);
        });
      });
    }
  }
};
</script>

<style>
</style>
