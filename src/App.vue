<template>
  <ConfigProvider :locale="zhCN" :transform-cell-text="transformCellText" v-bind="lockOn">
    <router-view />
  </ConfigProvider>
</template>

<script lang="ts">
  import { defineComponent } from 'vue';
  import { ConfigProvider } from 'ant-design-vue';
  import { createBreakpointListen } from '/@/hooks/event/useBreakpoint';

  import zhCN from 'ant-design-vue/es/locale/zh_CN';
  import moment from 'moment';
  import 'moment/dist/locale/zh-cn';

  import { useConfigProvider, useInitAppConfigStore } from './useApp';
  import { useLockPage } from '/@/hooks/web/useLockPage';
  import { useSetting } from '/@/hooks/core/useSetting';

  moment.locale('zh-cn');

  export default defineComponent({
    name: 'App',
    components: { ConfigProvider },
    setup() {
      // Initialize application settings
      useInitAppConfigStore();
      // Initialize breakpoint monitoring
      createBreakpointListen();
      // Get system configuration
      const { projectSetting } = useSetting();
      // Get ConfigProvider configuration
      const { transformCellText } = useConfigProvider();

      let lockOn = {};
      if (projectSetting.lockTime) {
        // Monitor the mouse or keyboard time, used to recalculate the lock screen time
        const { on } = useLockPage();
        lockOn = on;
      }

      return {
        transformCellText,
        zhCN,
        lockOn,
      };
    },
  });
</script>
