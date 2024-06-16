<template>
    <div>
      <table class="table">
        <thead>
          <tr>
            <th>Name</th>
            <th>Position</th>
            <th>Office</th>
            <th>Age</th>
            <th>Start date</th>
            <th>Salary</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(row, index) in rows" :key="index">
            <td>{{ row.name }}</td>
            <td>{{ row.position }}</td>
            <td>{{ row.office }}</td>
            <td>{{ row.age }}</td>
            <td>{{ row.startDate }}</td>
            <td>{{ row.salary }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        rows: [
          { name: 'Tiger Nixon', position: 'System Architect', office: 'Edinburgh', age: 61, startDate: '2011/04/25', salary: '$320,800' },
          { name: 'Garrett Winters', position: 'Accountant', office: 'Tokyo', age: 63, startDate: '2011/07/25', salary: '$170,750' },
          // Agrega más datos aquí...
        ],
      };
    },
  };
  </script>
  
  <style>
  .table {
    width: 100%;
    margin-top: 20px;
    border-collapse: collapse;
  }
  
  .table, .table th, .table td {
    border: 1px solid #ccc;
  }
  
  .table th, .table td {
    padding: 10px;
    text-align: left;
  }
  </style>
  
  