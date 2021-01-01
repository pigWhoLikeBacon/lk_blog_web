<template>
  <el-card class="box-card">
    <div slot="header" class="clearfix">
      <span>标签</span>
      <el-button style="float: right; padding: 3px 0" type="text" @click="open()">修改</el-button>
    </div>
    <el-tag v-for="tag in article.tags" :key="tag.name" :type="tag.type">
      {{ tag.name }}
    </el-tag>
  </el-card>
</template>

<script>
import { getAllTag } from '@/api/tag'

export default {
  name: 'LabelSelector',
  data() {
    return {
      allTags: getAllTag()
    }
  }
}
</script>

<style scoped>

</style>
