<template>
  <div v-if='mac_os' id='titlebar' class='mac'>&nbsp;</div>
  <div v-else id='titlebar'>
    <div id='titlebar-left'>
      RSS Feed Reader
    </div>
    <div id='titlebar-right'>
      <!--
        This is intentionally ugly: https://css-tricks.com/fighting-the-space-between-inline-block-elements/
        This is the easiest solution and I'm lazy.
      -->
      <button @click='minimizeWindow'><span class='fas fa-minus'></span>
      </button><button @click='maximizeWindow'><span class='far fa-clone' v-if='is_maximized'></span><span class='far fa-square' v-else></span>
      </button><button @click='closeWindow' class='titlebar-close'><span class='fas fa-times'></span>
      </button>
    </div>
  </div>
</template>

<script>
  import { remote } from 'electron';

  const window = remote.getCurrentWindow();

  export default {
    name: 'titlebar',
    data() {
      return {
        is_maximized: window.isMaximized(),
        mac_os: remote.process.platform === 'darwin',
        settings_panel_open: false,
      };
    },
    mounted() {
      window.on('unmaximize', () => {
        this.is_maximized = false;
      });
      window.on('maximize', () => {
        this.is_maximized = true;
      });
    },
    methods: {
      closeWindow() {
        window.close();
      },
      maximizeWindow() {
        if (window.isMaximized()) {
          window.unmaximize();
        } else {
          window.maximize();
        }
      },
      minimizeWindow() {
        window.minimize();
      },
    },
  };
</script>
