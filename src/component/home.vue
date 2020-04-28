<template>
  <div>
    <!-- back to top -->
    <div>
      <a-back-top />
      <strong style="color: rgba(64, 64, 64, 0.6)"></strong>
    </div>
    <!-- carousel -->
    <a-carousel style="max-width: 80rem; margin: 1rem auto 1rem auto" autoplay>
      <div v-for="item in image" :key="item.imgUrl">
        <img class="scrollImg" :src="item.imgUrl" />
      </div>
    </a-carousel>
    <p style="font-size: 4rem; text-align: start; padding-left: 10rem;">Recommend</p>
    <div style="margin: auto auto 10rem 10rem; border-left: 5px solid red;">
      <div style="max-width: 70rem; margin: 1rem auto 1rem auto;">
        <a-list itemLayout="vertical" size="large" :pagination="pagination" :dataSource="animeList">
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
            <a-list-item-meta :description="item.url">
              <span slot="title" style="font-size: 3rem;">{{item.title}}</span>
              <img slot="avatar" width="130" alt="logo" :src="item.img" />
            </a-list-item-meta>
          </a-card>
        </a-list>
      </div>
    </div>
  </div>
</template>
<style scoped>
/* For demo */
.ant-carousel >>> .slick-slide {
  text-align: center;
  max-height: 30rem;
  line-height: 30rem;
  background: #364d79;
  overflow: hidden;
}

/* .ant-carousel >>> .slick-slide h3 {
  color: #fff;
} */

.scrollImg {
  display: block;
  margin: auto;
  max-width: 130%;
}
</style>
<script>

export default {
  data() {
    return {
      image: [
        { imgUrl: "../../static/img/1.jpg" },
        { imgUrl: "../../static/img/2.jpg" },
        { imgUrl: "../../static/img/3.jpg" },
        { imgUrl: "../../static/img/4.jpg" },
        { imgUrl: "../../static/img/5.jpg" }
      ],
      pagination: {
        onChange: page => {
          console.log(page);
        },
        pageSize: 5
      }
    };
  },
  methods: {
    toPlay() {
      this.$notification.open({
        message: "Notification Title",
        description:
          "This is the content of the notification.",
        onClick: () => {
          console.log("Notification Clicked!");
        },
        duration: 2
      });
      this.$emit("playList");
    }
  },
  props: ['animeList']
};
</script>
