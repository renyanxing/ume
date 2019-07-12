<template>
  <div class="app-container">
    <el-form ref="form" :model="form" label-width="120px">

      <el-container>
        <el-main>
          <el-container>
            <el-main>
              <el-form-item label="茶·名称">
                <el-input v-model="form.teaname" />
              </el-form-item>
              <el-form-item label="茶·种类">
                <el-select v-model="form.teakind" placeholder="请选择茶的种类">
                  <el-option v-for="item in form.kind" :key="item.value" :label="item.label" :value="item.value">
                  </el-option>
                </el-select>
                <el-button type="text">添加种类</el-button>
              </el-form-item>
              <el-form-item label="茶·入手时间">
                <el-col :span="11">
                  <el-date-picker v-model="form.date" type="date" placeholder="Pick a date" style="width: 100%;" />
                </el-col>
              </el-form-item>

              <el-form-item label="茶·品牌">
                <el-select v-model="form.teabrand" placeholder="请选择茶的品牌">
                  <el-option v-for="item in form.brand" :key="item.value" :label="item.label" :value="item.value">
                  </el-option>
                </el-select>
                <el-button type="text">添加品牌</el-button>
              </el-form-item>

              <el-form-item label="茶·存量">
                <el-input class="tea_amount_el_input" v-model="form.amount" /><span style="padding-left:15px;">克</span>

              </el-form-item>

              <el-form-item label="茶·年份">
                <el-col :span="11">
                  <el-date-picker v-model="form.birthday" type="date" placeholder="Pick a date" style="width: 100%;" />
                </el-col>
              </el-form-item>
            </el-main>
            <el-main>
                <el-upload
                action="http://localhost:51397/api/uploadimg.ashx"
                list-type="picture-card"
                :on-preview="handlePictureCardPreview"
                :on-remove="handleRemove">
                <i class="el-icon-plus"></i>
              </el-upload>
              <el-dialog :visible.sync="dialogVisible">
                <img width="100%" :src="dialogImageUrl" alt="">
              </el-dialog>
            </el-main>
          </el-container>
        </el-main>
        <el-footer>
          <el-form-item>
            <el-button class="button" type="primary" @click="onSubmit">收入囊中</el-button>
            <el-button class="button" @click="onCancel">关闭页面</el-button>
          </el-form-item>
        </el-footer>
      </el-container>



    </el-form>

  </div>
</template>

<script>
  export default {
    data() {
      return {
        form: {
          teaname: '',
          teakind: '',
          kind: [],
          date: '',
          teabrand: '',
          brand: [],
          amount: '',
        },
        dialogImageUrl:'',
        dialogVisible:false
      }
    },
    methods: {
      onSubmit() {
        this.$message('submit!')
      },
      onCancel() {
        this.$message({
          message: 'cancel!',
          type: 'warning'
        })
      },
      handleRemove(file, fileList) {
        debugger;
        axios.post('http://localhost:51397/api/removeimg.ashx',{url:file.url,filename:file.filename})
        .then(function(response){
          this.$message('success!')
      })
      .catch(function (error) { // 请求失败处理
        console.log(error);
      });
        
      },
      handlePictureCardPreview(file) {
        this.dialogImageUrl = file.url;
        this.dialogVisible = true;
      }
    }
  }
</script>

<style scoped>
  .line {
    text-align: center;
  }

  .tea_amount_el_input {
    width: 200px;
  }

  .button {
    font-size: large;
  }
</style>