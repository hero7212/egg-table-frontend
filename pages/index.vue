<template>
  <div class="container">
    <el-row>
      <el-col>
        <el-button @click="open('新增')">新增</el-button>
      </el-col>
    </el-row>
    <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column
        prop="name"
        label="姓名"
        width="180">
      </el-table-column>
      <el-table-column
        prop="age"
        label="年龄"
        width="180">
      </el-table-column>
      <el-table-column
        prop="sex"
        label="性别">
      </el-table-column>
    </el-table>
    <el-dialog
      :title="dialogTitle"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="close">
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="姓名">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="年龄">
          <el-input type="number" v-model="form.age"></el-input>
        </el-form-item>
        <el-form-item label="性别">
          <el-radio-group v-model="form.sex">
            <el-radio :label="0">男</el-radio>
            <el-radio :label="1">女</el-radio>
          </el-radio-group>
        </el-form-item>

      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="close">取 消</el-button>
        <el-button type="primary" @click="save">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>


export default {
  data() {
    return {
      tableData: [

      ],
      dialogTitle: '新增',
      dialogVisible: false,
      form: {
        name: '',
        age: 0,
        sex: 0
      }
    }
  },
  methods: {
    open(title) {
      this.dialogTitle = title
      this.dialogVisible = true
    },

    close() {
      this.dialogVisible = false
    },

    async save() {
      let res = await this.$axios.post(`http://127.0.0.1:7001/add`,this.form)
      let {code} = res.data
      if (code==0) {
        this.close()
        this.getStudents()
      }
      
    },

    async getStudents() {
      let {data} = await this.$axios.get(`http://127.0.0.1:7001/`)
      this.tableData = data
    }
  },
  created() {
    this.getStudents()
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
}
</style>
