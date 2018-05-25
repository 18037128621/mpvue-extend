<template>
  <div>
    <div class="title">矢量字体图标：</div>
    <div>
        <h3 class="h3">基本图标</h3>
        <p class="text">您可以将Font Awesome图标使用在几乎任何地方，只需要使用CSS前缀 fa ，再加上图标名称。 Font Awesome是为使用内联元素而设计的。</p>
        <i class="fa fa-camera-retro"></i> fa-camera-retro
    </div>
    <div>
        <h3 class="h3">大图标</h3>
        <p class="text">使用 fa-lg (33%递增)、fa-2x、 fa-3x、fa-4x，或者 fa-5x 类 来放大图标。</p>
        <i class="fa fa-camera-retro fa-lg"></i> fa-lg<br />
        <i class="fa fa-camera-retro fa-2x"></i> fa-2x<br />
        <i class="fa fa-camera-retro fa-3x"></i> fa-3x<br />
        <i class="fa fa-camera-retro fa-4x"></i> fa-4x<br />
        <i class="fa fa-camera-retro fa-5x"></i> fa-5x<br />
    </div>
    <div class="link">更多关于字体演示请访问http://fontawesome.dashgame.com/#wrap</div>
    <button>去学语法</button>
  </div>
</template>
<script>
  

  export default {
    components: {
    },
    data() {
      return {
      };
    },
    mounted() {
    },
    computed: {
      
    },
    methods: {
    },
  };
</script>
<style>
.h3{
    font-size:18px;
    height:40px;
    line-height:40px;
    font-weight:600;
    border-bottom:1px solid #ccc;
}
.text{
    color:#999;
    font-size:12px;
    border-bottom:1px solid #ccc;
}
.link{
    color:#00BC00;
    font-size:12px;
}
</style>
