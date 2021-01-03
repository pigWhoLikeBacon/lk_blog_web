<template>
  <div>
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <span>标签</span>
        <el-button style="float: right; padding: 3px 0" type="text" @click="dialogVisible = true">修改</el-button>
      </div>
      <Tags :tags="selectedTags" />
    </el-card>
    <el-dialog
      title="提示"
      :visible.sync="dialogVisible"
      width="30%"
    >
      <el-card class="box-card">
        <Tags :tags="unselectedTags" />
      </el-card>
      <el-card class="box-card">
        <Tags :tags="selectedTags" />
      </el-card>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
// import { getAllTag } from '@/api/tag'
import Tags from './tags.vue'

export default {
  name: 'TagSelector',
  components: { Tags },
  props: {
    selectedTags: {
      type: Array,
      default: null
    },
    allTags: {
      type: Array,
      default: null
    }
  },
  data() {
    return {
      dialogVisible: false
    }
  },
  computed: {
    // 找出未被选择的tag
    unselectedTags: function() {
      var unselectedTags = []
      for (var tag1 of this.allTags) {
        var flag = true
        for (var tag2 of this.selectedTags) {
          if (tag1.id === tag2.id) {
            flag = false
            break
          }
        }
        if (flag) {
          unselectedTags.push(tag1)
        }
      }
      return unselectedTags
    }
  }
}
</script>

<style scoped>

</style>
