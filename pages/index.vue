<template>
  <div>
    <v-sheet color="rgba(33, 99, 232, 0.1)" class="d-flex align-center justify-start pa-1 rounded-8">
      <div class="d-flex align-center justify-start">
        <div>
          <v-chip
            small
            label
            color="indigo"
            text-color="white"
            class="mr-4"
          >
            Trending:
          </v-chip>
        </div>
        <v-carousel
          cycle
          height="100%"
          hide-delimiters
          hide-delimiter-background
          :show-arrows="false"
        >
          <div class="d-flex align-center justify-start">
            <v-carousel-item
              v-for="(slide, i) in slides"
              :key="i"
            >
              {{ slide }} Slide dfadsaf fdsafasfdsf ffasfweffdafda dfadwerwrwerewferererere
            </v-carousel-item>
          </div>
          <v-btn slot="prev" icon>
            mdi-account-multiple-outline
          </v-btn>
          <v-btn slot="next" icon>
            mdi-account-multiple-outline
          </v-btn>
        </v-carousel>
      </div>
    </v-sheet>
    <!-- <v-carousel
      cycle
      height="32"
      hide-delimiters
      hide-delimiter-background
      :show-arrows="false"
      class="rounded-8"
    >
      <v-carousel-item
        v-for="(slide, i) in slides"
        :key="i"
        eager
      >
        <v-sheet color="rgba(33, 99, 232, 0.1)" class="d-flex align-center justify-space-between pa-1">
          <div class="d-flex align-center justify-start">
            <v-chip
              small
              label
              color="indigo"
              text-color="white"
              class="mr-2"
            >
              Trending:
            </v-chip>
            <span>{{ slide }} Slide</span>
          </div>
          <div>
            right
          </div>
        </v-sheet>
      </v-carousel-item>
    </v-carousel> -->
    <span>12</span>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data: () => ({
    isMobile: false,
    dialog: false,
    colors: [
      'indigo',
      'warning',
      'pink darken-2',
      'red lighten-1',
      'deep-purple accent-4'
    ],
    slides: [
      'First',
      'Second',
      'Third',
      'Fourth',
      'Fifth'
    ]
  }),
  computed: {
    theme () {
      return this.$vuetify.theme.isDark ? 'dark' : 'light'
    }
  },
  mounted () {
    this.onResize()
    window.addEventListener('resize', this.onResize, { passive: true })
  },
  beforeDestroy () {
    if (typeof window !== 'undefined') {
      window.removeEventListener('resize', this.onResize, { passive: true })
    }
  },
  methods: {
    touchAll () {
      const value = this.$vuetify.theme.themes[this.theme]
      this.$vuetify.theme.themes[this.theme] = {}
      this.$vuetify.theme.themes[this.theme] = value
    },

    swap () {
      this.$vuetify.theme.isDark = !this.$vuetify.theme.isDark

      // necessary to reset colors after changing the theme, perhaps a Vuetify.js bug
      this.touchAll(1)
    },

    onResize () {
      this.isMobile = window.innerWidth < 600
    }
  }
}
</script>
