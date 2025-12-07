<template>
  <div>
    <h1>学生列表</h1>
    <table>
      <thead>
        <tr>
          <th>字段</th>
          <th>值</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="student in students" :key="student.email">
          <td>姓名</td>
          <td>{{ student.name }}</td>
        </tr>
        <tr v-for="student in students" :key="student.email + '-age'">
          <td>年龄</td>
          <td>{{ student.age }}</td>
        </tr>
        <tr v-for="student in students" :key="student.email + '-email'">
          <td>邮箱</td>
          <td>{{ student.email }}</td>
        </tr>
        <tr v-for="student in students" :key="student.email + '-address'">
          <td>地址</td>
          <td>{{ student.address }}</td>
        </tr>
        <tr v-for="student in students" :key="student.email + '-classid'">
          <td>班级ID</td>
          <td>{{ student.classid }}</td>
        </tr>
        <tr v-for="student in students" :key="student.email + '-time'">
          <td>时间</td>
          <td>{{ student.time }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'StudentList',
  props: {
    students: {
      type: Array,
      required: true
    }
  }
};
</script>

<style>
table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}
</style>