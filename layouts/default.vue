<template>
  <v-app app :dark="$vuetify.theme.dark">
    <v-app-bar
      app
      flat
      py-0
      class="app-container"
      :height="$vuetify.breakpoint.sm ? 'dense' : 'prominent'"
      :color="$vuetify.theme.dark ? '#121212' : '#fff'"
    >
      <v-container px-0 pt-2 pb-0 mx-0 fluid>
        <v-row no-gutters class="hidden-sm-and-down">
          <v-col cols="4" />
          <v-col
            cols="6"
            offset="2"
            class="d-flex justify-end nowrap align-center first-header-right"
          >
            <v-btn-toggle
              v-model="fontSize"
              dense
              mandatory
              active-class="light-blue darken-2"
              class="white--text"
            >
              <v-btn v-for="(item, index) in fontSizeList" :key="index" small>
                {{ item.label }}
              </v-btn>
            </v-btn-toggle>
            <v-switch
              color="primary"
              class="elevation-0"
              message="none"
              :value="$vuetify.theme.dark"
              @change="changeMode"
            />
            <ul class="share-wrapper">
              <li v-for="(share, shareIdx) in shareList" :key="shareIdx">
                <ShareNetwork
                  :network="share.type"
                  :url="currentHref"
                  title="Vision Blog"
                >
                  <v-btn
                    icon
                    small
                    plain
                    :color="share.color ? share.color : 'blue-grey lighten-3'"
                  >
                    <v-icon>{{ share.icon }}</v-icon>
                  </v-btn>
                </ShareNetwork>
              </li>
            </ul>
          </v-col>
        </v-row>
        <v-row align-center justify-space-around dense>
          <v-col sm="6" md="3">
            <v-btn small plain color="#2163e8">
              <v-icon left>
                mdi-message-bulleted
              </v-icon>
              Vision's Blog
            </v-btn>
          </v-col>
          <v-col sm="0" md="6" align-self="center" class="hidden-sm-and-down">
            <v-toolbar-items>
              <v-menu
                v-for="(n, index) in menu"
                :key="index"
                open-on-hover
                offset-y
              >
                <template #activator="{ on, attrs }">
                  <v-btn small depressed text v-bind="attrs" v-on="on">
                    {{ n.title }}
                    <v-icon v-show="n.submenu" right>
                      mdi-chevron-down
                    </v-icon>
                  </v-btn>
                </template>
                <v-list v-if="n.submenu">
                  <v-list-item
                    v-for="(item, cIdx) in n.submenu"
                    :key="cIdx"
                    @click="() => {}"
                  >
                    <v-list-item-title> {{ item.itemTitle }}</v-list-item-title>
                  </v-list-item>
                </v-list>
              </v-menu>
            </v-toolbar-items>
          </v-col>
          <v-col sm="6" md="3">
            3
          </v-col>
        </v-row>
        <!-- <v-layout>
          <v-flex sm6 md3 shrink grow>
            <v-btn
              icon
              small
              plain
              color="#2163e8"
            >
              <v-icon left>
                mdi-message-bulleted
              </v-icon>
            </v-btn>
          </v-flex>
          <v-flex sm0 md6 class="hidden-sm-and-down">
            6
          </v-flex>
          <v-flex sm6 md3>
            3
          </v-flex>
        </v-layout> -->
        <!-- <v-row no-gutters>
          <v-col cols="4">
            4
          </v-col>
          <v-col cols="4">
            4
          </v-col>
          <v-col cols="4">
            4
          </v-col>
        </v-row> -->
      </v-container>
    </v-app-bar>
    <v-main>
      <v-container
        px-4
        py-0
        :class="[
          'app-container',
          $vuetify.breakpoint.sm ? 'app-container-dense' : 'app-container-prominent'
        ]"
      >
        <v-row>
          <v-col cols="6">
            6
          </v-col>
          <v-col cols="6">
            6
          </v-col>
        </v-row>
        <v-layout>
          <v-flex 12>
            第一行第一列
          </v-flex>
        </v-layout>
        <v-layout>
          <v-flex 6>
            第二行第一列
          </v-flex>
          <v-flex 6>
            第二行第二列
          </v-flex>
        </v-layout>
        <p v-for="(item, i) in 30" :key="i">
          {{ item }}
        </p>
      </v-container>
    </v-main>
    <!-- <v-app-bar>
      <v-toolbar-title>
        <v-img width="44" height="44" :src="require('~/assets/images/v.png')" />
      </v-toolbar-title>
      <v-toolbar-items class="hidden-sm-and-down">
        <v-menu
          v-for="(n, index) in menu"
          :key="index"
          open-on-hover
          offset-y
        >
          <template #activator="{ on, attrs }">
            <v-btn text v-bind="attrs" v-on="on">
              {{ n.title }}
            </v-btn>
          </template>

          <v-list v-if="n.submenu">
            <v-list-item
              v-for="(item, cIdx) in n.submenu"
              :key="cIdx"
              @click="() => {}"
            >
              <v-list-item-title> {{ item.itemTitle }}</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
      </v-toolbar-items>
    </v-app-bar>

    <v-app-bar class="hidden-md-and-up">
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-spacer />
      <v-navigation-drawer v-model="drawer" app>
        <v-list-item>
          <v-avatar>
            <img src="https://cdn.vuetifyjs.com/images/john.jpg" alt="John">
          </v-avatar>
          <v-list-item-content style="text-align: center;">
            <v-list-item-title class="title">
              小花瓣
            </v-list-item-title>
            <v-list-item-subtitle>
              xiaohuaban
            </v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-divider />
        <v-list>
          <v-list-item v-for="item in items" :key="item.title" router :to="item.route">
            <v-list-item-icon>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>
                {{ item.title }}
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>
    </v-app-bar>
    <v-main>
      <Nuxt />
    </v-main> -->
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      fontSize: undefined,
      fontSizeList: [
        { value: 14, label: 'A-' },
        { value: 16, label: 'A' },
        { value: 18, label: 'A+' }
      ],
      shareList: [
        { icon: 'mdi-facebook', color: '#3b5998', type: 'facebook' },
        { icon: 'mdi-twitter', color: '#00acee', type: 'twitter' },
        { icon: 'mdi-sina-weibo', color: '#DD405B', type: 'weibo' },
        { icon: 'mdi-linkedin', color: '#0e76a8', type: 'linkedin' },
        { icon: 'mdi-skype', color: '#039DDD', type: 'skype' }
      ],
      currentHref: '',
      menu: [
        {
          index: '1',
          title: 'Home',
          submenu: [
            {
              itemIndex: '1-1',
              itemTitle: 'Home Default'
            },
            {
              itemIndex: '1-2',
              itemTitle: 'Home Lazy Load'
            },
            {
              itemIndex: '1-3',
              itemTitle: 'Magazine Classic'
            },
            {
              itemIndex: '1-4',
              itemTitle: 'Home Cards'
            },
            {
              itemIndex: '1-5',
              itemTitle: 'Blog Classic'
            }
          ]
        },
        {
          index: '2',
          title: 'Pages',
          submenu: [
            {
              itemIndex: '2-1',
              itemTitle: 'About'
            },
            {
              itemIndex: '2-2',
              itemTitle: 'Contact'
            }
          ]
        },
        {
          index: '3',
          title: 'Post'
        },
        {
          index: '4',
          title: 'LifeStyle'
        },
        {
          index: '5',
          title: 'Components'
        }
      ],
      drawer: true,
      items: [
        { title: '基本信息', icon: 'mdi-contacts', route: '/Profile' },
        { title: '科研成果', icon: 'mdi-keyboard', route: '/Research' },
        { title: '校园经历', icon: 'mdi-home', route: '/College' },
        { title: '实战经历', icon: 'mdi-gavel', route: '/Project' }
      ],
      admins: [
        ['Management', 'mdi-account-multiple-outline'],
        ['Settings', 'mdi-cog-outline']
      ],
      cruds: [
        ['Create', 'mdi-plus-outline'],
        ['Read', 'mdi-file-outline'],
        ['Update', 'mdi-update'],
        ['Delete', 'mdi-delete']
      ]
    }
  },
  created () {
    if (process.browser) {
      // eslint-disable-next-line nuxt/no-globals-in-created
      this.currentHref = window.location.href
    }
  },
  methods: {
    changeMode (mode) {
      // eslint-disable-next-line no-console
      this.$vuetify.theme.dark = mode
    }
    // shareLink (type) {

    // }
  }
}
</script>

<style lang="scss" scoped>
.app-container {
  max-width: 1200px;
  margin: 0 auto;
  ::v-deep .v-input__slot {
    margin-bottom: 0px;
  }
  ::v-deep .v-messages {
    display: none !important;
  }
}
.first-header-right div {
  margin-right: 1rem;
}
.app-container-dense {
  margin-top: 56px;
}
.app-container-prominent {
  margin-top: 80px;
}
.share-wrapper {
  padding-left: 0px;
  list-style-type: none;
  li {
    display: inline-block;
    margin-right: 6px;
    a {
      text-decoration: none;
    }
  }
}
</style>
