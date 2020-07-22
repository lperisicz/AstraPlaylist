<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
      clipped
      color="#404040"
    >
      <v-list dense>
        <v-list-item
          v-for="item in items"
          :key="item.text"
          link
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>
              {{ item.text }}
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-subheader class="mt-4 ml-3 grey--text text--darken-1" style="font-size: 24px; color: white !important;">Playlists<v-spacer>

        </v-spacer>
        <v-icon @click="loadPlaylist">restore</v-icon>
        </v-subheader>
        <v-list>
          <v-list-item
            v-for="item in items2"
            :key="item.id"
            link
          >
            <v-list-item-avatar>
              <img
                src="slika.jpg"
                alt=""
              >
            </v-list-item-avatar>
            <v-list-item-title v-text="item.title" @click="showPlaylist(item.id)"></v-list-item-title>
          </v-list-item>
        </v-list>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      clipped-left
      color="#000000"
      dense
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-icon
        class="mx-4"
        large
      >
        mdi-youtube
      </v-icon>
      <v-toolbar-title class="mr-12 align-center">
        <span class="title">Astra playlist</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-app-bar>
    <v-content style="background-color: #606060; height: 100%;" >
      <nuxt />
    </v-content>
  </v-app>
</template>

<script>
    export default {
      middleware: 'auth',
        props: {
            source: String,
        },
        data: () => ({
            drawer: null,
            items2: [
                { picture: 28, text: 'Joseph' },
                { picture: 38, text: 'Apple' },
                { picture: 48, text: 'Xbox Ahoy' },
                { picture: 58, text: 'Nokia' },
                { picture: 78, text: 'MKBHD' },
            ],
        }),
      methods: {
          showPlaylist(id){
            this.$router.push('/playlist?id=' + id)
          },
        async loadPlaylist(){
          const res = await this.$axios.post('playlist/filter')
          this.items2 = res.data.data.data
        }
      },
        async created () {
            this.loadPlaylist()
            this.$vuetify.theme.dark = true
        },
    }
</script>
