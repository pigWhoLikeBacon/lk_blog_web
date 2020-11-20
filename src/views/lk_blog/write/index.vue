<template>
  <div class="app-container">
    <el-row :gutter="10">
      <el-col :span="24" :sm="18">
        <mavon-editor ref="md" :style="'height:' + height" @imgAdd="imgAdd" />
      </el-col>
      <el-col :span="24" :sm="6">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>卡片名称</span>
            <el-button style="float: right; padding: 3px 0" type="text">操作按钮</el-button>
          </div>
          <div v-for="o in 4" :key="o" class="text item">
            {{ '列表内容 ' + o }}
          </div>
        </el-card>
      </el-col>
    </el-row>
    <input type="button" value="Post" @click="post()">
  </div>
</template>

<script>
import { upload } from '@/utils/upload'
import { mapGetters } from 'vuex'
import request from '@/utils/request'
export default {
  name: 'Markdown',
  data() {
    return {
      height: document.documentElement.clientHeight - 200 + 'px'
    }
  },
  computed: {
    ...mapGetters([
      'imagesUploadApi',
      'baseApi'
    ])
  },
  mounted() {
    const that = this
    window.onresize = function temp() {
      that.height = document.documentElement.clientHeight - 200 + 'px'
    }
  },
  methods: {
    imgAdd(pos, $file) {
      upload(this.imagesUploadApi, $file).then(res => {
        const data = res.data
        const url = this.baseApi + '/file/' + data.type + '/' + data.realName
        this.$refs.md.$img2Url(pos, url)
      })
    },
    post: function() {
      window.alert(request({
        url: 'api/article',
        method: 'get'
      }))
      window.alert(request({
        url: 'api/article',
        method: 'post',
        data: {"articleId":null,"cover":"1234ghhnjfdghjkfdgyhjkdftytyuj","introduce":"1234","title":"1234","content":"1234","views":null,"isShow":"true","createTime":null,"updateTime":null}
      }))
    }
  }
}
</script>

<style scoped>
.el-col {
  border-radius: 4px;
}

.text {
  font-size: 14px;
}

.item {
  margin-bottom: 18px;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both
}

.box-card {
  width: 100%;
}
</style>
