<template>
  <div style="padding: 100px 300px 100px 300px ; display: flex; justify-content: space-between">
    <v-card
      class="mx-auto"
      min-width="400"
    >
      <v-card-title class="white--text orange darken-4">
        Song list

        <v-spacer></v-spacer>

        <v-btn
          color="white"
          class="text--primary"
          fab
          small
        >
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
        <template v-slot="{ item }">
          <v-list-item>
            <v-list-item-avatar>
              <v-avatar
                :color="item.color"
                size="56"
                class="white--text"
              >
                {{ item.initials }}
              </v-avatar>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title>{{ item.fullName }}</v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
              <v-btn
                depressed
                small
                @click="addSong(item)"
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
      class="mx-auto"
      min-width="400"
    >
      <v-card-title class="white--text orange darken-4">
        Play list

        <v-spacer></v-spacer>

        <v-btn
          color="white"
          class="text--primary"
          fab
          small
        >
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </v-card-title>

      <v-card-text class="pt-4">
        mogli bi mogli bi mogli bi Ovo slusat
      </v-card-text>

      <v-divider></v-divider>

      <v-virtual-scroll
        :items="items2"
        :item-height="50"
        height="500"
      >
        <template v-slot="{ item }">
          <v-list-item>
            <v-list-item-avatar>
              <v-avatar
                :color="item.color"
                size="56"
                class="white--text"
              >
                {{ item.initials }}
              </v-avatar>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title>{{ item.fullName }}</v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
              <v-btn
                depressed
                small
              >
                View User

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
  </div>
</template>
<script>
    export default {
        layout: 'front',
        data: () => ({
            items: [
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'},
                { color: 'orange', initials: 'BM', fullName: 'Borna Marin'}

            ],
            items2: []
        }),



        methods: {
            genRandomIndex (length) {
                return Math.ceil(Math.random() * (length - 1))
            },
        },
    }
</script>
