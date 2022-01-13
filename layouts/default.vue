<template>
  <v-app app :dark="$vuetify.theme.dark">
    <v-app-bar
      app
      flat
      py-0
      class="app-container"
      :height="$vuetify.breakpoint.smAndDown ? 'dense' : 'prominent'"
      :dark="$vuetify.theme.dark"
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
          <v-col sm="0" md="6" class="hidden-sm-and-down">
            <v-toolbar-items class="header-toolbar">
              <v-menu
                v-for="(n, index) in menu"
                :key="index"
                open-on-hover
                close-on-content-click
                offset-y
              >
                <template #activator="{ on, attrs }">
                  <!-- <v-btn
                    small
                    depressed
                    text
                    bottom
                    plain
                    v-bind="attrs"
                    v-on="on"
                  >
                    {{ n.title }}
                    <v-icon v-show="n.submenu" right>
                      mdi-chevron-down
                    </v-icon>
                  </v-btn> -->
                  <v-list-item
                    dense
                    tag="span"
                    v-bind="attrs"
                    v-on="on"
                  >
                    <v-list-item-title>
                      {{ n.title }}
                      <v-icon v-show="n.submenu">
                        mdi-chevron-down
                      </v-icon>
                    </v-list-item-title>
                    <!-- <v-list-item-icon v-show="n.submenu" right>
                      <v-icon>
                        mdi-chevron-down
                      </v-icon>
                    </v-list-item-icon> -->
                  </v-list-item>
                </template>
                <v-list v-if="n.submenu" nav dense>
                  <v-list-item
                    v-for="(item, cIdx) in n.submenu"
                    :key="cIdx"
                    :to="item.path"
                    dense
                    tag="span"
                  >
                    <v-list-item-title> {{ item.itemTitle }}</v-list-item-title>
                  </v-list-item>
                </v-list>
              </v-menu>
            </v-toolbar-items>
          </v-col>
          <v-col sm="6" md="3" mr-1 class="d-flex justify-end nowrap align-center second-header-right">
            <v-btn icon small plain @click="showSearch = !showSearch">
              <v-icon>mdi-magnify</v-icon>
            </v-btn>
            <v-text-field
              v-show="showSearch"
              solo
              hide-details
              label="Filled"
              prepend-inner-icon="mdi-magnify"
              :height="28"
              style="border-radius: 16px;"
            />
            <v-btn icon small plain @click="aboutMeDialog = !aboutMeDialog">
              <v-icon>mdi-menu</v-icon>
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-app-bar>
    <v-main>
      <v-container
        px-0
        py-1
        full-height
        :class="[
          'app-container',
          $vuetify.breakpoint.smAndDown ? 'app-container-dense' : 'app-container-prominent'
        ]"
      >
        <Nuxt />
        <!-- <v-row>
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
        </p> -->
      </v-container>
    </v-main>
    <v-navigation-drawer
      v-model="aboutMeDialog"
      app
      absolute
      right
      temporary
      :width="$vuetify.breakpoint.smAndDown ? '90%' : '400px'"
    >
      <div
        style="height: 100vh;"
        class="d-flex flex-column overflow-hidden"
      >
        <div class="d-flex align-center justify-end flex-nowrap px-4 py-4">
          <v-btn icon small plain @click="aboutMeDialog = !aboutMeDialog">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </div>
        <div class="d-flex overflow-hidden">
          <div class="about-content">
            <v-container>
              <v-row dense>
                <v-col cols="12">
                  <v-card
                    color="blue-grey"
                  >
                    <v-card-title class="text-h6">
                      Base Info
                    </v-card-title>
                    <v-row align="center" dense>
                      <v-col v-for="(info, index) in profile" :key="index" cols="12">
                        <v-icon class="pl-8">
                          {{ info.icon }}
                        </v-icon>
                        <v-card-subtitle tag="span">
                          <a v-if="info.link" :href="info.href">{{ info.info }}</a>
                          <span v-else>{{ info.info }}</span>
                        </v-card-subtitle>
                      </v-col>
                    </v-row>
                  </v-card>
                </v-col>
                <v-col cols="12" class="pt-4">
                  <v-card color="red lighten-5">
                    <v-card-title class="text-h6">
                      Skills
                    </v-card-title>
                    <v-row
                      dense
                      class="d-flex flex-wrap px-2 skill-wrapper"
                    >
                      <v-col v-for="(sk, index) in skills" :key="index">
                        <!-- <v-img
                          :width="80"
                          :alt="sk.alt"
                          :src="sk.src"
                        /> -->
                        <a href="#">
                          <img :src="sk.src" alt="sk.alt" width="80px">
                        </a>
                      </v-col>
                    </v-row>
                  </v-card>
                </v-col>
              </v-row>
            </v-container>
          </div>
        </div>
        <!-- <div style="flex: 1;display: flex; overflow: hidden;">
          <div style="height: 100%; width: 100%; overflow-y: auto;">
            <h1 v-for="(item, idx) in 30" :key="idx">
              123
            </h1>
          </div>
        </div> -->
      </div>
    </v-navigation-drawer>
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
              itemTitle: 'Home Default',
              path: '/'
            },
            {
              itemIndex: '1-2',
              itemTitle: 'Home Lazy Load',
              path: '/lazy'
            },
            {
              itemIndex: '1-3',
              itemTitle: 'Magazine Classic',
              path: '/magazine'
            },
            {
              itemIndex: '1-4',
              itemTitle: 'Home Cards',
              path: '/cards'
            },
            {
              itemIndex: '1-5',
              itemTitle: 'Blog Classic',
              path: '/classic'
            }
          ]
        },
        {
          index: '2',
          title: 'Pages',
          submenu: [
            {
              itemIndex: '2-1',
              itemTitle: 'About',
              path: '/about'
            },
            {
              itemIndex: '2-2',
              itemTitle: 'Contact',
              path: '/contact'
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
      aboutMeDialog: false,
      drawer: true,
      profile: [
        { icon: 'mdi-school-outline', info: 'Bachelor Degree' },
        { icon: 'mdi-professional-hexagon', info: 'Web Developer' },
        { icon: 'mdi-map-marker-radius-outline', info: 'ShenZhen, China' },
        { icon: 'mdi-email-send-outline', info: 'askxuefeng@gmail.com', link: true, href: 'www.baidu.com' },
        { icon: 'mdi-link-variant', info: 'www.baidu.com', link: true, href: 'www.baidu.com' }
      ],
      skills: [
        { alt: 'nuxt', src: '/images/nuxt.png' },
        { alt: 'laravel', src: '/images/laravel.png' },
        { alt: 'passionate', src: '/images/passionate_people.png' },
        { alt: 'vehikl', src: '/images/vehikl.png' },
        { alt: 'vueschool', src: '/images/vueschool.png' }
      ],
      showSearch: false,

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
    // aboutMe () {
    //   this.aboutMeDialog = !this.aboutMeDialog
    // }
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
.header-toolbar {
  height: 28px;
  line-height: 28px;
  // .v-btn:hover {
  //   color: #2163e8;
  // }
  ::v-deep .v-list-item--dense, .v-list--dense .v-list-item {
    min-height: 28px;
  }
}
.second-header-right button {
  margin-right: 6px;
}
.second-header-right {
  ::v-deep .v-text-field.v-text-field--solo .v-input__control {
    min-height: 28px;
  }
}
.about-content {
  width: 100%;
  height: 100%;
  overflow-y: auto;
  a {
    text-decoration: none;
  }
}
.skill-wrapper img {
  -webkit-filter: grayscale(100%); /* Webkit */
  filter: gray; /* IE6-9 */
  filter: grayscale(100%); /* W3C */
  &:hover {
    filter:none;
    transition:all .5s;
  }
}
</style>
