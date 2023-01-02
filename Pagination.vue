<template>
  <!-- 分页列表 -->
  <table class="table table-hover">
    <thead>
      <tr>
        <th class="col-1">编号</th>
        <th class="col-4">标题</th>
        <th class="col-4">来源</th>
        <th class="col-3">通过率</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="problem in pageinfo.list" :key="problem.id">
        <td>{{ problem.problemId }}</td>
        <td>{{ problem.problemTitle }}</td>
        <td>
          <span style="background: blue; color: white">{{
            problem.problemFrom
          }}</span>
        </td>
        <td>{{ problem.problemDone }}/{{ problem.problemSubmit }}</td>
      </tr>
    </tbody>
  </table>
  <!-- 控制分页的选择栏 -->
  <ul class="pagination justify-content-center">
    <li>
      <a class="my-page-link" href="#" @click="setPage(1)">&laquo;</a>
    </li>
    <li>
      <a class="my-page-link" href="#" @click="setPage(pageinfo.prePage)"
        >&lt;</a
      >
    </li>
    <li
      v-for="page in pageinfo.navigatepageNums"
      :key="page"
      :class="{ chosen: data.pageNum === page }" 
      @click="setPage(page)"
    >
      <a class="my-page-link" href="#">{{ page }}</a>
    </li>
    <li>
      <a class="my-page-link" href="#" @click="setPage(pageinfo.nextPage)"
        >&gt;</a
      >
    </li>
    <li>
      <a class="my-page-link" href="#" @click="setPage(pageinfo.pages)"
        >&raquo;</a
      >
    </li>
  </ul>
</template>

<script>
import { useStore } from "vuex";
import $ from "jquery";
import { ref } from "vue";
export default {
  setup() {
    var pageinfo = ref([]);
    var store = useStore();
    //请求分页参数
    var data = {
      pageNum: "1",
      pageSize: "12",
    };

    const get_list = () => {
      $.ajax({
        type: "post",
        url: `${store.state.config.url}/problems/list`,
        data: JSON.stringify(data),
        dataType: "json",
        contentType: "application/json;charset=utf-8",
        success(resp) {
          pageinfo.value = resp;
          console.log(resp);
        },
        error() {
          alert("error");
        },
      });
    };
    get_list();
    //根据点击的pageNum切换page
    const setPage = (pageNum) => {
      if (pageNum !== 0) {
        data.pageNum = pageNum;
        get_list();
      }
    };

    return {
      pageinfo,
      data,
      setPage,
      get_list,
    };
  },
};
</script>

<style scoped>
.my-page-link {
  display: inline-block;
  text-decoration: none;
  text-align: center;
  border: 1px solid #dddddd;
  width: 40px;
}
.chosen {
  background: lightblue !important;
}
</style>