<template>
	<div class="top-bar">
		<div class="item" :title="obs.version ? 'OBS Websocket Version: ' + obs.version : false">
			Connection: <span :class="connectionStateClass" v-text="connectionStateText"></span>
		</div>
		<div class="item" v-show="connectionState">
		    <i class="fa fa-twitch"></i> Status: <span :class="streamStatusClass" v-text="streamStatusText"></span>
		</div>
		<div class="item">
		    <button @click="toggleStream" v-show="connectionState" v-text="streamToggleButtonText"></button>
		</div>
		<div class="space"></div>
		<div class="item">
			<button @click="forceRefresh" title="Force Refresh (In case things get out of sync)"><i class="material-icons">refresh</i></button>
		</div>
		<div class="item">
			<button @click="toggleSettings"><i class="material-icons">settings</i></button>
		</div>
		<div class="item">
			<button @click="toggleFullscreen"><i class="material-icons">fullscreen</i></button>
		</div>
	</div>
</template>

<script>
	import toggleFullscreenMixin from '../mixins/fullscreen'

	export default {
        mounted: function() {
          console.log('Hello');  
        },
		mixins: [toggleFullscreenMixin],

		computed: {
            connectionState() {
              if(!this.obs.connected) {
                  return false;
              }  
              if(this.obs.authRequired) {
                  return false;
              }
              if(this.obs.connected) {
                  return true;
              }
            },
			connectionStateClass() {
				if (!this.obs.connected) {
					return 'error-text'
				}
				if (this.obs.authRequired) {
					return 'warning-text'
				}
				return 'success-text'
			},
            streamStatusText() {
              if(!this.obs.streamStatus) {
                  return 'Offline'
              }
              else
              {
                  return 'Online'
              }
            },
            streamStatusClass() {
              if(!this.obs.streamStatus) {
                  return 'error-text'
              }
              else
              {
                  return 'success-text'
              }
            },
            streamToggleButtonText() {
              if(!this.obs.streamStatus) {
                  return 'Start streaming'
              }
              else {
                  return 'Stop streaming'
              }
            },
			connectionStateText() {
				if (!this.obs.connected) {
					return 'Down'
				}
				if (this.obs.authRequired) {
					return 'Authenticate'
				}
				return 'Ok'
			}
		},
		methods: {
			forceRefresh() {
				this.$emit('force-refresh')
			},
			toggleSettings() {
				this.$emit('toggle-settings')
			},
            toggleStream() {
                this.$root.$refs.app.$obs.toggleStreamStatus()

            }
		},
		props: {
			obs: {
				type: Object,
				required: true
			}
		}
	}
</script>
