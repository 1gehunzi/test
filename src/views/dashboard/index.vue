<template>
  <div style="height: 100vh; width: 80%">
    <!-- {{ pageInfo }} -->
    {{ page }} {{ total }}
    <!-- @pages-rendered="afterCreated"  -->
    <VuePdfApp page-scale="page-fit" :pdf="pdf" :config="config" @open="openHandler" @pages-rendered="onRender" />
  </div>
</template>

<script>
import VuePdfApp from 'vue-pdf-app'
import 'vue-pdf-app/dist/icons/main.css'
export default {
  components: {
    VuePdfApp
  },
  data() {
    return {
      page: 1,
      total: 0,
      pdf: null,
      // config 用于控制pdf外观
      config: {
        // secondaryToolbar: true,
        secondaryToolbar: {
          secondaryPresentationMode: false,
          secondaryOpenFile: false,
          secondaryPrint: false,
          secondaryDownload: false,
          secondaryViewBookmark: true,
          firstPage: true,
          lastPage: true,
          pageRotateCw: true,
          pageRotateCcw: true,
          cursorSelectTool: true,
          cursorHandTool: true,
          scrollVertical: true,
          scrollHorizontal: true,
          scrollWrapped: true,
          spreadNone: true,
          spreadOdd: true,
          spreadEven: true,
          documentProperties: true
        },
        toolbar: {
          toolbarViewerLeft: {
            findbar: true,
            previous: true,
            next: true,
            pageNumber: true
          },
          // toolbarViewerRight: true,
          toolbarViewerRight: {
            presentationMode: true,
            openFile: false,
            print: false,
            download: false,
            viewBookmark: true
          },
          toolbarViewerMiddle: {
            zoomOut: true,
            zoomIn: true,
            scaleSelectContainer: true
          }
        }
      }
    }
  },
  computed: {
  },
  mounted() {
    this.fetch()
  },
  methods: {
    async fetch() {
      const result = await fetch('https://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf')
      const blob = await result.blob()
      this.pdf = await blob.arrayBuffer()
    },
    onRender(pdfApp) {
      pdfApp.pdfViewer.eventBus.on('pagechanging', ({ pageNumber }) => {
        console.log('11111111111,', pageNumber)
        // TODO： 页码变化的事件
        if (this.page < this.total) {
          this.page = pageNumber
        }
      })
    },
    openHandler(pdfApp) {
      console.log(pdfApp, '--------')
      this.total = pdfApp.pagesCount
    }
  }
}
</script>

<style>
/* @import "https://unpkg.com/vue-pdf-app@2.0.0/dist/icons/main.css"; */
/* @import "https://unpkg.com/vue-pdf-app@2.0.0/dist/icons/main.css"; */
/* @import "vue" */
* { touch-action: pan-y; }
</style>
