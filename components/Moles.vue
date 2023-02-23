<template>
  <div v-bind:class="classNames">
    <Mole v-bind:mole_state="mole1" moleID="0" v-on:whack="send_app"/>
    <Mole v-bind:mole_state="mole2" moleID="1" v-on:whack="send_app"/>
    <Mole v-bind:mole_state="mole3" moleID="2" v-on:whack="send_app"/>
    <Mole v-bind:mole_state="mole4" moleID="3" v-on:whack="send_app"/>
  </div>
</template>

<script>
import Mole from './Mole.vue';

export default {
  name: 'Moles',
  components: {
    Mole,
  },
  props: ['mole1','mole2','mole3','mole4','gameActive'],
  emits:['event'],
  computed: {
    classNames() {
      return {
        'moles-container': true,
        'game-active': this.gameActive,
      };
    }
  },
  methods: {
    send_app(moleID) {
      this.$emit('event', moleID);
    }
  },
};
</script>

<style scoped>
.moles-container {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  opacity: 0.5;
  transition: opacity 0.3s ease;
}

.moles-container.game-active {
  opacity: 1;
}
</style>