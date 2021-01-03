<template>
  <div>
    <el-tag
      v-for="tag in tags"
      :key="tag.id"
      :type="tag.color"
    >
      {{ tag.content }}
    </el-tag>
  </div>
</template>

<script>
export default {
  name: 'Tags',
  props: {
    tags: {
      type: Array,
      default: null
    }
  }
}
</script>

<style scoped>

</style>
