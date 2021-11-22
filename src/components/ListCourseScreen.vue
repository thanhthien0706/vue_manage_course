<template>
  <div id="screen-list-course">
    <div class="box_btn_all_delete">
      <button
        class="btn_delete_all"
        v-if="isCheckDeleteAll"
        @click="deleteAllCourse"
      >
        Xóa tất cả
      </button>
    </div>
    <p v-if="isCheck">Bạn chưa có Khóa học nào</p>
    <ul class="course-list" v-if="isDelete">
      <li
        class="course-list-item"
        v-for="(item, index) in computedListCourse"
        :key="item"
      >
        <div class="box_content_main_list">
          <div class="box_header">
            <h4>
              <strong>{{ item.nameCourse }}</strong>
            </h4>
            <p @click="deleteCourse(index)" style="cursor: pointer">Xóa</p>
          </div>
          <p style="margin: 10px 0">
            <strong>Mô tả: </strong> {{ item.descriptCourse }}
          </p>
          <a :href="item.linkCourse">Xem ngay</a>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "ListCourseScreen",
  data() {
    return {
      // dataListCourse: this.showListCourses(),
      dataListCourse: [],
      isCheck: false,
      isCheckDeleteAll: true,
      isDelete: false,
    };
  },
  beforeMount() {
    this.showListCourses();
  },
  methods: {
    showListCourses() {
      if (localStorage.getItem("listCourse") != null) {
        this.isDelete = true;
        let data = JSON.parse(localStorage.getItem("listCourse"));
        if (data == "") {
          this.isCheck = true;
          this.isCheckDeleteAll = false;
        }
        this.dataListCourse = data;
      } else {
        this.isCheckDeleteAll = false;
        this.isCheck = true;
      }
    },

    deleteCourse(index) {
      let data = JSON.parse(localStorage.getItem("listCourse"));

      const arr1 = data.slice(0, index);
      const arr2 = data.slice(index + 1, data.length);
      const newArr = [...arr1, ...arr2];

      localStorage.setItem("listCourse", JSON.stringify(newArr));
      this.showListCourses();
    },
    deleteAllCourse() {
      localStorage.removeItem("listCourse");
      this.isDelete = false;
      this.showListCourses();
    },
  },
  computed: {
    computedListCourse() {
      return this.dataListCourse;
    },
  },
};
</script>

<style lang="css" scoped>
#screen-list-course {
  width: 35%;
  margin: 30px auto 0;
}

#screen-list-course .box_btn_all_delete {
  text-align: right;
}

#screen-list-course .box_btn_all_delete .btn_delete_all {
  padding: 5px 10px;
  font-size: 14px;
  background-color: red;
  color: #fff;
  border: none;
  border-radius: 10px;
}

#screen-list-course .course-list {
  margin: 0;
  padding: 0;
  list-style: none;
}

#screen-list-course .course-list-item {
  margin: 20px 0;
  display: flex;
}

#screen-list-course .course-list-item .box_content_main_list {
  width: 100%;
  padding: 30px 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0);
  border-radius: 20px;
  text-align: left;
}

#screen-list-course .course-list-item .box_content_main_list .box_header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: row;
}
#screen-list-course .course-list-item .box_content_main_list .box_header > h4,
#screen-list-course .course-list-item .box_content_main_list .box_header > p {
  margin: 0;
}
</style>
