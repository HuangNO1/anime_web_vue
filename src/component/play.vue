<template>
  <div>
    <span style="font-size: 5rem; margin-top: 2rem;">{{Title}}</span>
    <div>
      <div style="margin-top: 2rem;">
        <a-button type="primary" @click="getData()">test</a-button>

        <a-button type="primary" @click="showDrawer()">Select the episode</a-button>
        <a-drawer
          title="Play List"
          placement="right"
          :closable="false"
          @close="onClose"
          :visible="visible"
          width="270"
        >
          <a-list :grid="{ gutter: 1, column: 1 }" :dataSource="playList">
            <a-list-item
              slot="renderItem"
              slot-scope="item, index"
              @click="playMv( item[1],item[0])"
            >
              <a-card>
                <span slot="title">
                  <a-button type="primary"><a-icon type="play-circle" />{{item[0]}}</a-button>
                  
                </span>
              </a-card>
            </a-list-item>
          </a-list>
        </a-drawer>
      </div>
    </div>
    <div style="padding: 1rem; margin: 3rem auto 1rem auto; max-width: 60rem;">
      <video
        id="video"
        class="video-js vjs-default-skin"
        controls
        preload="none"
        poster="../../static/img/video.jpg"
        data-setup="{}"
        v-bind:src="mvUrl"
        style="width: 100%; height: 100%; object-fit: fill"
      >
        <source type="video/mp4" />
      </video>
    </div>
    <div style="margin: 0 auto 10rem auto; font-size: 1rem;">
      <a :href="mvUrl"><a-button type="primary"><a-icon type="download" />Click me to download the video.</a-button></a>
      <!-- <a :href="mvUrl"><a-icon type="download" />Click me to download the video.</a> -->
    </div>
    <div style="padding: 1rem; margin: 3rem auto 1rem auto; max-width: 60rem; height: 60rem;">
      <iframe name = "child" id = "child" src="static/index.html" width="100%" height="100%" frameborder="0" scrolling="no" style="position:related;top: 2.8px;left: 0px;"></iframe>
    </div>
    
  </div>
</template>
<script>
import $ from "jquery";
import axios from "axios";

export default {
  created:function(){
    console.log('created')
    getData()
  },
  data() {
    return {
      playList: {},
      visible: false,
      Title: "第01集",
      mvUrl: ""
    };
  },
  props: {
    playTitle: String,
    required: true
  },
  methods: {
    showDrawer() {
      this.visible = true;
      this.$message.success("Success to open the play list.", 1);
    },
    onClose() {
      this.visible = false;
      this.$message.info("Close the play list.", 1);
    },
    playMv(url, title) {
      // $.cookie('playURL', null);
      this.mvUrl = url;
      console.log(this.mvUrl);
      this.Title = title;
      this.$message.info("Select " + this.Title + ".", 1);
      // $.cookie('playURL', this.mvUrl);

    },
    getData: function() {
      // this.dmurl = this.$route.query.url;

      console.log("getdata"),
        // (this.playList = );
        
      axios
        .get("http://192.168.1.102:8000/viewDm?keyW=" +this.playTitle)
        .then(response => {
          //如果在手机上访问需要更改域名
          console.log(response.data.data);
          this.playList = response.data.data;
          console.log(this.playList)
        });
        this.mvUrl = this.playList[0][1];
        console.log(this.playList)
    }
  },
  mounted() {}
};
</script>
<style>
.Comment {
  margin: 2rem auto 4rem auto;
  max-width: 40rem;
}
</style>