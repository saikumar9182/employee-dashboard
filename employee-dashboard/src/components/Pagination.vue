<template>
  <div class="pagination">
    <nav aria-label="Page navigation example" style="margin: auto">
      <ul class="pagination">
        <li class="page-item">
          <a class="page-link" :class="pageNo==1?'disable-click':''" aria-label="Previous">
            <span @click.prevent="moveDirection('Previous')" aria-hidden="true">&laquo; Previous</span>
          </a>
        </li>
        <li class="page-item" style="display:flex;">
          <a class="page-link"
            ><input type="number" v-model="pageNo" placeholder="Enter Page No"
          /></a>
          <button @click="changePage" class="d-flex align-items-center">Move to this page<i class="ri-arrow-right-s-line"></i></button>
        </li>
        <li class="page-item">
          <a class="page-link" :class="skip==totalEmployees-10 ?'disable-click':''"  aria-label="Next">
            <span @click.prevent="moveDirection('Next')" aria-hidden="true" ref="next" >&raquo; Next</span>
          </a>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
// import _ from 'lodash';
// import { EventBus } from '@/event-bus';


export default {
  name: "Pagination",
  data() {
    return {
      skip: 0,
      pageNo: 1,
    };
  },
  props: ['totalEmployees'],
  watch: {
    totalEmployees() {
      this.skip = 0;
      this.pageNo = 1;
    }
  },
  methods: {
      moveDirection(dir) {
        console.log('in move dir', dir);
          if(dir === 'Previous') {
            // console.log('hahhaha', this.skip);
              if (this.skip > 0) {
                  this.skip-=10;
                  this.pageNo--;
              }
          }
          if(dir === 'Next') {
                        // console.log('pappapa', this.skip);
              if(this.skip < this.totalEmployees - 10) {
                  this.skip +=10;
                  this.pageNo++;
              }
          }
          this.$emit('change-page', {pageNo:this.pageNo})
      },
      changePage() {
          const vm = this;
          const totalPages = vm.totalEmployees/10;
          vm.skip = (vm.pageNo-1)*10;
          if (vm.pageNo > 0 && vm.pageNo <=totalPages) {
              vm.$emit('page-jump',{pageNo:vm.pageNo})
          }
      }
  },
  mounted() {
  },
};
</script>

<style scoped>
.disable-click {
  pointer-events: none;
}
input[type="number"] {
  height: 100%;
  border: none;
  outline: none;
}
ul {
  margin-top:1rem;
}
a {
    cursor:pointer;
}
button {
    background-color: #1A72FD;
    color:white;
    border:none;
    outline:none;
}
</style>