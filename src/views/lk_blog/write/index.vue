<template>
  <div class="app-container">
    <el-row :gutter="10">
      <el-col :span="24" :sm="18">
        <mavon-editor ref="md" :style="'height:' + height" @imgAdd="imgAdd" v-model="article.content"/>
      </el-col>
      <el-col :span="24" :sm="6">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>卡片名称</span>
            <el-button style="float: right; padding: 3px 0" type="text">操作按钮</el-button>
          </div>
          <div class="text item">
            <el-form ref="form" :label-position="'top'" :model="form" label-width="80px">
              <el-form-item>
                <el-input v-model="article.id" placeholder="">
                  <template slot="prepend">ID</template>
                </el-input>
              </el-form-item>
              <el-form-item>
                <el-input v-model="article.title" placeholder="">
                  <template slot="prepend">标题</template>
                </el-input>
              </el-form-item>
              <el-form-item label="cover">
                <el-image
                  style="width: 100px; height: 100px"
                  :src="article.cover"
                  :preview-src-list="[article.cover]"
                  :fit="'cover'"
                />
              </el-form-item>
              <el-form-item>
                <TagSelector :selected-tags="article.tags" :all-tags="allTags" />
              </el-form-item>
              <el-form-item label="简介">
                <el-input v-model="article.introduce" type="textarea" />
              </el-form-item>
              <el-form-item>
                <label class="el-form-item__label" style="padding: 0;">是否展示&nbsp;</label>
                <el-switch v-model="article.isShow" />
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="post()">提交</el-button>
                <el-button>取消</el-button>
              </el-form-item>
            </el-form>
          </div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import { upload } from '@/utils/upload'
import { mapGetters } from 'vuex'
import TagSelector from './module/tag/tagSelector.vue'
import { getAllTag } from '@/api/tag'
import { add, edit, getArticle } from '@/api/article'
export default {
  name: 'Write',
  components: { TagSelector },
  data() {
    return {
      height: document.documentElement.clientHeight - 200 + 'px',
      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      },
      allTags: [],
      article: {
        id: '',
        cover: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg',
        introduce: '',
        title: '',
        content: '',
        views: 0,
        isShow: true,
        tags: []
      }
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
    const query = this.$route.query
    window.onresize = function temp() {
      that.height = document.documentElement.clientHeight - 200 + 'px'
    }
    getAllTag().then(res => {
      this.allTags = res.content
    }).catch(() => { })
    if (typeof (query.id) !== 'undefined') {
      const params = {
        page: 0,
        size: 1,
        id: query.id
      }
      getArticle(params).then(res => {
        this.article = res.content[0]
      }).catch(() => { })
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
      if (this.article.id === '') {
        add(this.article)
      } else {
        edit(this.article)
      }
    }
  }
}
</script>

<style scoped>
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
  clear: both;
}

.box-card {
  width: 100%;
}
</style>
