<template>
  <v-app id="sandbox">
    <v-navigation-drawer
      v-model="primaryDrawer.model"
      :clipped="primaryDrawer.clipped"
      permanent
      :mini-variant="primaryDrawer.mini"
      app
      overflow
    >
      <center><v-img class="avatar" src="https://randomuser.me/api/portraits/men/85.jpg"></v-img></center>
      <v-divider></v-divider>

      <v-list
        dense
        nav
      >
        <v-list-item
          v-for="item in items"
          :key="item.title"
          link
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="primaryDrawer.clipped"
      app
    > 
    <v-icon @click="is_permanent">
      fas fa-bars
    </v-icon>
      <v-toolbar-title>&nbsp;&nbsp;Vuetify</v-toolbar-title>
    </v-app-bar>

    <v-content>
      <v-container fluid>
        <v-row
          align="center"
          justify="center"
        >
          <v-col cols="12">
            <dashboard></dashboard>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>
<style scoped>
  .avatar{
    margin-top:50px;
    margin-bottom:50px;
    height:100px;
    width:95px;
    border-radius:50px;
  }
</style>

<script>

  import dashboard from "../components/dashboard.vue";

  export default {
    components:{
      dashboard,
    },
    data: () => ({
      items: [
          { title: 'Dashboard', icon: 'fas fa-tachometer-alt' },
          { title: 'Buses', icon: 'fas fa-bus-alt'},
          { title: 'Schedules', icon: 'far fa-calendar-alt' },
          { title: 'My Account', icon: 'far fa-user-circle' },
        ],
      primaryDrawer: {
        model: null,
        clipped: true,
        mini:true
      },
     
    }),
    methods:{
      is_permanent(){
        this.primaryDrawer.mini = !this.primaryDrawer.mini;
      }
    }
  }
</script>