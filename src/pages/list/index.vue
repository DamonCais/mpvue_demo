<template>
  <div>
    <reslist :itemList="itemList" />
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
    let type = this.$root.$mp.query.type;
    this.itemList = [];
    console.log(type);
    switch (type) {
      case "movie":
        this.filter = [
          "剧情片",
          "动作片",
          "喜剧片",
          "爱情片",
          "科幻片",
          "恐怖片",
          "战争片"
        ];
        break;
      case "tv":
        this.filter = ["国产剧", "港台剧", "欧美剧", "日韩剧"];
        break;
      case "variety":
        this.filter = ["综艺"];
        break;
      case "animation":
        this.filter = ["动漫"];
        break;
    }
    console.log(this.filter);
    this.getList(this.filter, this.page);
    this.page = this.page + 1;
    this.getList(this.filter, this.page);
  },
  data() {
    return {
      itemList: [],
      filter: [],
      page: 0
    };
  },
  onReachBottom() {
    this.page = this.page + 1;
    this.getList(this.filter, this.page);
  },
  methods: {
    async getList(filter, page) {
      let res = await post("/list", { filter, page });
      res.forEach(element => {
        let arr = element.name.split(" ");
        if (arr.length === 2) {
          element.title = arr[0];
        } else {
          element.title = arr.slice(0, arr.length - 1).join(" ");
        }
        element.subTitle = arr[arr.length - 1];
      });
      this.itemList = this.itemList.concat(...res);
    }
  }
};
</script>

<style>
</style>
