<template>
  <div class="home mt-16">
    <v-row no-gutters>
      <v-col cols="12" sm="6" md="8">
        <v-card class="pa-2" outlined tile>
          <!-- Where the video is played -->
          <div class="container">
            <div class="row">
              <div class="col-md-12 my-3">
                <h2>Room</h2>
                <input v-model="roomId" />
              </div>
            </div>
            <div class="row">
              <div class="col-md-12">
                <div class="">
                  <Webrtc
                    ref="webrtc"
                    width="100%"
                    :roomId="roomId"
                    v-on:joined-room="logEvent"
                    v-on:left-room="logEvent"
                    v-on:opened-room="logEvent"
                    v-on:share-started="logEvent"
                    v-on:share-stopped="logEvent"
                    @error="onError"
                    turnServer="turn:ws-turn5.xirsys.com:80?transport=udp"
                    stunServe="stun:ws-turn5.xirsys.com"
                  />
                </div>
                <div class="row">
                  <div class="col-md-12 my-3">
                    <button
                      type="button"
                      class="btn btn-primary"
                      @click="onJoin"
                    >
                      Join
                    </button>
                    <button
                      type="button"
                      class="btn btn-primary"
                      @click="onLeave"
                    >
                      Leave
                    </button>
                    <button
                      type="button"
                      class="btn btn-primary"
                      @click="onCapture"
                    >
                      Capture Photo
                    </button>
                    <button
                      type="button"
                      class="btn btn-primary"
                      @click="onShareScreen"
                    >
                      Share Screen
                    </button>
                  </div>
                </div>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12">
                <h2>Captured Image</h2>
                <figure class="figure">
                  <img :src="img" class="img-responsive" />
                </figure>
              </div>
            </div>
          </div>
          <!--/ Where the video is played -->
        </v-card>
      </v-col>
      <v-col cols="6" md="4">
        <v-card class="pa-2 __room" outlined tile>
          <v-list three-line>
            <template v-for="(item, index) in items">
              <v-subheader
                v-if="item.header"
                :key="item.header"
                v-text="item.header"
              ></v-subheader>

              <v-divider
                v-else-if="item.divider"
                :key="index"
                :inset="item.inset"
              ></v-divider>

              <v-list-item v-else :key="item.title" link>
                <v-list-item-avatar>
                  <v-img :src="item.avatar"></v-img>
                </v-list-item-avatar>

                <v-list-item-content>
                  <v-list-item-title v-html="item.title"></v-list-item-title>
                  <v-list-item-subtitle
                    v-html="item.subtitle"
                  ></v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </template> </v-list
        ></v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
// @ is an alias to /src
import Webrtc from "@/components/Webrtc.vue";

import * as io from "socket.io-client";
window.io = io;
export default {
  name: "Home",
  components: {
    Webrtc,
  },
  data() {
    return {
      img: null,
      roomId: "public-room",
      items: [
        { header: "In room" },
        {
          avatar: "https://cdn.vuetifyjs.com/images/lists/1.jpg",
          title: "Brunch this weekend?",
          subtitle: `<span class="text--primary">Ali Connors</span> &mdash; I'll be in your neighborhood doing errands this weekend. Do you want to hang out?`,
        },
        { divider: true, inset: true },
        {
          avatar: "https://cdn.vuetifyjs.com/images/lists/2.jpg",
          title: 'Summer BBQ <span class="grey--text text--lighten-1">4</span>',
          subtitle: `<span class="text--primary">to Alex, Scott, Jennifer</span> &mdash; Wish I could come, but I'm out of town this weekend.`,
        },
        { divider: true, inset: true },
        {
          avatar: "https://cdn.vuetifyjs.com/images/lists/3.jpg",
          title: "Oui oui",
          subtitle:
            '<span class="text--primary">Sandra Adams</span> &mdash; Do you have Paris recommendations? Have you ever been?',
        },
        { divider: true, inset: true },
        {
          avatar: "https://cdn.vuetifyjs.com/images/lists/4.jpg",
          title: "Birthday gift",
          subtitle:
            '<span class="text--primary">Trevor Hansen</span> &mdash; Have any ideas about what we should get Heidi for her birthday?',
        },
        { divider: true, inset: true },
        {
          avatar: "https://cdn.vuetifyjs.com/images/lists/5.jpg",
          title: "Recipe to try",
          subtitle:
            '<span class="text--primary">Britta Holt</span> &mdash; We should eat this: Grate, Squash, Corn, and tomatillo Tacos.',
        },
        {
          avatar: "https://cdn.vuetifyjs.com/images/lists/5.jpg",
          title: "Recipe to try",
          subtitle:
            '<span class="text--primary">Britta Holt</span> &mdash; We should eat this: Grate, Squash, Corn, and tomatillo Tacos.',
        },
      ],
    };
  },
  methods: {
    onCapture() {
      this.img = this.$refs.webrtc.capture();
    },
    onJoin() {
      console.log("Joining room");
      this.$refs.webrtc.join();
    },
    onLeave() {
      console.log("Leaving room");
      this.$refs.webrtc.leave();
    },
    onShareScreen() {
      this.img = this.$refs.webrtc.shareScreen();
    },
    onError(error, stream) {
      console.log("On Error Event", error, stream);
    },
    logEvent(event) {
      console.log("Event : ", event);
    },
  },
};
</script>
<style scoped>
.__room {
  height: 600px;
  overflow-y: scroll;
}
</style>
