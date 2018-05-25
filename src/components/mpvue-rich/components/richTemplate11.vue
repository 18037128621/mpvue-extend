<template>
  <!--判断是否是标签节点-->
  <block v-if="node.node == 'element'">
    <!--button类型-->
    <block v-if="node.tag == 'button'">
      <button type="default" size="mini">
      </button>
    </block>

    <!--li类型-->
    <block v-else-if="node.tag == 'li'">
      <view :class="node.classStr" :style="node.styleStr">
        {{node.text}}
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
        {{node.text}}
      </view>
    </block>

    <!--br类型-->
    <block v-else-if="node.tag == 'br'">
      <text>\n</text>
    </block>

    <!--其他标签-->
    <block v-else>
      <view :class="node.classStr" :style="node.styleStr">
        {{node.text}}
      </view>
    </block>
  </block>

  <!--判断是否是文本节点-->
  <block v-else-if="node.node == 'text'">{{node.text}}</block>
</template>

<script>
import Vue from 'vue';
const bus = new Vue();
import richImg from './richImg';
import richVideo from './richVideo';

export default {
  name: 'richTemplate11',
  props: {
    node: {},
  },
  components: {
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
