<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="文章封面" prop="cover">
            <el-input v-model="form.cover" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="简介" prop="introduce">
            <el-input v-model="form.introduce" :rows="3" type="textarea" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="题目" prop="title">
            <el-input v-model="form.title" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="内容" prop="content">
            <el-input v-model="form.content" :rows="3" type="textarea" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="是否展示" prop="isShow">
            <el-radio v-model="form.isShow" v-for="item in dict.article_show" :key="item.id" :label="item.value">{{ item.label }}</el-radio>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button type="text" @click="crud.cancelCU">取消</el-button>
          <el-button :loading="crud.cu === 2" type="primary" @click="crud.submitCU">确认</el-button>
        </div>
      </el-dialog>
      <!--表格渲染-->
      <el-table ref="table" v-loading="crud.loading" :data="crud.data" size="small" style="width: 100%;" @selection-change="crud.selectionChangeHandler">
        <el-table-column type="selection" width="55" />
        <el-table-column prop="articleId" label="ID" />
        <el-table-column prop="cover" label="文章封面" />
        <el-table-column prop="introduce" label="简介" />
        <el-table-column prop="title" label="题目" />
        <el-table-column prop="content" label="内容" />
        <el-table-column prop="views" label="浏览量" />
        <el-table-column prop="isShow" label="是否展示">
          <template slot-scope="scope">
            {{ dict.label.article_show[scope.row.isShow] }}
          </template>
        </el-table-column>
        <el-table-column prop="createTime" label="创建日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.createTime) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="updateTime" label="更新时间">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.updateTime) }}</span>
          </template>
        </el-table-column>
        <el-table-column v-permission="['admin','article:edit','article:del']" label="操作" width="150px" align="center">
          <template slot-scope="scope">
            <udOperation
              :data="scope.row"
              :permission="permission"
            />
          </template>
        </el-table-column>
      </el-table>
      <!--分页组件-->
      <pagination />
    </div>
  </div>
</template>

<script>
import crudArticle from '@/api/article'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { articleId: null, cover: null, introduce: null, title: null, content: null, views: null, isShow: null, createTime: null, updateTime: null }
export default {
  name: 'Article',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  dicts: ['article_show'],
  cruds() {
    return CRUD({ title: '文章', url: 'api/article', idField: 'articleId', sort: 'articleId,desc', crudMethod: { ...crudArticle }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'article:add'],
        edit: ['admin', 'article:edit'],
        del: ['admin', 'article:del']
      },
      rules: {
        cover: [
          { required: true, message: '文章封面不能为空', trigger: 'blur' }
        ],
        introduce: [
          { required: true, message: '简介不能为空', trigger: 'blur' }
        ],
        title: [
          { required: true, message: '题目不能为空', trigger: 'blur' }
        ],
        content: [
          { required: true, message: '内容不能为空', trigger: 'blur' }
        ],
        isShow: [
          { required: true, message: '是否展示不能为空', trigger: 'blur' }
        ]
      }    }
  },
  methods: {
    // 钩子：在获取表格数据之前执行，false 则代表不获取数据
    [CRUD.HOOK.beforeRefresh]() {
      return true
    }
  }
}
</script>

<style scoped>

</style>
