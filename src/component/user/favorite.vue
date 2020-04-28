<template>
  <div>
    <span style="font-size: 4rem; nargin: 3rem auto 3rem auto;">Favorite</span>
    <div style="max-width: 70rem; margin: 1rem auto 1rem auto;">
      <a-list itemLayout="vertical" size="large" :pagination="pagination" :dataSource="listData">
        <!-- <div slot="footer">
            <b>ant design vue</b> footer part
        </div>-->
        <a-card
          slot="renderItem"
          slot-scope="item, index"
          key="item.title"
          hoverable
          @click="toPlay"
        >
          <a-list-item-meta :description="item.description">
            <span slot="title" style="font-size: 3rem;">{{item.title}}</span>
            <img slot="avatar" width="260" alt="logo" :src="item.avatar" />
          </a-list-item-meta>
        </a-card>
      </a-list>
    </div>
  </div>
</template>
<script>
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
      listData,
      pagination: {
        onChange: page => {
          console.log(page);
        },
        pageSize: 5
      }
    };
  },
  methods: {
    callback(val) {
      console.log(val);
    },
    toPlay() {
      this.$emit("playList");
    }
  }
};
</script>