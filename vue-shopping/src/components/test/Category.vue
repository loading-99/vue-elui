<template>
  <div>
    <slot :games="games"></slot>
  </div>
</template>
<script>
export default {
  name: 'Category',
  props: ['title'],
  // 数据在子组件自身
  data() {
    return {
      games: ['红色警戒', '穿越火线', '劲舞团', '超级玛丽']
    }
  }
}
</script>