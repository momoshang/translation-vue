<template>
  <div>


    <el-row :gutter="10">
      <el-col :span="4" :offset="4">
        <el-select v-model="fanyi.value1" placeholder="请选择">
          <el-option
            v-for="item in options1"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </el-col>
      <el-col :span="4" :offset="4">
        <el-select v-model="fanyi.value" placeholder="请选择">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </el-col>
      <el-col :span="1" >
        <i class="el-icon-right"></i>
      </el-col>
      <el-col :span="4" >
        <el-select v-model="fanyi.value2" placeholder="请选择">
          <el-option
            v-for="item in options2"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </el-col>

    </el-row>
    <el-divider></el-divider>
    <el-row>
      <el-col :span="16" :offset="4">
        <el-input
          v-on:change="fan"
          type="textarea"
          :rows="5"
          placeholder="请输入内容"
          v-model="fanyi.textarea"
          >
        </el-input>
      </el-col>

    </el-row>

    <el-row>
      <el-col :span="16" :offset="4">

          <el-input
            type="textarea"
            :rows="5"
            placeholder="翻译结果"
            v-model="results">
          </el-input>

      </el-col>

    </el-row>

    <el-row>
      <el-col :span="4" :offset="16">
        <el-input v-model="input" placeholder="请评分"></el-input>
      </el-col>

    </el-row>

    <el-row>
      <el-col :span="2" :offset="16">
        <el-button @click="exporti" type="primary" round>导出</el-button>
      </el-col>
      <el-col :span="2" :offset="0">
        <el-button @click="submit" type="success" round>提交</el-button>
      </el-col>

    </el-row>



  </div>
</template>



<script>
const axios = require('axios');
export default {

  // name: "translation",
  data() {
    return {
      options1: [{
        value: '百度翻译',
        label: '百度翻译'
      }, {
        value: '谷歌翻译',
        label: '谷歌翻译'
      }],

      options: [{
        value: '中文',
        label: '中文'
      }, {
        value: '英文',
        label: '英文'
      }],
      options2: [{
        value: '中文',
        label: '中文'
      }, {
        value: '英文',
        label: '英文'
      }],

      results:'',
      input: '',

      fanyi: {
        value1: '',
        value: '',
        value2: '',
        textarea: '',

      },
    }
  },
  methods: {
    exporti(){
      console.log("33")
      // window.location.href('')
      // this.$router.replace('http://127.0.0.1:8003/tran/exportT')
      // this.$router.push('http://127.0.0.1:8003/tran/exportT')
      window.open('http://127.0.0.1:8003/tran/exportT')

    },
    submit() {
      console.log("222")

      axios({
        url: 'http://127.0.0.1:8003/tran/submit',
        method: 'POST', //提交姿势
        data: {
          engine: this.fanyi.value1,
          original: this.fanyi.value,
          purpose: this.fanyi.value2,
          input: this.fanyi.textarea,
          output: this.results,
          score: this.input
        },
        headers: {
          "Content-Type": "application/json",
          "Access-Control-Allow-Origin": "*",
          "Access-Control-Allow-Credentials": "true"
        }

      }).then(     //成功时返回
        res => {
          console.log(res)
          if (res.data===1){
            this.$message({
              message: '提交成功',
              type: 'success'
            });
          }else {
            this.$message.error('提交失败');
          }
          open2()
        }
      )

    },

    fan: function () {
      console.log("111")
      axios({
        url: 'http://127.0.0.1:8003/tran/translation',
        method: 'POST', //提交姿势
        data: {
          value1: this.fanyi.value1,
          value: this.fanyi.value,
          value2: this.fanyi.value2,
          textarea: this.fanyi.textarea
        },
        headers: {
          "Content-Type": "application/json",
          "Access-Control-Allow-Origin": "*",
          "Access-Control-Allow-Credentials": "true"
        }

      }).then(     //成功时返回
        res => {
          console.log(res)
          this.results = res.data.data
        }
      )

    }

  }


}
</script>

<style>
.el-row {
  margin-bottom: 20px;
&:last-child {
   margin-bottom: 0;
 }
}
.el-col {
  border-radius: 4px;
}
.bg-purple-dark {
  background: #99a9bf;
}
.bg-purple {
  background: #d3dce6;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}
</style>



