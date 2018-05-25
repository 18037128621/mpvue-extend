<template>
  <!--判断是否是标签节点-->
  <block v-if="node.node == 'element'">
    <block v-if="node.tag == 'button'">
      <button type="default" size="mini">
        <block v-for="(node, index) of node.nodes" :key="index">
          <rich-template :node="node" />
        </block>
      </button>
    </block>

    <!--li类型-->
    <block v-else-if="node.tag == 'li'">
      <view :class="node.classStr" :style="node.styleStr">
        <block v-for="(node, index) of node.nodes" :key="index">
          <rich-template :node="node" />
        </block>
      </view>
    </block>

    <!--video类型-->
    <block v-else-if="node.tag == 'video'">
      <rich-video :node="node" />
    </block>

    <!--img类型-->
    <block v-else-if="node.tag == 'img'">
      <rich-img :node="node" />
    </block>

    <!--a类型-->
    <block v-else-if="node.tag == 'a'">
      <view @click="richATap" :class="node.classStr" :data-href="node.attr.href" :style="node.styleStr">
        <block v-for="(node, index) of node.nodes" :key="index">
          <rich-template :node="node" />
        </block>
      </view>
    </block>

    <!--br类型-->
    <block v-else-if="node.tag == 'br'">
      <text>\n</text>
    </block>

    <!--其他标签-->
    <block v-else>
      <view :class="node.classStr" :style="node.styleStr">
        <block v-for="(node, index) of node.nodes" :key="index">
          <rich-template :node="node" />
        </block>
      </view>
    </block>

  </block>

  <!--判断是否是文本节点-->
  <block v-else-if="node.node == 'text'">{{node.text}}</block>
</template>

<script>
import Vue from 'vue';
const bus = new Vue();
import richTemplate from './richTemplate4';
import richImg from './richImg';
import richVideo from './richVideo';

export default {
  name: 'richTemplate3',
  props: {
    node: {},
  },
  components: {
    richTemplate,
    richImg,
    richVideo,
  },
  methods: {
    richATap(e) {
      const { href } = e.target.dataset;
      if (!href) return;
      bus.$emit('navigate', href);
    },
  },
};
</script>
