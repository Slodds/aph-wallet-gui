<template>
  <div id="app" v-cloak>
    <div class="drag-area"></div>
    <router-view></router-view>
    <flash-message class="vue-flash-container"></flash-message>
    <div id="out-of-date" v-if="isOutOfDate">A new version is available. <a :href="newVersionDownloadUrl" :title="newVersionDownloadUrl" target="_blank">Click here</a> to download v{{this.$store.state.latestVersion.version}}.</div>
  </div>
</template>

<script>
let fetchLatestVersionIntervalId;

export default {
  name: 'aphtoken-wallet',

  beforeDestroy() {
    clearInterval(fetchLatestVersionIntervalId);
  },

  beforeMount() {
    const shell = require('electron').shell;
    document.addEventListener('click', (e) => {
      if (e.target.tagName === 'A' && e.target.target === '_blank' && e.target.href.startsWith('http')) {
        e.preventDefault();
        shell.openExternal(e.target.href);
      }
    });

    this.fetchLatestVersion();
    fetchLatestVersionIntervalId =
      setInterval(this.fetchLatestVersion(), this.$constants.WALLET_VERSION_CHECK);
  },

  computed: {
    isOutOfDate() {
      return this.$store.state.latestVersion
        && this.$store.state.latestVersion.version > this.$store.state.version;
    },
    newVersionDownloadUrl() {
      if (process.platform === 'darwin') {
        return this.$store.state.latestVersion.downloadUrlMac;
      } else if (process.platform === 'linux') {
        return this.$store.state.latestVersion.downloadUrlLin;
      } else if (process.platform === 'win32') {
        return this.$store.state.latestVersion.downloadUrlWin;
      }
      return '';
    },
  },

  created() {
    window.addEventListener('online', this.updateOnlineStatus);
    window.addEventListener('offline', this.updateOnlineStatus);
    this.updateOnlineStatus();
  },

  data() {
    return {
      latestWalletVersion: '',
      online: true,
      outOfDate: false,
    };
  },

  destroyed() {
    window.removeEventListener('online', this.updateOnlineStatus);
    window.removeEventListener('offline', this.updateOnlineStatus);
  },

  methods: {
    fetchLatestVersion() {
      this.$store.dispatch('fetchLatestVersion');
    },

    updateOnlineStatus() {
      this.online = navigator.onLine;
    },
  },
};
</script>

<style lang="scss">
::-webkit-scrollbar {
  width: $border-width;
}

::-webkit-scrollbar-thumb:vertical {
  background: $purple;
}

* {
  box-sizing: border-box;
  letter-spacing: $letter-spacing;
}

a {
  text-decoration: none;

  &:focus {
    outline: none;
  }
}

#app, #app > * {
  height: 100%;

  .vue-flash-container  {
    height: auto;
  }
}

.drag-area {
  -webkit-app-region: drag;
  background:transparent;
  height: $space-lg !important;
  left: 0;
  position: fixed;
  top: 0;
  width: 100%;
}

#out-of-date {
  background: rgba(black, 0.8);
  color: $red;
  font-family: GilroyMedium;
  font-size: toRem(14px);
  height: auto;
  left: 0;
  padding: $space-sm;
  position: fixed;
  right: 0;
  text-align: center;
  top: 0;
  z-index: 10000;

  a {
    color: white;
    cursor: pointer;
  }
}

.vue-flash-container {
  bottom: $space;
  position: absolute;
  right: $space;
  width: toRem(500px);
  z-index: 10000;
}

.flash__message {
  background-color: $dark !important;
  border-radius: $border-radius;
  border: $border;
  font-family: GilroyMedium;
  margin: 0;
  padding: $space-lg;
  font-size: toRem(18px);
  word-wrap: break-word;


  .flash__close-button {
    display: none;
  }

  &.error {
    border-color: $red;
    color: $red;
  }

  &.info {
    border-color: $purple;
    color: $purple;
  }

  &.success {
    border-color: $green;
    color: $green;
  }

  &.warning {
    border-color: $orange;
    color: $orange;
  }

  & + .flash__message {
    margin-top: $space;
  }
}
</style>
