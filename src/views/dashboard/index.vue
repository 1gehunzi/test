<template>
  <div style="height: 50vh; width: 50%">
    <!-- {{ pageInfo }} -->
    {{ page }} {{ total }}
    <!-- @pages-rendered="afterCreated"  -->
    <VuePdfApp page-scale="page-fit" pdf="compressed.tracemonkey-pldi-09.pdf" :config="config" @open="openHandler" @pages-rendered="onRender" />
  </div>
</template>

<script>
import VuePdfApp from 'vue-pdf-app'

export default {
  components: {
    VuePdfApp
  },
  data() {
    return {
      page: 1,
      total: 0,
      // config 用于控制pdf外观
      config: {
        secondaryToolbar: false,
        toolbar: {
          toolbarViewerLeft: {
            findbar: false,
            previous: false,
            next: false,
            pageNumber: true
          },
          toolbarViewerRight: false,
          toolbarViewerMiddle: {
            zoomOut: false,
            zoomIn: false,
            scaleSelectContainer: true
          }
        }
      }
    }
  },
  computed: {

  },
  methods: {
    onRender(pdfApp) {
      pdfApp.pdfViewer.eventBus.on('pagechanging', ({ pageNumber }) => {
        console.log('11111111111,', pageNumber)
        // TODO： 页码变化的事件
        this.page = pageNumber
      })
    },
    openHandler(pdfApp) {
      this.total = pdfApp.pagesCount
    }
  }
}
</script>

<style>
@import "https://unpkg.com/vue-pdf-app@2.0.0/dist/icons/main.css";
</style>
