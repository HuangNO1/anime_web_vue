<template>
  <a-layout id="components-layout-demo-fixed-sider">
    <!-- layout sider -->
    <a-layout-sider
      :style="{ overflow: 'auto', height: '100vh', position: 'fixed', left: 0 }"
      width="260px"
      :theme="theme"
      collapsible
      v-model="collapsed"
      :trigger="null"
    >
      <!-- icon -->
      <div class="logo" :class="[light ? 'darkText':'whiteText']">
        <span>JK - Alfred</span>
      </div>
      <!-- nav menu -->
      <a-menu
        style="width: 256px"
        :defaultSelectedKeys="['1']"
        :defaultOpenKeys="['1']"
        mode="inline"
        :theme="theme"
        :selectedKeys="[current]"
        @click="handleClick"
      >
        <!-- home -->
        <a-menu-item key="1" @click="homeClick">
          <a-icon type="home" />Home
        </a-menu-item>
        <!-- user -->
        <a-sub-menu key="sub1">
          <span slot="title">
            <a-icon type="user" />
            <span>User</span>
          </span>
          <a-menu-item key="2" @click="infoClick">Information</a-menu-item>
          <a-menu-item key="3" @click="historyClick">History</a-menu-item>
          <a-menu-item key="4" @click="favorClick">Favorite</a-menu-item>
        </a-sub-menu>
        <!-- class -->
        <a-menu-item key="5" @click="classClick">
          <a-icon type="appstore" />Classification
        </a-menu-item>
        <!-- settings -->
        <a-menu-item key="6" @click="settingsClick">
          <a-icon type="setting" />Settings
        </a-menu-item>
        <!-- about -->
        <a-menu-item key="7" @click="aboutClick">
          <span>
            <a-icon type="team" />about
          </span>
        </a-menu-item>
      </a-menu>
      <br />
      <br />
      <!-- theme -->
      <div style="margin-left: 2rem; margin-bottom: 2rem;">
        <a-switch @change="changeTheme" checkedChildren="dark" unCheckedChildren="light" />
      </div>
    </a-layout-sider>
    <a-layout
      :style="{ marginLeft: '260px' }"
      :class="[light ? ['grayBackground','darkText']:['darkBackground','whiteText']]"
    >
      <!-- header -->
      <a-layout-header
        :style="{ padding: 0 }"
        :class="[light ? 'lightBackground':'darkBackground']"
        style="height: 4rem;"
      >
        <a-row type="flex" justify="space-between">
          <a-col>
            <form action>
              <a-input-search
                id="searchIN"
                placeholder="Search"
                style="min-width: 10rem; max-width: 50rem; margin-left: 1rem"
                @search="onSearch"
              />
            </form>
          </a-col>
          <!-- <a-col :span="8"></a-col> -->
          <a-col style="margin-right: 1rem;">
            <a-avatar size="large" icon="user" style />
            <a-button type="primary">
              <router-link to="/login">Sign In</router-link>
            </a-button>
            <a-button type="danger">
              <router-link to="/login">Log Out</router-link>
            </a-button>
          </a-col>
        </a-row>
      </a-layout-header>
      <!-- content -->
      <a-layout-content
        :style="{ margin: '2rem 2rem 0', overflow: 'initial' }"
        :class="[light ? ['lightBackground', 'darkText']:['darkBackground', 'whiteText']]"
      >
        <div :style="{ padding: '0px', textAlign: 'center' }">
          <home v-if="homeShow" @playList="playList" :animeList="animeList"></home>
          <information v-if="infoShow"></information>
          <history v-if="historyShow" @playList="playList"></history>
          <favorite v-if="favorShow" @playList="playList"></favorite>
          <classification v-if="classificationShow" @playList="playList"></classification>
          <settings v-if="settingsShow"></settings>
          <about v-if="aboutShow"></about>
          <play v-if="playShow" :playTitle="playTitle"></play>
          <search v-if="searchShow" @playList="playList" :animeList="animeList"></search>
        </div>
      </a-layout-content>
      <!-- footer -->
      <a-layout-footer
        :style="{ textAlign: 'center' }"
        :class="[light ? ['darkText','grayBackground']:['whiteText', 'darkBackground']]"
        style="height: 5rem"
      >JK ©2019 Created by JK - Alfred</a-layout-footer>
    </a-layout>
  </a-layout>
</template>


<script>
import $ from "jquery";
import axios from "axios";

import home from "@/component/home.vue";
import settings from "@/component/settings.vue";
import about from "@/component/about.vue";
import play from "@/component/play.vue";
import classification from "@/component/classification.vue";
import information from "@/component/user/information.vue";
import history from "@/component/user/history.vue";
import favorite from "@/component/user/favorite.vue";
import search from "@/component/search.vue";

export default {
  data() {
    return {
      current: "1",
      theme: "light",
      light: true,
      collapsed: false,
      homeShow: true,
      settingsShow: false,
      aboutShow: false,
      playShow: false,
      classificationShow: false,
      infoShow: false,
      historyShow: false,
      favorShow: false,
      searchShow: false,
      animeList: {},
      playTitle: ""
    };
  },

  created() {
    this.animeList = [
      [
        "http://www.imomoe.io/view/7226.html",
        "\u78a7\u84dd\u5e7b\u60f3\u7b2c\u4e8c\u5b63",
        "http://p.xiaomingming.org/FileUpload/201910421331123591.jpg"
      ],
      [
        "http://www.imomoe.io/view/7383.html",
        "GrandBlue",
        "http://p.xiaomingming.org/FileUpload/20182281333154023.jpg"
      ],
      [
        "http://www.imomoe.io/view/6297.html",
        "\u866b\u5b7d",
        "http://pic.xiaomingming.org/FileUpload/406.jpg"
      ],
      [
        "http://www.imomoe.io/view/6195.html",
        "\u82cd\u4e4b\u6d41\u661fSPT\u96f7\u5179\u7eb3",
        "http://pic.xiaomingming.org/FileUpload/508.jpg"
      ],
      [
        "http://www.imomoe.io/view/6073.html",
        "\u84dd\u5b9d\u77f3\u4e4b\u8c1c \u91cd\u5236\u7248",
        "http://pic.xiaomingming.org/FileUpload/627.jpg"
      ],
      [
        "http://www.imomoe.io/view/5968.html",
        "\u84dd\u82b1",
        "http://pic.xiaomingming.org/FileUpload/747.jpg"
      ],
      [
        "http://www.imomoe.io/view/5804.html",
        "\u672a\u9ebb\u7684\u90e8\u5c4b",
        "http://pic.xiaomingming.org/FileUpload/906.jpg"
      ],
      [
        "http://www.imomoe.io/view/5466.html",
        "\u5149\u4e0e\u6c34\u7684\u5973\u795e",
        "http://pic.xiaomingming.org/FileUpload/1252.jpg"
      ],
      [
        "http://www.imomoe.io/view/5394.html",
        "\u82cd\u5929\u4e4b\u62f3",
        "http://pic.xiaomingming.org/FileUpload/1341.jpg"
      ],
      [
        "http://www.imomoe.io/view/5155.html",
        "\u5047\u9762\u9a91\u58eb\u8d85\u7ea7\u7535\u738b\u7535\u5f71\u4e09\u90e8\u66f2\uff1a\u84dd\u4e4b\u7ae0",
        "http://pic.xiaomingming.org/FileUpload/1572.jpg"
      ]
    ];
  },
  methods: {
    handleClick(e) {
      console.log("click ", e);
      this.current = e.key;
    },
    // Change theme : dark or light
    changeTheme(checked) {
      this.theme = checked ? "dark" : "light";
      this.light = checked ? false : true;
      if (checked == true) {
        this.$message.success("Dark theme", 0.5);
      } else {
        this.$message.success("light theme", 0.5);
      }
    },
    onSearch() {
      this.homeShow = false;
      this.settingsShow = false;
      this.aboutShow = false;
      this.playShow = false;
      this.classificationShow = false;
      this.infoShow = false;
      this.historyShow = false;
      this.favorShow = false;
      this.searchShow = true;
      var stateObj = { settings: "settings" };
      window.history.pushState(stateObj, "settings", "settings.html");
      this.$message.loading("Searching...", 10);
      var key = document.getElementById("searchIN").value;

      axios
        .get("http://192.168.1.102:8000/search?keyW=" + key)
        .then(response => {
          console.log(response.data);
          console.log(response.data.data);
          this.animeList = response.data.data;
          this.$message.success("Loading finished", 1);
        });
    },
    // component-show
    homeClick() {
      this.homeShow = true;
      this.settingsShow = false;
      this.aboutShow = false;
      this.playShow = false;
      this.classificationShow = false;
      this.infoShow = false;
      this.historyShow = false;
      this.favorShow = false;
      this.searchShow = false;
      var stateObj = { home: "home" };
      window.history.pushState(stateObj, "home", "home.html");
    },
    settingsClick() {
      this.homeShow = false;
      this.settingsShow = true;
      this.aboutShow = false;
      this.playShow = false;
      this.classificationShow = false;
      this.infoShow = false;
      this.historyShow = false;
      this.favorShow = false;
      this.searchShow = false;
      var stateObj = { settings: "settings" };
      window.history.pushState(stateObj, "settings", "settings.html");
    },
    aboutClick() {
      this.homeShow = false;
      this.settingsShow = false;
      this.aboutShow = true;
      this.playShow = false;
      this.classificationShow = false;
      this.infoShow = false;
      this.historyShow = false;
      this.favorShow = false;
      this.searchShow = false;
      var stateObj = { about: "about" };
      window.history.pushState(stateObj, "about", "about.html");
    },
    playList(playTitle) {
      this.homeShow = false;
      this.settingsShow = false;
      this.aboutShow = false;
      this.playShow = true;
      this.classificationShow = false;
      this.infoShow = false;
      this.historyShow = false;
      this.favorShow = false;
      this.searchShow = false;
      this.playTitle = playTitle;
      var stateObj = { play: "play" };
      window.history.pushState(stateObj, "play", "play.html");
    },
    classClick() {
      this.homeShow = false;
      this.settingsShow = false;
      this.aboutShow = false;
      this.playShow = false;
      this.classificationShow = true;
      this.infoShow = false;
      this.historyShow = false;
      this.favorShow = false;
      this.searchShow = false;
      var stateObj = { classification: "classification" };
      window.history.pushState(
        stateObj,
        "classification",
        "classification.html"
      );
    },
    infoClick() {
      this.homeShow = false;
      this.settingsShow = false;
      this.aboutShow = false;
      this.playShow = false;
      this.classificationShow = false;
      this.infoShow = true;
      this.historyShow = false;
      this.favorShow = false;
      this.searchShow = false;
    },
    historyClick() {
      this.homeShow = false;
      this.settingsShow = false;
      this.aboutShow = false;
      this.playShow = false;
      this.classificationShow = false;
      this.infoShow = false;
      this.historyShow = true;
      this.favorShow = false;
      this.searchShow = false;
    },
    favorClick() {
      this.homeShow = false;
      this.settingsShow = false;
      this.aboutShow = false;
      this.playShow = false;
      this.classificationShow = false;
      this.infoShow = false;
      this.historyShow = false;
      this.favorShow = true;
      this.searchShow = false;
    }
  },
  components: {
    home,
    settings,
    about,
    play,
    classification,
    information,
    history,
    favorite,
    search
  }
};
</script>
<style>
#components-layout-demo-fixed-sider .logo {
  height: 32px;
  /* background: rgba(255, 255, 255, 0.2); */
  margin: 16px;
  text-align: center;
  font-size: 1.5rem;
}

.Header {
  text-align: center;
  font-size: 3rem;
  color: black;
  background-color: rgb(255, 255, 255);
  /* position: fixed;
  width: 100%; */
}
#fff .darkText {
  color: black;
}

.whiteText {
  color: #fff;
}

.darkBackground {
  background-color: #162646;
}

.lightBackground {
  background-color: #fff;
  color: black;
}

.grayBackground {
  background-color: rgb(213, 219, 223);
}
</style>
