<!-- Author:Shun Kiyoura-->
<template>
  <v-layout column align-center>
    <v-container xs12 sm8 md6>
      <div class="mx-20">content</div>
      <div class="text-center">
        <v-list
          v-if="rooms.length != 0"
          three-line
          style="max-height: 500px"
          class="overflow-y-auto"
        >
          <template v-for="room in rooms">
            <div>
              <v-list-item
                true
                :key="room.chat_room_id"
                link
                :to="`/rooms/${room.id}`"
              >
                <v-list-item-avatar>
                  <v-img
                    :src="avatar(room.users.find(n => n.id != id).id)"
                  ></v-img>
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title
                    v-html="room.users.find(n => n.id != id).username"
                    class="text-left"
                  ></v-list-item-title>
                  <v-list-item-subtitle
                    class="text-left"
                    v-if="room.users.find(n => n.id != id).valid == 0"
                    >{{
                      room.users.find(n => n.id != id).username + "を招待中です"
                    }}</v-list-item-subtitle
                  >
                  <v-list-item-subtitle
                    v-else
                    v-html="room.last_chat.content"
                  ></v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>

              <v-divider class="mx-1"></v-divider>
            </div>
          </template>
        </v-list>
        <v-card-text v-else
          ><h2 style="color: gray">
            Invite your friends to chat!
          </h2></v-card-text
        >
      </div>
    </v-container>
  </v-layout>
</template>

<script>
export default {
  async created() {
    const res = await this.$axios.get("/chat_rooms", {
      params: {
        id: this.$store.state.user.userInfo.id
      }
    });
    this.rooms = res.data;
    this.id = this.$store.state.user.userInfo.id;
    console.log(this.rooms);
  },

  data: () => ({
    rooms: [],
    id: null
  }),
  computed: {
    avatar() {
      return id => {
        const imageLen = 10;
        console.log(id);
        return `/user_icon_${(id % imageLen) + 1}.jpg`;
      };
    }
  }
};
</script>
