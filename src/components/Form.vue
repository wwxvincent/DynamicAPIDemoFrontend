<template>
  <div class="form-container">
    <h2>SQL Input</h2>
    <el-form :model="formData">
      <el-form-item label="SQL Type">
        <el-select v-model="formData.type">
          <el-option label="MySQL" value="mysql"></el-option>
          <el-option label="OceanDB" value="oceandb"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="SQL Query">
        <el-input type="textarea" v-model="formData.sql"></el-input>
      </el-form-item>
      <el-form-item label="Validation Rules">
        <el-checkbox-group v-model="formData.ruleNames">
          <el-checkbox label="SyntaxValidation"></el-checkbox>
          <el-checkbox label="TableJoinCountValidation"></el-checkbox>
          <el-checkbox label="SampleValidation"></el-checkbox>
        </el-checkbox-group>
      </el-form-item>
      <el-form-item label="Context">
        <el-input-number v-model="formData.context.maxJoinTables" :min="1" :max="5"></el-input-number>
      </el-form-item>
      <el-button @click="handleSubmit">Submit</el-button>
    </el-form>

    <!-- 显示从后端获取的数据 -->
    <div v-if="responseData" class="response-data">
      <h3>Response Data:</h3>
      <div v-for="(value, key) in responseData" :key="key" class="response-item">
        <h4>{{ key }}:</h4>
        <pre>{{ value }}</pre>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'; // 引入 axios

export default {
  data() {
    return {
      formData: {
        type: 'mysql',
        sql: '',
        ruleNames: [],
        context: {
          maxJoinTables: 1
        }
      },
      responseData: null // 用于存储从后端获取的数据
    };
  },
  methods: {
    handleSubmit() {
      // 向后端发送数据
      axios
          .post('http://localhost:8092/vincent/sql/validate', this.formData)  // 使用你实际的后端接口
          .then((response) => {
            // 处理后端响应
            this.responseData = response.data; // 将数据存储到 responseData 中
          })
          .catch((error) => {
            console.error('There was an error!', error);
          });
    }
  }
};
</script>

<style scoped>
.form-container {
  margin: 20px;
}

.response-data {
  margin-top: 20px;
  padding: 15px;
  background-color: #f4f4f4;
  border-radius: 8px;
}

.response-item {
  margin-bottom: 15px;
  padding: 10px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 8px;
}

.response-item h4 {
  margin: 0;
  font-size: 1.1rem;
  font-weight: bold;
}

.response-item pre {
  margin: 5px 0 0 0;
  white-space: pre-wrap; /* 保证内容换行显示 */
  word-wrap: break-word; /* 防止长单词溢出 */
  background-color: #f5f5f5;
  padding: 10px;
  border-radius: 5px;
  font-family: monospace;
}

.el-form-item {
  margin-bottom: 20px;
}
</style>
