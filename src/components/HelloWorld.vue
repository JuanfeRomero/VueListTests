<template>
  <v-container>
    <v-row>
      <v-col>
        <v-card
          class="mx-auto"
        >
          <v-toolbar
            color="teal"
            dark
          >
            <v-app-bar-nav-icon></v-app-bar-nav-icon>
            
            <v-toolbar-title>Types</v-toolbar-title>

            <v-spacer></v-spacer>

            <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-toolbar>

          <v-list>
            <v-list-group
              v-for="item in types"
              :key="item.name"
            >
            
              <template v-slot:activator>
                <v-list-item-content>
                  <v-list-item-title class="text-h5" v-text="item.name"></v-list-item-title>
                </v-list-item-content>
              </template>

              <v-list-item
                v-for="child in item.pokemon.pokemon"
                :key="child.pokemon.name"
              >
                <v-list-item-content>
                  <v-list-item-title class="text-center text-h6 indigo darken-1" v-text="child.pokemon.name"></v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-group>
          </v-list>
        </v-card>
      </v-col>
      <v-col>
        <v-card
          class="mx-auto"
        >
          <v-treeview
            rounded
            hoverable
            activatable
            color="red"
            :items="arbol"
          >
          </v-treeview>
        </v-card>
      </v-col>
      <v-col>
        <v-card
          class="mx-auto"
        >
          <v-list>
            <v-list-item>
              <v-list-item-icon>
                <v-icon>mdi-home</v-icon>
              </v-list-item-icon>

              <v-list-item-title>Home</v-list-item-title>
            </v-list-item>

            <v-list-group
              :value="true"
              prepend-icon="mdi-account-circle"
            >
              <template v-slot:activator>
                <v-list-item-title>Users</v-list-item-title>
              </template>

              <v-list-group
                :value="true"
                no-action
                sub-group
              >
                <template v-slot:activator>
                  <v-list-item-content>
                    <v-list-item-title>Admin</v-list-item-title>
                  </v-list-item-content>
                </template>

                <v-list-item
                  v-for="([title, icon], i) in admins"
                  :key="i"
                  link
                >
                  <v-list-item-title v-text="title"></v-list-item-title>

                  <v-list-item-icon>
                    <v-icon v-text="icon"></v-icon>
                  </v-list-item-icon>
                </v-list-item>
              </v-list-group>

              <v-list-group
                no-action
                sub-group
              >
                <template v-slot:activator>
                  <v-list-item-content>
                    <v-list-item-title>Actions</v-list-item-title>
                  </v-list-item-content>
                </template>

                <v-list-item
                  v-for="([title, icon], i) in cruds"
                  :key="i"
                  link
                >
                  <v-list-item-title v-text="title"></v-list-item-title>

                  <v-list-item-icon>
                    <v-icon v-text="icon"></v-icon>
                  </v-list-item-icon>
                </v-list-item>
              </v-list-group>
            </v-list-group>
          </v-list>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

  export default {
    name: 'HelloWorld',
    data: () => ({
      admins: [
        ['Management', 'mdi-account-multiple-outline'],
        ['Settings', 'mdi-cog-outline'],
      ],
      cruds: [
        ['Create', 'mdi-plus-outline'],
        ['Read', 'mdi-file-outline'],
        ['Update', 'mdi-update'],
        ['Delete', 'mdi-delete']
              ],
      types: [],
      arbol: [
        {
          id: 1,
          name: 'Applications :',
          children: [
            { id: 2, name: 'Calendar : app'},
            { id: 3, name: 'Chrome : app'},
            { id: 4, name: 'Webstorm : app'},
          ],
        },
        {
          id: 5,
          name: 'Documents :',
          children: [
            {
              id: 7,
              name: 'src :',
              children: [
                { id: 8, name: 'index : ts' },
                { id: 9, name: 'bootstrap : ts'},
              ],
            },
          ],
        },
        {
          id: 10,
          name: 'material2 :',
          children: [
            { id: 12, name: 'v-btn : ts'},
            { id: 13, name: 'v-card : ts'},
            { id: 14, name: 'v-window : ts'},
          ],
        },
        {
          id: 15,
          name: 'Downloads :',
          children: [
            { id: 16, name: 'October : pdf'},
            { id: 17, name: 'November : pdf'},
            { id: 18, name: 'Tutorial : html'},
          ],
        },
        {
          id: 19,
          name: 'Videos :',
          children: [
            {
              id: 20, name: 'Tutorials: ', children: [
                { id: 21, name: 'Basic layouts : mp4'},
                { id: 22, name: 'Advanced techniques : mp4'},
                { id: 23, name: 'All about app: dir'},
              ],
            },
            {
              id: 24, name: 'Intro: mov'
            },
            {
              id: 25, name: 'Conference introduction : avi',
            },
          ],
        },
      ],
    }),
    async mounted() {
      async function getProduct(types) {
        types = (await axios.get('https://pokeapi.co/api/v2/type')).data.results;
        for( let i = 0; i < types.length; i++){
          const pokesByType = (await axios.get(types[i].url)).data;
          types[i].pokemon = pokesByType;
        }
        return await types;
      }
      this.types = await getProduct();
      await console.log(this.types);
    }
  }
</script>
