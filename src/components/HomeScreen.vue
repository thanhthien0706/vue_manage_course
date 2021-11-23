<template>
  <div id="screen-home">
    <h1>Quản lí khóa học</h1>

    <div class="box_main_content">
      <button
        v-for="buttonItem in buttonItems"
        :key="buttonItem.id"
        :class="[
          'btnChangePage',
          { active: pageCurrent == buttonItem.component },
        ]"
        @click="changPage(buttonItem.component)"
      >
        {{ buttonItem.title }}
      </button>

      <component :is="changPageCurrent"></component>
    </div>
  </div>
</template>

<script>
import AddCourseScreen from "./AddCourseScreen.vue";
import ListCourseScreen from "./ListCourseScreen.vue";

export default {
  name: "HomeScreen",
  components: {
    AddCourseScreen,
    ListCourseScreen,
  },
  data() {
    return {
      pageCurrent: ListCourseScreen,
      buttonItems: [
        {
          id: 1,
          component: ListCourseScreen,
          title: "Danh sách khóa học",
        },
        {
          id: 2,
          component: AddCourseScreen,
          title: "Thêm khóa học",
        },
      ],
    };
  },
  methods: {
    changPage(componentName) {
      return (this.pageCurrent = componentName);
    },
  },
  computed: {
    changPageCurrent() {
      return this.pageCurrent;
    },
  },
};
</script>

<style lang="css" scoped>
.btnChangePage {
  padding: 8px 12px;
  font-size: 16px;
  border: none;
  background: none;
  position: relative;
  margin: 0 3px;
  font-weight: bold;
  cursor: pointer;
  transition: 0.5s;
  border-radius: 5px 5px 0 0;
}

.btnChangePage::before {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 3px;
  background-color: #41b883;
  transition: 0.5s;
}

.btnChangePage:hover {
  background-color: rgba(65, 184, 131, 0.2);
}

.btnChangePage:hover::before {
  width: 100%;
}

.btnChangePage.active {
  background-color: rgba(65, 184, 131, 0.2);
}

.btnChangePage.active::before {
  width: 100%;
}
</style>
