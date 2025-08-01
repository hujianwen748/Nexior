<template>
  <div :direction="direction" class="navigator">
    <div class="top">
      <div v-if="direction === 'column'">
        <logo-tiny @click="onHome" />
      </div>
      <div
        v-for="(link, linkIndex) in links"
        :key="linkIndex"
        :class="{link: true, active: link.routes.includes($route.name as string)}"
      >
        <el-tooltip v-if="direction === 'column'" effect="dark" :content="link.displayName" placement="right">
          <el-image v-if="link.logo" :src="link.logo" class="avatar" @click="$router.push(link.route)" />
        </el-tooltip>
      </div>
    </div>
    <div class="bottom">
      <user-center />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { ElTooltip, ElImage } from 'element-plus';
import {
  ROUTE_PROFILE_INDEX,
  ROUTE_INDEX,
  ROUTE_MIDJOURNEY_INDEX,
  ROUTE_LUMA_INDEX,
  ROUTE_LUMA_HISTORY,
  ROUTE_HAILUO_INDEX,
  ROUTE_HAILUO_HISTORY,
  ROUTE_SUNO_INDEX,
  ROUTE_SUNO_HISTORY,
  ROUTE_FLUX_INDEX,
  ROUTE_CHATGPT_CONVERSATION_NEW,
  ROUTE_CHATGPT_CONVERSATION,
  ROUTE_DEEPSEEK_CONVERSATION_NEW,
  ROUTE_DEEPSEEK_CONVERSATION,
  ROUTE_GROK_CONVERSATION_NEW,
  ROUTE_GROK_CONVERSATION,
  ROUTE_KLING_INDEX,
  ROUTE_KLING_HISTORY
} from '@/router/constants';
import { CHAT_MODEL_ICON_CHATGPT, CHAT_MODEL_ICON_DEEPSEEK, CHAT_MODEL_ICON_GROK } from '@/constants/chat';
import LogoTiny from './LogoTiny.vue';
import UserCenter from '@/components/user/Center.vue';

export default defineComponent({
  name: 'Navigator',
  components: {
    ElImage,
    LogoTiny,
    ElTooltip,
    UserCenter
  },
  props: {
    direction: {
      type: String,
      default: 'column'
    }
  },
  data() {
    return {
      operating: {
        dark: false,
        locale: false
      },
      activeIndex: this.$route.name as string
    };
  },
  computed: {
    links() {
      const result = [];
      // Add chatgpt's leftmost icon
      if (this.$store?.state?.site?.features?.chatgpt?.enabled) {
        result.push({
          route: {
            name: ROUTE_CHATGPT_CONVERSATION_NEW
          },
          displayName: this.$t('common.nav.chatgpt'),
          logo: CHAT_MODEL_ICON_CHATGPT,
          routes: [ROUTE_CHATGPT_CONVERSATION, ROUTE_CHATGPT_CONVERSATION_NEW]
        });
      }
      // Add deepseek's leftmost icon
      if (this.$store?.state?.site?.features?.deepseek?.enabled) {
        result.push({
          route: {
            name: ROUTE_DEEPSEEK_CONVERSATION_NEW
          },
          displayName: this.$t('common.nav.deepseek'),
          logo: CHAT_MODEL_ICON_DEEPSEEK,
          routes: [ROUTE_DEEPSEEK_CONVERSATION, ROUTE_DEEPSEEK_CONVERSATION_NEW]
        });
      }
      // Add grok's leftmost icon
      if (this.$store?.state?.site?.features?.grok?.enabled) {
        result.push({
          route: {
            name: ROUTE_GROK_CONVERSATION_NEW
          },
          displayName: this.$t('common.nav.grok'),
          logo: CHAT_MODEL_ICON_GROK,
          routes: [ROUTE_GROK_CONVERSATION, ROUTE_GROK_CONVERSATION_NEW]
        });
      }
      // Add midjourney's leftmost icon
      if (this.$store?.state?.site?.features?.midjourney?.enabled) {
        result.push({
          route: {
            name: ROUTE_MIDJOURNEY_INDEX
          },
          displayName: this.$t('common.nav.midjourney'),
          logo: 'https://cdn.acedata.cloud/wto43b.png',
          routes: [ROUTE_MIDJOURNEY_INDEX]
        });
      }
      // Add flux's leftmost icon
      if (this.$store?.state?.site?.features?.flux?.enabled) {
        result.push({
          route: {
            name: ROUTE_FLUX_INDEX
          },
          displayName: this.$t('common.nav.flux'),
          logo: 'https://cdn.acedata.cloud/ogm2oa.png',
          routes: [ROUTE_FLUX_INDEX]
        });
      }
      // Add qrart's leftmost icon
      // if (this.$store?.state?.site?.features?.qrart?.enabled) {
      //   result.push({
      //     route: {
      //       name: ROUTE_QRART_INDEX
      //     },
      //     displayName: this.$t('common.nav.qrart'),
      //     routes: [ROUTE_QRART_INDEX, ROUTE_QRART_HISTORY]
      //   });
      // }
      // Add suno's leftmost icon
      if (this.$store?.state?.site?.features?.suno?.enabled) {
        result.push({
          route: {
            name: ROUTE_SUNO_INDEX
          },
          displayName: this.$t('common.nav.suno'),
          logo: 'https://cdn.acedata.cloud/l3ffw7.jpg',
          routes: [ROUTE_SUNO_INDEX, ROUTE_SUNO_HISTORY]
        });
      }
      // Add luma's leftmost icon
      if (this.$store?.state?.site?.features?.luma?.enabled) {
        result.push({
          route: {
            name: ROUTE_LUMA_INDEX
          },
          displayName: this.$t('common.nav.luma'),
          logo: 'https://cdn.acedata.cloud/ahjfwi.png',
          routes: [ROUTE_LUMA_INDEX, ROUTE_LUMA_HISTORY]
        });
      }
      // Add pika's leftmost icon
      // if (this.$store?.state?.site?.features?.pika?.enabled) {
      //   result.push({
      //     route: {
      //       name: ROUTE_PIKA_INDEX
      //     },
      //     displayName: this.$t('common.nav.pika'),
      //     logo: 'https://cdn.acedata.cloud/i80tgn.png',
      //     routes: [ROUTE_PIKA_INDEX, ROUTE_PIKA_HISTORY]
      //   });
      // }
      // Add hailuo's leftmost icon
      if (this.$store?.state?.site?.features?.hailuo?.enabled) {
        result.push({
          route: {
            name: ROUTE_HAILUO_INDEX
          },
          displayName: this.$t('common.nav.hailuo'),
          logo: 'https://cdn.acedata.cloud/0qg4gp.png',
          routes: [ROUTE_HAILUO_INDEX, ROUTE_HAILUO_HISTORY]
        });
      }
      // Add kling's leftmost icon
      if (this.$store?.state?.site?.features?.kling?.enabled) {
        result.push({
          route: {
            name: ROUTE_KLING_INDEX
          },
          displayName: this.$t('common.nav.kling'),
          logo: 'https://cdn.acedata.cloud/qpbbbb.jpg',
          routes: [ROUTE_KLING_INDEX, ROUTE_KLING_HISTORY]
        });
      }
      // Add headshots's leftmost icon
      // if (this.$store?.state?.site?.features?.headshots?.enabled) {
      //   result.push({
      //     route: {
      //       name: ROUTE_HEADSHOTS_INDEX
      //     },
      //     displayName: this.$t('common.nav.headshots'),
      //     icon: 'fa-solid fa-id-card',
      //     routes: [ROUTE_HEADSHOTS_INDEX, ROUTE_HEADSHOTS_HISTORY]
      //   });
      // }
      if (this.direction === 'row') {
        result.push({
          route: {
            name: ROUTE_PROFILE_INDEX
          },
          displayName: this.$t('common.nav.profile'),
          icon: 'fa-solid fa-user',
          routes: [ROUTE_PROFILE_INDEX]
        });
      }
      return result;
    },
    authenticated() {
      return !!this.$store.state.token.access;
    }
  },
  methods: {
    onHome() {
      this.$router.push({
        name: ROUTE_INDEX
      });
    }
  }
});
</script>

<style lang="scss" scoped>
.navigator {
  display: flex;
  align-items: center;
  position: relative;

  &[direction='row'] {
    flex-direction: row;
    padding-top: 10px;
    border-top: 1px solid var(--el-border-color);
    .chevron,
    .logo {
      display: none;
    }
    .top {
      display: flex;
      flex-direction: row;
      align-items: center;
      justify-content: space-evenly;
      width: 100%;
      .link {
        text-align: center;
        .description {
          font-size: 10px;
          margin-top: 3px;
        }
        &.active {
          color: var(--el-color-primary);
        }
      }
    }
    .bottom {
      display: none;
    }
  }
  &[direction='column'] {
    flex-direction: column;
    .el-menu {
      width: 150px;
      border-right: none;
      background: none;
      .el-menu-item {
        height: 50px;
        color: var(--el-text-color-primary);
        &.active,
        &:hover,
        &:focus {
          background-color: var(--el-button-hover-bg-color);
          color: var(--el-color-primary);
        }
      }
    }

    .chevron {
      position: absolute;
      right: -12px;
      top: 50%;
      transform: translateY(-50%) scale(0.8);
      z-index: 10;
    }

    .logo {
      width: 80%;
      max-height: 50px;
      cursor: pointer;
      margin: 10px auto 20px auto;
      display: block;
      &.tiny {
        margin: 0 auto 10px auto;
        width: 40px;
        height: 40px;
      }
    }

    .top,
    .bottom {
      display: flex;
      flex-direction: column;
      padding-top: 10px;
      min-width: 60px;
      .link {
        width: 100%;
        height: 40px;
        margin-bottom: 10px;
        .avatar {
          display: block;
          margin: auto;
          width: 35px;
          height: 35px;
          border-radius: 50%;
          cursor: pointer;
          border: 1px solid var(--el-border-color-lighter);
        }
      }
    }
    .bottom {
      position: absolute;
      bottom: 0;
      display: flex;
      flex-direction: column;
      width: 100%;
      justify-content: flex-end;
      align-items: center;
      padding-bottom: 10px;
    }
  }
}
</style>
