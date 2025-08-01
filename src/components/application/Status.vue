<template>
  <div class="status">
    <el-dialog v-model="showInfo" width="600px">
      <div v-if="application">
        <p class="text-left mb-4">
          {{ $t('application.message.applicationSelection') }}
        </p>
        <div class="applications flex flex-col gap-2 mb-6">
          <div
            v-for="(app, index) in applications"
            :key="index"
            :class="{
              item: true,
              active: application?.id === app.id
            }"
            @click="onSelectApplication(app)"
          >
            <el-icon class="icon"><check /></el-icon>
            <application-info :application="app" :show-type="true" :show-id="true" />
            <el-button round type="primary" @click.stop="onBuyMore(application)">{{
              $t('common.button.buyMore')
            }}</el-button>
          </div>
        </div>
        <span class="actions">
          <api-price
            v-if="showPrice && application?.service?.apis?.[0]?.price"
            class="price inline-block"
            :price="application?.service?.apis?.[0]?.price"
          />
        </span>
      </div>
    </el-dialog>
    <el-button circle @click="showInfo = true">
      <font-awesome-icon icon="fa-solid fa-wallet" class="icon" />
    </el-button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { ElButton, ElIcon, ElDialog, ElTooltip } from 'element-plus';
import { IApplicationType, IApplication, IService } from '@/models';
import { ROUTE_CONSOLE_APPLICATION_EXTRA, ROUTE_CONSOLE_APPLICATION_SUBSCRIBE } from '@/router';
import ApiPrice from '@/components/api/Price.vue';
import { Check } from '@element-plus/icons-vue';
import ApplicationInfo from './Info.vue';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';

export interface IData {
  showInfo: boolean;
  applicationType: typeof IApplicationType;
}

export default defineComponent({
  name: 'ApplicationStatus',
  components: {
    ElButton,
    Check,
    ElDialog,
    FontAwesomeIcon,
    ApplicationInfo,
    ApiPrice,
    ElIcon
  },
  props: {
    application: {
      type: Object as () => IApplication | undefined,
      required: true
    },
    applications: {
      type: Array as () => IApplication[] | undefined,
      default: undefined
    },
    showPrice: {
      type: Boolean,
      default: true
    },
    service: {
      type: Object as () => IService | undefined,
      required: true
    }
  },
  emits: ['select'],
  data(): IData {
    return {
      showInfo: false,
      applicationType: IApplicationType
    };
  },
  computed: {
    authenticated() {
      return !!this.$store.state.token.access;
    },
    user() {
      return this.$store.state.user;
    }
  },
  methods: {
    onBuyMore(application: IApplication) {
      // open in new tab for this url
      const url = this.$router.resolve({
        name: ROUTE_CONSOLE_APPLICATION_EXTRA,
        params: {
          id: application.id
        }
      }).href;
      window.open(url, '_blank');
    },
    onSelectApplication(application: IApplication) {
      this.$emit('select', application);
    }
  }
});
</script>

<style lang="scss" scoped>
.applications {
  .item {
    cursor: pointer;
    padding: 20px;
    background-color: var(--el-bg-color);
    border-radius: 15px;
    border: 1px solid var(--el-border-color-lighter);
    .icon {
      visibility: hidden;
    }
    &.active {
      .icon {
        visibility: visible;
      }
    }
    display: flex;
    align-items: center;
    gap: 30px;
    .icon {
      color: var(--el-color-white);
      font-size: 20px;
      background-color: var(--el-color-primary);
      border-radius: 50%;
      padding: 5px;
    }
  }
}

.actions {
  margin: 0 5px;
  display: inline-block;
  @media (max-width: 767px) {
    display: block;
    margin: 5px auto 0 auto;
    width: fit-content;
  }
  .el-button {
    border-radius: 20px;
  }
}
.btn-apply {
  border-radius: 20px;
}
</style>
