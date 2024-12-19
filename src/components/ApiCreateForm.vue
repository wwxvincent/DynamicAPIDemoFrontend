<template>
  <div class="api-create-form">
    <!-- Create Type 选择框 -->
    <div class="form-section">
      <label for="createType">Create Type</label>
      <select id="createType" v-model="formData.createType">
        <option value="TABLE">TABLE</option>
        <option value="SQL">SQL</option>
      </select>
    </div>

    <!-- Method -->
    <div class="form-section">
      <label for="method">Method</label>
      <select id="method" v-model="formData.method">
        <option value="POST">POST</option>
        <option value="GET">GET</option>
      </select>
    </div>

    <!-- API Name -->
    <div class="form-section">
      <label for="apiName">API Name</label>
      <input
          id="apiName"
          type="text"
          v-model="formData.apiName"
          placeholder="Enter API Name"
      />
    </div>

    <!-- API Work Area -->
    <div class="form-section">
      <label for="apiWorkArea">API Work Area</label>
      <input
          id="apiWorkArea"
          type="text"
          v-model="formData.apiWorkArea"
          placeholder="Enter API Work Area"
      />
    </div>

    <!-- Path -->
    <div class="form-section">
      <label for="path">Path</label>
      <input
          id="path"
          type="text"
          v-model="formData.path"
          placeholder="Enter API Path"
      />
    </div>

    <!-- Source Type -->
    <div class="form-section">
      <label for="sourceType">Source Type</label>
      <input
          id="sourceType"
          type="text"
          v-model="formData.sourceType"
          placeholder="Enter Source Type"
      />
    </div>

    <!-- Source Schema -->
    <div class="form-section">
      <label for="sourceSchema">Source Schema</label>
      <input
          id="sourceSchema"
          type="text"
          v-model="formData.sourceSchema"
          placeholder="Enter Source Schema"
      />
    </div>

<!--    &lt;!&ndash; Select List &ndash;&gt;-->
<!--    <div class="form-section">-->
<!--      <label for="selectList">Select List</label>-->
<!--      <input-->
<!--          id="selectList"-->
<!--          type="text"-->
<!--          v-model="formData.selectList"-->
<!--          placeholder="Enter comma-separated columns"-->
<!--      />-->
<!--    </div>-->
    <!-- Select List -->
    <div v-if="formData.createType === 'TABLE'" class="form-section">
      <label for="selectList">Select List</label>
      <input
          id="selectList"
          type="text"
          v-model="formData.selectList"
          placeholder="Enter comma-separated columns"
      />
    </div>

    <!-- 如果 Create Type 是 TABLE，显示 sourceTable 和相应的 paramsList -->
    <div v-if="formData.createType === 'TABLE'">
      <!-- Source Table -->
      <div class="form-section">
        <label for="sourceTable">Source Table</label>
        <input
            id="sourceTable"
            type="text"
            v-model="formData.sourceTable"
            placeholder="Enter Source Table"
        />
      </div>

      <!-- Params List for TABLE -->
      <div class="params-section">
        <h3>Params List</h3>
        <div
            v-for="(param, index) in formData.paramsList"
            :key="index"
            class="param-item"
        >
          <div class="param-fields">
            <div class="form-section">
              <label>Sort</label>
              <input
                  type="number"
                  v-model="param.sort"
                  placeholder="Sort Order"
              />
            </div>
            <div class="form-section">
              <label>Param Name</label>
              <input
                  type="text"
                  v-model="param.param_name"
                  placeholder="Enter Param Name"
              />
            </div>
            <div class="form-section">
              <label>Param Value</label>
              <input
                  type="text"
                  v-model="param.param_value"
                  placeholder="Enter Value"
              />
            </div>
            <div class="form-section">
              <label>Operator</label>
              <input
                  type="text"
                  v-model="param.operator"
                  placeholder="Enter Operator"
              />
            </div>
            <div class="form-section">
              <label>Required</label>
              <input
                  type="text"
                  v-model="param.required"
                  placeholder="Is Required?"
              />
            </div>
            <div class="form-section">
              <label>Default Value</label>
              <input
                  type="text"
                  v-model="param.default_value"
                  placeholder="Default Value"
              />
            </div>
            <div class="form-section">
              <label>Description</label>
              <input
                  type="text"
                  v-model="param.description"
                  placeholder="Enter Description"
              />
            </div>
            <div class="form-section">
              <label>Sample</label>
              <input
                  type="text"
                  v-model="param.sample"
                  placeholder="Sample Value"
              />
            </div>
          </div>
          <button @click="removeParam(index)" class="remove-button">
            Remove
          </button>
        </div>
        <button @click="addParam" class="add-param">Add Param</button>
      </div>
    </div>

    <!-- 如果 Create Type 是 SQL，显示 returnType 和 sqlSentence -->
    <div v-if="formData.createType === 'SQL'">
      <!-- Return Type -->
      <div class="form-section">
        <label for="returnType">Return Type</label>
        <select id="returnType" v-model="formData.returnType">
          <option value="json">JSON</option>
          <option value="xml">XML</option>
        </select>
      </div>

      <!-- SQL Sentence -->
      <div class="form-section">
        <label for="sqlSentence">SQL Sentence</label>
        <textarea
            id="sqlSentence"
            v-model="formData.sqlSentence"
            placeholder="Enter SQL Sentence"
            rows="6"
            cols="60"
        ></textarea>
      </div>

      <!-- Params List for SQL -->
      <div class="params-section">
        <h3>Params List</h3>
        <div
            v-for="(param, index) in formData.paramsList"
            :key="index"
            class="param-item"
        >
          <div class="param-fields">
            <div class="form-section">
              <label>Sort</label>
              <input
                  type="number"
                  v-model="param.sort"
                  placeholder="Sort Order"
              />
            </div>
            <div class="form-section">
              <label>Param Name</label>
              <input
                  type="text"
                  v-model="param.param_name"
                  placeholder="Enter Param Name"
              />
            </div>
            <div class="form-section">
              <label>Param Value</label>
              <input
                  type="text"
                  v-model="param.param_value"
                  placeholder="Enter Value"
              />
            </div>
            <div class="form-section">
              <label>Default Value</label>
              <input
                  type="text"
                  v-model="param.default_value"
                  placeholder="Default Value"
              />
            </div>
            <div class="form-section">
              <label>Description</label>
              <input
                  type="text"
                  v-model="param.description"
                  placeholder="Enter Description"
              />
            </div>
          </div>
          <button @click="removeParam(index)" class="remove-button">
            Remove
          </button>
        </div>
        <button @click="addParam" class="add-param">Add Param</button>
      </div>
    </div>

    <!-- 提交按钮 -->
    <button class="submit-button" @click="submitForm">Submit</button>

    <!-- 后端返回数据展示 -->
    <div class="response-section" v-if="responseData">
      <h3>Response Data</h3>
      <pre>{{ responseData }}</pre>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "ApiCreateForm",
  data() {
    return {
      formData: {
        createType: "TABLE",
        apiName: "",
        apiWorkArea: "",
        path: "/test",
        method: "POST",
        sourceType: "mysql",
        sourceSchema: "gptDB",
        sourceTable: "user",
        selectList: ["name", "id", "depId"],
        paramsList: [
          {
            sort: 1,
            param_name: "depId",
            param_value: "6",
            operator: "=",
            required: "0",
            default_value: "1",
            description: "部门ID",
            sample: "1",
          },
        ],
      },
      apiFields: [
        {id: "apiWorkArea", label: "API Work Area", model: "apiWorkArea", placeholder: "Enter API Work Area"},
        {id: "path", label: "Path", model: "path", placeholder: "Enter API Path"},
        {id: "sourceType", label: "Source Type", model: "sourceType", placeholder: "Enter Source Type"},
        {id: "sourceSchema", label: "Source Schema", model: "sourceSchema", placeholder: "Enter Source Schema"},
        {id: "sourceTable", label: "Source Table", model: "sourceTable", placeholder: "Enter Source Table"},
      ],
      responseData: null, // 用于存储后端返回的数据
    };
  },
  methods: {
    addParam() {
      this.formData.paramsList.push({
        sort: this.formData.paramsList.length + 1,
        param_name: "",
        param_value: "",
        operator: "=",
        required: "0",
        default_value: "",
        description: "",
        sample: "",
      });
    },
    removeParam(index) {
      this.formData.paramsList.splice(index, 1);
    },
    async submitForm() {
      try {
        const response = await axios.post("http://localhost:8092/vincent/api/create", this.formData);
        this.responseData = JSON.stringify(response.data, null, 2); // 格式化返回的数据
      } catch (error) {
        console.error("Error submitting form:", error);
        this.responseData = `Error: ${error.message}`;
      }
    },
  },
};
</script>

<style scoped>
/* 保持与 SQL Input 风格一致 */
.api-create-form {
  margin: 20px;
}

.form-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.form-section label {
  width: 30%;
  text-align: left;
}

.form-section input,
.form-section select {
  width: 60%;
  padding: 5px;
}

.params-section {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.param-item {
  margin-bottom: 15px;
  padding: 15px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 8px;
}

.param-fields .form-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 5px;
}

.add-param {
  margin-top: 10px;
  padding: 5px 10px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.add-param:hover {
  background-color: #0056b3;
}

.remove-button {
  margin-top: 10px;
  padding: 5px 10px;
  background-color: #dc3545;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.remove-button:hover {
  background-color: #c82333;
}

.submit-button {
  padding: 10px 20px;
  background-color: #28a745;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.submit-button:hover {
  background-color: #218838;
}

.response-section {
  margin-top: 20px;
  background-color: #f4f4f4;
  padding: 15px;
  border-radius: 8px;
}

.response-section h3 {
  margin-bottom: 10px;
}

.response-section pre {
  margin: 5px 0;
  white-space: pre-wrap;
  word-wrap: break-word;
  background-color: #f5f5f5;
  padding: 10px;
  border-radius: 5px;
  font-family: monospace;
  border: 1px solid #ddd;
}
</style>

