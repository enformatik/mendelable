<template>
  <div class="c-element-profile">
    <div class="c-masonry-grid" ref="c-masonry-grid">
      <div class="c-masonry-grid__sizer"></div>

      <div class="c-masonry-grid__item c-masonry-grid__item--width-75">
        <element-general-properties :element="element"></element-general-properties>
      </div>

      <div class="c-masonry-grid__item">
        <element-atomic-properties :element="element"></element-atomic-properties>
      </div>

      <div class="c-masonry-grid__item">
        <div v-if="element.boilingPoint && element.boilingPoint['C'] && element.meltingPoint && element.meltingPoint['C']" class="card">
          <div v-if="Object.keys(element).length > 0" class="card-block">
            <h6 class="card-title text-uppercase font-weight-bold">
              <small>{{ $t("element.physicalProperties") }}</small>
            </h6>

            <bar-chart :element="element" class="mt-5"></bar-chart>
          </div>
        </div>
      </div>

      <div class="c-masonry-grid__item c-masonry-grid__item--width-100">
        <element-historical-properties :element="element"></element-historical-properties>
      </div>
    </div>
  </div>
</template>

<script>
  import { mapState, mapGetters } from 'vuex'
  import Masonry from 'masonry-layout'
  import ElementGeneralProperties from './ElementProfile/ElementGeneralProperties'
  import ElementAtomicProperties from './ElementProfile/ElementAtomicProperties'
  import ElementHistoricalProperties from './ElementProfile/ElementHistoricalProperties'
  import BarChart from './shared/BarChart'

  export default {
    name: 'element-profile',
    components: {
      BarChart, ElementGeneralProperties, ElementAtomicProperties, ElementHistoricalProperties
    },
    mounted: function () {
      this.renderGrid()
    },
    watch: {
      element: function () {
        this.renderGrid()
      },
      debug: function () {
        this.renderGrid()
      }
    },
    methods: {
      renderGrid: function () {
        this.$nextTick(() => {
          let ctx = this.$refs['c-masonry-grid']

          // eslint-disable-next-line no-new
          new Masonry(ctx, {
            itemSelector: '.c-masonry-grid__item',
            columnWidth: '.c-masonry-grid__sizer',
            percentPosition: true
          })
        })
      }
    },
    computed: {
      ...mapGetters({
        elements: 'localizedElements'
      }),
      element () {
        return this.elements[this.$route.params.id]
      },
      ...mapState({
        debug: state => state.configuration.debug
      })
    }
  }
</script>

<style lang="scss" scoped>
  @import "../assets/scss/bootstrap/_variables";

  .c-element-profile {
    margin: -0.5em;

    .card {
      overflow: auto;
      margin: 0.5em;
    }
  }

  .c-masonry-grid__sizer, .c-masonry-grid__item {
    width: 100%;

    @media (min-width: map-get($grid-breakpoints, lg)) {
      width: 40%;
    }

    @media (min-width: map-get($grid-breakpoints, xl)) {
      width: 25%;
    }
  }

  .c-masonry-grid__item--width-75 {
    width: 100%;

    @media (min-width: map-get($grid-breakpoints, lg)) {
      width: 60%;
    }

    @media (min-width: map-get($grid-breakpoints, xl)) {
      width: 75%;
    }
  }

  .c-masonry-grid__item--width-100 {
    width: 100%;
  }
</style>
