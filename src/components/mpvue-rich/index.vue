<template>
<!--基础元素-->
<div class="richText" :class="className">
  <block v-for="node of nodes" :key="node.index">
    <richTemplate :node="node" />
  </block>
</div>
</template>

<script>
import Vue from 'vue';
const bus = new Vue();
import HtmlToJson from './libs/html2json';

import richTemplate from './components/richTemplate0';

export default {
  name: 'rich',
  props: {
    className: {
      type: String,
      default: '',
    },
    content: {
      type: String,
      default: '',
    },
    noData: {
      type: String,
      default: '<div style="color: red;">数据不能为空</div>',
    },
    startHandler: {
      type: Function,
      default: null,
    },
    endHandler: {
      type: Function,
      default: null,
    },
    charsHandler: {
      type: Function,
      default: null,
    },
    imageProp: {
      type: Object,
      default() {
        return {
          mode: 'aspectFit',
          padding: 0,
          lazyLoad: false,
        };
      },
    },
  },
  components: {
    richTemplate,
  },
  data() {
    return {
      imageUrls: [],
    };
  },
  computed: {
    nodes() {
      const {
        content,
        noData,
        imageProp,
        startHandler,
        endHandler,
        charsHandler,
      } = this;
      const parseData = content || noData;
      const customHandler = {
        start: startHandler,
        end: endHandler,
        chars: charsHandler,
      };
      const { nodes, imageUrls } = HtmlToJson(parseData, customHandler, imageProp);
      this.imageUrls = imageUrls.map(url => url);
      return nodes;
    },
  },
  mounted() {
    this.initEvents();
  },
  methods: {
    initEvents() {
      bus.$on('navigate', this.navigate);
      bus.$on('preview', this.preview);
      bus.$on('nopreview', this.removeImageUrl);
    },
    navigate(href) {
      this.$emit('navigate', href);
    },
    preview(src) {
      if (!this.imageUrls.length) return;
      wx.previewImage({
        current: src,
        urls: this.imageUrls,
      });
      this.$emit('preview', src);
    },
    removeImageUrl(src) {
      const { imageUrls } = this;
      imageUrls.splice(imageUrls.indexOf(src), 1);
    },
  },
};
</script>
