<template>
  <div id="screen-home">
    <h1>Quản lí khóa học</h1>

    <div class="box_main_content">
      <button
        v-for="buttonItem in buttonItems"
        :key="buttonItem.id"
        :class="{ active: pageCurrent == buttonItem.component }"
        @click="changPage(buttonItem.component)"
      >
        {{ buttonItem.title }}
      </button>

      <keep-alive>
        <component :is="changPageCurrent"></component>
      </keep-alive>
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
          title: "List Course",
        },
        {
          id: 2,
          component: AddCourseScreen,
          title: "Add Course",
        },
      ],
    };
  },
  methods: {
    changPage(componentName) {
      return (this.pageCurrent = componentName);
    },
  },
  computed:{
      changPageCurrent(){
          return (this.pageCurrent);
      }
  }
};
</script>

<style lang="css" scoped>
button.active {
  background-color: red;
}
</style>
