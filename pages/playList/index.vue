<template>
  <div style=" display: flex; justify-content: center;" class="pt-12">
    <v-card
      min-width="500px"
      class="mr-6"
    >
      <v-card-title class="white--text orange darken-4">
        Available songs
        <v-spacer></v-spacer>
        <v-btn
          class="text--primary"
          @click="newSongDialog"
        >
          Add song
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </v-card-title>

      <v-card-text class="pt-4">
        Samo opusteno
      </v-card-text>

      <v-divider></v-divider>

      <v-virtual-scroll
        :items="items"
        :item-height="50"
        height="500"
      >
        <template v-slot="{ index , item }">
          <v-list-item>
            <v-list-item-avatar>
              <v-avatar
                :color="blue"
                size="56"
                class="white--text"
              >
                {{ item.id }}
              </v-avatar>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
              <v-btn
                depressed
                small
                @click="addSong(item, index)"
              >
                Add +

                <v-icon
                  color="orange darken-4"
                  right
                >
                  mdi-open-in-new
                </v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </template>
      </v-virtual-scroll>
    </v-card>
    <v-card
      min-width="500px"
      class="ml-6"
    >
      <v-card-title class="white--text orange darken-4">
        Your playlist
        <v-spacer></v-spacer>

        <v-btn
          color="black"
          class="text--primary"
          to="/playlist"
        >
          Add brand new Playlist
        </v-btn>
      </v-card-title>

      <v-card-text class="pt-4">
        <v-text-field v-model="playlist.title" label="Set playlist name" hide-details outlined></v-text-field>
      </v-card-text>

      <v-divider></v-divider>

      <v-virtual-scroll
        :items="items2"
        :item-height="50"
        height="500"
      >
        <template v-slot="{ index, item }">
          <v-list-item>
            <v-list-item-avatar>
              <v-avatar
                :color="blue"
                size="56"
                class="white--text"
              >
                {{ item.id}}
              </v-avatar>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
              <v-btn
                depressed
                small
                @click="removeItem(index)"
              >
                Remove
                <v-icon
                  color="orange darken-4"
                  right
                >
                  mdi-delete
                </v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </template>
      </v-virtual-scroll>
      <v-card-actions style="display: flex; justify-content: flex-end">
        <v-btn class="ma-2" color="orange" @click="savePlaylist">Save</v-btn>
      </v-card-actions>
    </v-card>
    <v-dialog v-model="dialog" width="500px">
        <v-card style="background-color: #606060">
          <v-toolbar color="orange" dark>
            <v-toolbar-title>New song</v-toolbar-title>
          </v-toolbar>
          <v-card-text>
            <v-form ref="newSong">
            <div>
              <span>Song title</span>
              <v-text-field outlined v-model="newSong.title" :rules="rules.newSong">

              </v-text-field>
            </div>
            <div>
              <span>Filename</span>
              <v-text-field outlined v-model="newSong.filename" :rules="rules.newSong">

              </v-text-field>
            </div>
            </v-form>
          </v-card-text>
          <v-card-actions style="display: flex; justify-content: flex-end">
            <v-btn color="red" @click="!dialog">Cancle</v-btn>
            <v-btn color="green" @click="addNewSong">Save</v-btn>
          </v-card-actions>
        </v-card>
    </v-dialog>
  </div>
</template>
<script>
    export default {
        layout: 'front',
        data: () => ({
          playlist: {title: ''},
            dialog: false,
            newSong:{ title: '', filename: '' },
          rules: {
            newSong: [(v) => !!v || "This field is required"]
          },
            items: [
            ],
            items2: []
        }),
      watchQuery (newID, oldId){
            this.getUsersPlayList(newID.id)

      },
        async created(){
          this.getFullPlaylist()
          this.items2 = []
          this.playlist.title = ''
        },
        methods: {
          async getUsersPlayList(id){
            if(id){
              try{
                const playlist = await this.$axios.get('playlist/get/' + id)
                console.log(playlist)
                this.items2 = playlist.data.data.songs
                this.playlist.title = playlist.data.data.title
              }catch(e){
              }
            } else {
              this.items2 = []
              this.playlist.title = ''
            }


          },
          async getFullPlaylist(){
            const res = await this.$axios.post('song/filter')
            this.items = res.data.data.data
          },
           async  addNewSong(){
            if(!this.$refs.newSong.validate()) return
            await this.$axios.post('song/create', this.newSong)
            this.$toast.success('You added new song to your playlist')
            this.getFullPlaylist()
             this.dialog = !this.dialog
             window.location.reload(true)
          },
          newSongDialog(){
            this.dialog = !this.dialog
          },
          addSong(item){
            this.items2.push(item)
          },
          removeItem(index){
            this.items2.splice(index,1)
          },
          savePlaylist(){
            const songIds = this.items2.map(song => song.id)
            console.log(songIds)
            try{
              if(this.$route.query.id){
                this.$axios.post('song/attach', {
                  title: this.playlist.title,
                  songIds: songIds,
                  playlistId: this.$route.query.id
                })
              }else{
                this.$axios.post('song/attach', {
                  title: this.playlist.title,
                  songIds: songIds
                })
              }
              this.$toast.success('Created new playlist')
            } catch (e) {
              this.$toast.error('Something went wrong')
            }


          }
        },
    }
</script>
