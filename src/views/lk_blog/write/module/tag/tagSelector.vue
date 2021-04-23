<template>
  <div>
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <span>标签</span>
        <el-button style="float: right; padding: 3px 0" type="text" @click="dialogVisible = true">修改</el-button>
      </div>
      <el-tag
        v-for="tag in selectedTags"
        :key="tag.id"
        :type="tag.color"
        class="tag"
      >
        {{ tag.content }}
      </el-tag>
    </el-card>
    <el-dialog
      title="选择标签"
      :visible.sync="dialogVisible"
      width="30%"
    >
      <p>未选择</p>
      <el-card class="box-card">
        <el-tag
          v-for="tag in unselectedTags"
          :key="tag.id"
          :type="tag.color"
          class="stag tag"
          @click="select(tag)"
        >
          {{ tag.content }}
        </el-tag>
      </el-card>
      <p>已选择</p>
      <el-card class="box-card">
        <el-tag
          v-for="tag in selectedTags"
          :key="tag.id"
          :type="tag.color"
          class="stag tag"
          @click="unselect(tag)"
        >
          {{ tag.content }}
        </el-tag>
      </el-card>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
<!--        <el-button type="primary" @click="reset()">重 置</el-button>-->
      </span>
    </el-dialog>
  </div>
</template>

<script>
// import { getAllTag } from '@/api/tag'

export default {
  name: 'TagSelector',
  props: {
    selectedTags: {
      type: Array,
      default: function() {
        return []
      }
    },
    allTags: {
      type: Array,
      default: function() {
        return []
      }
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
  },
  methods: {
    select: function(tag) {
      this.selectedTags.push(tag)
    },
    unselect: function(tag) {
      for (var tagId in this.selectedTags) {
        if (tag.id === this.selectedTags[tagId].id) {
          this.selectedTags.splice(tagId, 1)
          break
        }
      }
    },
    reset: function() {
    }
  }
}
</script>

<style scoped>
.stag {
  cursor: pointer;
}

.tag {
  margin-right: 12px;
}

</style>
