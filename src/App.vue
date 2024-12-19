<template>
  <div id="app">
    <div class="tabs">
      <button :class="{'active-tab': activeTab === 'sql'}" @click="activeTab = 'sql'">SQL Input</button>
      <button :class="{'active-tab': activeTab === 'api'}" @click="activeTab = 'api'">Dynamic API Creation</button>
    </div>
    <div v-if="activeTab === 'sql'">
      <Form />
    </div>
    <div v-else>
      <ApiCreateForm />
    </div>
  </div>
</template>

<script>
import Form from './components/Form.vue';
import ApiCreateForm from './components/ApiCreateForm.vue';

export default {
  name: 'App',
  components: {
    Form,
    ApiCreateForm,
  },
  data() {
    return {
      activeTab: 'sql', // Default to SQL Input tab
    };
  },
};
</script>

<style>
/* Tab Styles */
.tabs {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

button {
  padding: 10px 20px;
  margin: 0 5px;
  border: none;
  background-color: #ddd;
  font-size: 1em;
  cursor: pointer;
  border-radius: 4px;
}

button:hover {
  background-color: #bbb;
}

.active-tab {
  background-color: #007bff;
  color: #fff;
}
</style>
