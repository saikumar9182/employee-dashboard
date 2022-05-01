<template>
  <div>
    <div style="height:10vh;width:90%;margin:auto;" class="header d-flex justify-content-sm-between align-items-center mb-3">
      <h1>Employee Dashboard</h1>
      <div class="d-flex">
      <div class="filter-wrapper">
        <span><i class="ri-filter-line"></i></span>
        <!-- Button trigger modal -->
        <button
          type="button"
          class="btn btn-primary"
          style="border: none"
          data-bs-toggle="modal"
          data-bs-target="#exampleModal"
        >
          Filters
        </button>

        <!-- Modal -->
        <div
          class="modal fade"
          id="exampleModal"
          tabindex="-1"
          aria-labelledby="exampleModalLabel"
          aria-hidden="true"
        >
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">
                  Select Filter
                </h5>
                <button
                  type="button"
                  class="btn-close"
                  data-bs-dismiss="modal"
                  aria-label="Close"
                ></button>
              </div>
              <div class="modal-body d-flex flex-column">
                <div class="d-flex is-align-items-center">
                  <p>Country:</p>
                  <select v-model="countryFilter" style="margin-bottom: 5px">
                    <option value="" disabled selected>Country</option>
                    <option
                      :value="country"
                      v-for="(country, index) in countries"
                      :key="index"
                    >
                      {{ country }}
                    </option>
                  </select>
                </div>
                <div class="d-flex is-align-items-center">
                  <p>Name:</p>
                  <input type="text" placeholder="Enter Name here..." v-model="nameFilter"/>
                </div>
              </div>
              <div class="modal-footer">
                <button
                  type="button"
                  class="btn btn-secondary"
                  data-bs-dismiss="modal"
                  @click.prevent="FetchEmployees"
                >
                  Save and Close
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="sortBy-wrapper">
        <span> <i class="ri-sort-desc"></i></span>
        <select style="background:#1A72Fd;color:white" v-model="sortBy">
          <option value="" disabled selected>Sort By</option>
          <option value="birthday">DOB</option>
          <option value="country">Country</option>
          <option value="name">Name</option>
        </select>
      </div>
      </div>
    </div>
    <div class="content">
      <table class="table">
        <thead>
          <tr>
            <th scope="col">ID:</th>
            <th scope="col">Name:</th>
            <th scope="col">Email:</th>
            <th scope="col">Phone:</th>
            <th scope="col">Country:</th>
            <th scope="col">DOB:</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="employee in EmployeesData" :key="employee.id">
            <th scope="row">{{ employee.id }}</th>
            <td><img :src="employee.avatar" />{{ employee.name }}</td>
            <td>{{ employee.email }}</td>
            <td>{{ employee.phone }}</td>
            <td>{{ employee.country }}</td>
            <td>{{ DateFormatter(employee.birthday) }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="pagination-comp">
      <pagination
        @change-page="ChangePage"
        @page-jump="ChangePage"
        :totalEmployees="totalEmployees"
      ></pagination>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pagination from "./Pagination.vue";

// import moment from "moment";
import _ from "lodash";
export default {
  components: { Pagination },
  name: "EmployeContent",
  data() {
    return {
      Employees: [],
      page: 1,
      limit: 10,
      sortBy: "",
      totalEmployees: 0,
      countries:[],
      names:[],
      countryFilter: "",
      nameFilter: "",
    };
  },
  watch: {
    sortBy() {
      this.FetchEmployees();
    },
    countryFilter() {
      console.log(this.countryFilter);
    },
  },
  computed: {
    EmployeesData() {
      const vm = this;
      return vm.Employees;
    },
  },
  methods: {
    DateFormatter(birthday) {
      let birthdate = "";
      for (let i = 0; i < birthday.length; i++) {
        if (birthday[i] === "T") break;
        birthdate += birthday[i];
      }
      return birthdate;
    },
    ChangePage(payload) {
      const vm = this;
      vm.page = payload.pageNo;
      vm.FetchEmployees();
    },
    FetchEmployees() {
      console.log("are we here", this.sortBy);
      const vm = this;
      const url = "https://620dfdda20ac3a4eedcf5a52.mockapi.io/api/employee";
      const params = { page: vm.page, limit: vm.limit};
      if(vm.sortBy) {
        params.sortBy = vm.sortBy;
      }
      if(vm.countryFilter) {
        params.country = vm.countryFilter;
      }
      if(vm.nameFilter) {
        params.name = vm.nameFilter;
      }
      // console.log(params);
      axios.get(url, { params }).then((response) => {
        console.log(response.data);
        vm.Employees = response.data;
        // vm.totalEmployees = response.data.length;
      });
    },
  },
  mounted() {
    const vm = this;
    const url = "https://620dfdda20ac3a4eedcf5a52.mockapi.io/api/employee";
    axios.get(url).then((response) => {
      vm.totalEmployees = response.data.length;
      console.log(response.data);
      _.each(response.data, (employee) =>{
        vm.countries.push(employee.country);
        vm.names.push(employee.name);
      })
    });
    vm.FetchEmployees();
  },
};
</script>

<style scoped>
*,
*::before,
*::after {
  box-sizing: border-box;
}
input[type="text"] {
  height:25px;
  outline:none;
  margin-left:30px;
}
select {
  /* appearance: none; */
  height: 100%;
  color:black;
  background-color: white;
  border-radius: 3px;
  text-align: center;
  border: none;
  padding: 0 1em 0 0;
  margin: 0;
  outline: none;
  font-family: inherit;
  font-size: inherit;
  cursor: pointer;
  line-height: inherit;
}
option {
  background: white;
  color:black;
}
img {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin-right: 30px;
}
h1 {
  color:#1A72FD;
}
.header i {
  margin: 15px;
}
table,
header {
  width: 90%;
  margin: auto;
}
</style>