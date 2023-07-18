<script setup lang="ts">
import { isDev, isPrerender } from "~/utils/nuxt";
import bg from "~/assets/image/outerwilds.jpg";
import config from "~/config";

const commitSha = computed(() => useRuntimeConfig().app.NUXT_ENV_CURRENT_GIT_SHA);
const commitUrl = computed(() => `https://github.com/${config.githubName}/${config.githubRepo}/commit/${commitSha.value}`);
const buildTime = ref<string>("$(inject:timestamp)");

const paragraphs = [
"观自在菩萨，行深般若波罗蜜多时，照见五蕴皆空，度一切苦厄。舍利子，色不异空，空不异色，色即是空，空即是色，受想行识，亦复如是。舍利子，是诸法空相，不生不灭，不垢不净，不增不减。是故空中无色，无受想行识，无眼耳鼻舌身意，无色声香味触法，无眼界，乃至无意识界，无无明，亦无无明尽，乃至无老死，亦无老死尽。无苦集灭道，无智亦无得。以无所得故。菩提萨埵，依般若波罗蜜多故，心无挂碍。无挂碍故，无有恐怖，远离颠倒梦想，究竟涅槃。三世诸佛，依般若波罗蜜多故，得阿耨多罗三藐三菩提。故知般若波罗蜜多，是大神咒，是大明咒，是无上咒，是无等等咒，能除一切苦，真实不虚。故说般若波罗蜜多咒，即说咒曰：揭谛揭谛，波罗揭谛，波罗僧揭谛，菩提萨婆诃。"
];

onBeforeMount(async () => {
  if (!isPrerender) {
    buildTime.value = (!isDev ? (await (await fetch("/timestamp.txt")).text()) : "-");
  }
});
</script>

<template>
  <div class="about flexc">
    <img :src="bg" alt="bg">
    <div class="flexc paragraphs">
      <p v-for="p,idx in paragraphs" :key="idx">
        {{ p }}
      </p>
    </div>
    <div class="status">
      Last built &lt;<a target="_blank" :href="commitUrl">{{ commitSha.substring(0, 8) }}</a>&gt; succeeded at
      <time>{{ buildTime }}</time>
    </div>
  </div>
</template>

<style lang="scss">
#default-layout.in-about {
  #header {
    background: rgb(255 255 255 / 5%);
    backdrop-filter: blur(1px);
  }

  #body {
    padding-top: 0;

    .about {
      width: 100vw;
      height: calc(100vh - #{$footer-height});
      padding-bottom: $footer-height;
      position: relative;

      img {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        object-fit: cover;
        object-position: right;
        z-index: 1;
      }

      .paragraphs {
        position: relative;
        z-index: 2;
        padding-top: 100px + $header-height;
        justify-content: center;

        p {
          font-size: f-size();
          letter-spacing: 0.8px;
          color: white;
          font-weight: 300;

          &:not(:last-of-type) {
            margin-bottom: 20px;
          }
        }
      }

      .status {
        color: #eee;
        opacity: 0.5;
        font-size: f-size(0.75);
        position: absolute;
        bottom: 10px;
        right: 10px;
        z-index: 1;
        transition: $common-transition;

        &:hover {
          opacity: 1;
        }

        time {
          color: #ffb350;
        }
      }
    }
  }

  #footer {
    position: fixed;
    width: 100%;
    left: 0;
    bottom: 20px;
    z-index: $z-index-footer;

    .middle {
      color: #a1a1a1;
    }

    &:hover {
      .middle {
        color: #e7e7e7;
      }
    }
  }

  #footer,
  #body .about {
    a[href] {
      color: $theme-color;
      transition: $common-transition;

      &:hover {
        color: $theme-color-lighten;
      }
    }
  }
}
</style>
