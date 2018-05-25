<template>
  <div>
    <rich :content="article" />
  </div>
</template>

<script>
import rich from '../../components/mpvue-rich'

export default {
  components: {
    rich
  },
  data () {
    return {
      //富文本使用
      article: '<div style="color:red;">我是HTML代码</div><p>gfdshgfdjhgdj</p><ul><li><div style="color:red;">我是HTML代码</div></li></ul>'
    }
  }
}
</script>
<style>

</style>