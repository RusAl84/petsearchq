<template><q-page>
  <q-card>
    <q-card-section>

  <yandex-map :coords="coord" :settings="settings" :zoom="10" @click="onClick">
      <ymap-marker
        v-for="(item, i) in coords"
        marker-id="i"
        :key="i"
        :coords="item.coord"
        :hint-content="item.content"

      ></ymap-marker>

    </yandex-map>
      <q-btn
          depressed
          color="primary"
          @click="onSendClick"
        >
          Оправить свои координаты
        </q-btn> {{location}} {{ycoords}}

    </q-card-section>
  </q-card>
</q-page>
</template>

<script>
import { yandexMap, ymapMarker } from 'vue-yandex-maps'
import { computed, defineComponent, ref } from "vue";
import { useStore } from 'vuex'
import API from "../api/api.js";

export default {
  name: 'App',
  components: { yandexMap, ymapMarker },
  setup() {
        const $store = useStore()
    const userName = computed({
      get: () => $store.state.mes.userName,
      set: val => {
        $store.commit('mes/updateUserNameState', val)
      }
      })
    const messages = computed({
      get: () => $store.state.mes.messages,
      set: val => {
        $store.commit('mes/updateMessagesState', val)
      }
    })
    const intervalCtx = computed({
      get: () => $store.state.mes.intervalCtx,
      set: val => {
        $store.commit('mes/updateIntervalCtxState', val)
      }
    })
    const lastMsgID = computed({
      get: () => $store.state.mes.lastMsgID,
      set: val => {
        $store.commit('mes/updateLastMsgIDState', val)
      }
    })
    const messageText = computed({
      get: () => $store.state.mes.messageText,
      set: val => {
        $store.commit('mes/updateMessageTextState', val)
      }
    })
    let location=[]
    return {
      coord: [55.75, 37.61],
      coords:  [
        {coord:[55.726822, 37.557682], content: "aaa" },
        {coord:[55.760178, 37.618575], content: "bbb" },
        {coord:[55.819721, 37.611704], content: "ccc" },
      ],
      ycoords: ref([55.75, 37.61]),
      location,
      settings : {
        apiKey: '73784db1-80df-448a-95a3-6f81554055ea',
        lang: 'ru_RU',
        coordorder: 'latlong',
        enterprise: false,
        version: '2.1'
    },$store, userName, messages, intervalCtx, lastMsgID,  messageText,
    }
  },
  //   mounted() {
  //   // this.userName = "Login";
  //   this.intervalCtx = setInterval(async () => {
  //     try {
  //       const msg = await API.getMessage(this.lastMsgID);
  //       if (typeof msg === typeof {}) {
  //         this.messages=msg
  //         this.lastMsgID++
  //       }
  //     } catch (e) {}
  //   }, 1000);
  // },
  // unmounted() {
  //   clearInterval(this.intervalCtx);
  // },
  computed: {
    balloonTemplate() {
      return `
        <h5 class="red">Hi, everyone!</h5>
        <p>I am here: ${this.coords.content}</p>
        <img src="http://via.placeholder.com/350x150">
      `
    }
  },
  methods: {
    // Реакция на кнопку отправки
    onSendClick() {
    // if(!("geolocation" in navigator)) {
    //   this.errorStr = 'Geolocation is not available.';
    //   return;
    // }

    // this.gettingLocation = true;
    // get position
    navigator.geolocation.getCurrentPosition(pos => {
      this.gettingLocation = false;
      this.location = pos;
    },)
    // setTimeout(() => {
    //     console.log("Weit4000!");
    //   }, 4000);
    this.ycoords=[this.location["coords"]["latitude"],this.location["coords"]["longitude"]]
    let e=0.01

    // this.$forceUpdate();
    // err => {
    //   this.gettingLocation = false;
    //   this.errorStr = err.message;
    // })
    },
    onClick(e) {
      this.ycoords = e.get('coords');

    },
},
}
</script>
<style>
.ymap-container {
  height: 600px;
}
</style>
