<template>
  <div>
    <!-- back to top -->
    <div>
      <a-back-top />
      <strong style="color: rgba(64, 64, 64, 0.6)"></strong>
    </div>
    <span style="font-size: 4rem; nargin: 3rem auto 3rem auto;">Classification</span>
    <div>
      <div v-for="item in varData" :key="item[0]" style="display: inline; text-align: left;">
        <a-button type="primary" style="margin: 0.5rem;" @click="clickClass(item[1])">{{ item[1] }}</a-button>
      </div>
    </div>
    <div >
      <!-- <a-radio-group v-model="tabPosition" style="margin:8px">
      <a-radio-button value="top">top</a-radio-button>
      <a-radio-button value="bottom">bottom</a-radio-button>
      <a-radio-button value="left">left</a-radio-button>
      <a-radio-button value="right">right</a-radio-button>
      </a-radio-group>-->

      <div style="max-width: 70rem; margin: 1rem auto 1rem auto;">
        <a-list itemLayout="vertical" size="large" :pagination="pagination" :dataSource="animeList">
          <!-- <div slot="footer">
            <b>ant design vue</b> footer part
          </div>-->
          <a-card
            slot="renderItem"
            slot-scope="item, index"
            key="item[0]"
            hoverable
            @click="toPlay(item[0])"
          >
            <a-list-item-meta :description="item[0]">
              <span slot="title" style="font-size: 3rem;">{{item[2]}}</span>
              <img slot="avatar" width="130" alt="logo" :src="item[1]" />
            </a-list-item-meta>
          </a-card>
        </a-list>
      </div>
    </div>
  </div>
</template>
<style>
.hello {
  text-align: left;
}
</style>
<script>
import $ from "jquery";
import axios from "axios";

const listData = [];
for (let i = 0; i < 11; i++) {
  listData.push({
    title: `Anime ${i}`,
    avatar:
      "https://gw.alipayobjects.com/zos/rmsportal/mqaQswcyDLcXyDKnZfES.png",
    description:
      "Ant Design, a design language for background applications, is refined by Ant UED Team."
  });
}

export default {
  data() {
    return {
      tabPosition: "top",
      listData,
      varData : [
        ["1", "搞笑"],
        ["2", "格斗"],
        ["3", "教育"],
        ["4", "竞技"],
        ["5", "剧情"],
        ["6", "科幻"],
        ["7", "历史"],
        ["8", "励志"],
        ["9", "魔法"],
        ["10", "青春"],
        ["11", "热血"],
        ["12", "社会"],
        ["13", "神魔"],
        ["14", "童话"],
        ["15", "校园"],
        ["16", "战争"],
        ["17", "真人"],
        ["18", "LOLI"]
      ],
      animeList: [],
      pagination: {
        onChange: page => {
          console.log(page);
        },
        pageSize: 10
      }
    };
  },
  methods: {
    callback(val) {
      console.log(val);
    },
    toPlay(playTitle) {
      this.$emit("playList",playTitle);
    },
    clickClass(key) {
      axios.get("http://192.168.1.102:8000/va?varity=" + key).then(response => {
        console.log('send get');
        console.log(response.data);
        console.log(response.data.data);
        this.animeList = response.data.data;
        this.$message.success("Loading finished", 1);
      });
    }
  }
};
</script>