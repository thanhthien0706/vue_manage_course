<template>
  <div id="screen-add-course">
    <form action="#" id="formAddCourse" @submit.prevent ref="formAddCourse">
      <div class="form-group">
        <label for="inputNameCourse">Tên: </label>
        <input
          type="text"
          v-model="inforCourse.nameCourse"
          :class="['form-control', { error: isErrorName }]"
          id="inputNameCourse"
          placeholder="Nhập thêm khóa học..."
          @input="onInputValue"
        />
      </div>
      <div class="form-group">
        <label for="inputDescript">Mô tả: </label>
        <textarea
          v-model="inforCourse.descriptCourse"
          class="form-control"
          id="tarDescript"
          placeholder="Nhập mô tả..."
        ></textarea>
      </div>
      <div class="form-group">
        <label for="inputLinkCourse">Link: </label>
        <input
          v-model="inforCourse.linkCourse"
          type="text"
          :class="['form-control', { error: isErrorLink }]"
          id="inputLinkCourse"
          placeholder="Link khóa học..."
          @input="onInputValue"
        />
      </div>

      <!-- onSubmitCourse -->
      <div class="form-group">
        <input
          type="submit"
          @click="onSubmitCourse"
          class="btn_submit"
          id="addButton"
          value="Thêm"
          :disabled="submitBtn"
        />
      </div>
    </form>

    <div :class="['box_notification', { active: isNotification }]">
      <p class="message_notification">{{ txtNotification }}</p>
      <p v-if="isViewCount">
        <strong>{{ viewCount }}</strong>
      </p>
      <ul>
        <li v-for="item in error" :key="item">
          {{ item }}
        </li>
      </ul>
      <div class="box_btn_notification">
        <button
          class="btn btn-confirm"
          v-if="btnConfirm.status"
          @click="listenEventNotificationConfirm"
          :disabled="isDisableBtnNotification"
        >
          {{ btnConfirm.text }}
        </button>
        <button
          class="btn btn-cancel"
          v-if="btnCancel.status"
          @click="listenEventNotificationCancel"
          :disabled="isDisableBtnNotification"
        >
          {{ btnCancel.text }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AddCourseScreen",
  data() {
    return {
      txtNotification: "",
      viewCount: 5,
      isViewCount: false,
      btnConfirm: {
        text: "oke",
        event: this.onClickConfirm,
        status: true,
      },
      btnCancel: {
        text: "Không🥲",
        event: this.onClickCancel,
        status: false,
      },
      submitBtn: false,
      isErrorLink: false,
      isErrorName: false,
      isNotification: false,
      isDisableBtnNotification: false,
      error: [],
      inforCourse: {
        nameCourse: "",
        descriptCourse: "",
        linkCourse: "",
      },
    };
  },
  methods: {
    onSubmitCourse() {
      this.error = [];

      if (!this.inforCourse.nameCourse.length) {
        this.error.push("Tên khóa học không được trống");
      }
      if (!this.inforCourse.linkCourse.length) {
        this.error.push("Link khóa học không được trống");
      }

      if (this.error.length) {
        this.isNotification = true;
        this.isErrorLink = true;
        this.isErrorName = true;
      } else {
        this.isNotification = true;
        this.onNhayLan1();
      }
    },

    onInputValue() {
      this.isErrorLink = false;
      this.isErrorName = false;
    },

    onClickConfirm() {
      this.isNotification = false;
    },

    onClickCancel() {
      this.txtNotification = "Oke tôi sẽ để cho bạn suy nghĩ 😊😊😊";
      this.btnCancel.status = false;
      this.btnConfirm.status = false;
      setTimeout(() => {
        this.isNotification = false;
        this.submitBtn = false;
        this.btnConfirm.status = true;
        this.btnCancel.status = true;
      }, 3000);
    },

    onNhayLan1() {
      this.txtNotification = "Bạn có chắc muốn thêm khóa học ??? 😏😏😏😏";
      this.btnConfirm.text = "Chắc😗";
      this.btnCancel.status = true;
      this.submitBtn = true;
      this.btnConfirm.event = this.onNhayLan2;
    },

    onNhayLan2() {
      this.txtNotification =
        "Tôi nghĩ bạn chưa chắc đâu. Suy nghĩ đi. Tôi sẽ mở lại sau "+this.viewCount+"s 😁😁😁😁";
      this.isViewCount = true;
      this.isDisableBtnNotification = true;
      let timeCount = setInterval(() => {
        this.viewCount--;
        if (this.viewCount == 0) {
          clearInterval(timeCount);
          this.txtNotification = "Oke bạn xác nhận lại đi nào 😀😀😀😀😀";
          this.isDisableBtnNotification = false;
          this.isViewCount = false;
          this.btnConfirm.event = this.onKhongNhay;
        }
      }, 1000);
    },
    onKhongNhay() {
      if (localStorage.getItem("listCourse") == null) {
        localStorage.setItem("listCourse", "[]");
      }

      let dataOld = JSON.parse(localStorage.getItem("listCourse"));

      let matches = dataOld.filter((e) => {
        return e.nameCourse == this.inforCourse.nameCourse.trim();
      });
      if (matches.length) {
        this.txtNotification = "Khóa học đã tồn tại 🥲🥲🥲";
      } else {
        this.txtNotification = "Khóa học của bạn đã được thêm vào 👌👍👌👍🤞";
        dataOld.push(this.inforCourse);
        localStorage.setItem("listCourse", JSON.stringify(dataOld));
      }
      this.isNotification = true;
      this.btnConfirm.status = false;
      this.btnCancel.status = false;
      setTimeout(() => {
        this.isNotification = false;
        this.submitBtn = false;
        this.btnConfirm.status = true;
        this.btnCancel.status = true;
      }, 2000);
    },
  },
  computed: {
    listenEventNotificationConfirm() {
      return this.btnConfirm.event;
    },
    listenEventNotificationCancel() {
      return this.btnCancel.event;
    },
  },
};
</script>

<style lang="css" scoped>
#formAddCourse {
  width: 30%;
  margin: 30px auto;
  border-radius: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.7);
  padding: 20px 20px;
  box-sizing: border-box;
  text-align: left;
}

#formAddCourse .form-group {
  margin: 10px 0;
  display: flex;
  flex-direction: column;
}

#formAddCourse .form-group label {
  font-weight: bold;
  font-size: 16px;
  margin-bottom: 5px;
}

#formAddCourse .form-group input.form-control {
  width: 100%;
  border: none;
  height: 30px;
  border-bottom: 2px solid rgba(65, 184, 131, 0.5);
  outline: none;
  font-size: 16px;
}

#formAddCourse .form-group input.form-control.error {
  border-bottom: 2px solid rgba(255, 0, 0, 0.5);
}

#formAddCourse .form-group textarea.form-control {
  width: 100%;
  height: 70px;
  outline: none;
  border: 2px solid rgba(65, 184, 131, 0.5);
  font-size: 16px;
}

#formAddCourse .btn_submit {
  margin-top: 10px;
  padding: 8px 0;
  border-radius: 10px;
  background: rgb(65, 184, 131);
  border: none;
  outline: none;
  font-size: 16px;
  color: #fff;
}

#formAddCourse .btn_submit:disabled {
  opacity: 0.5;
}

#screen-add-course .box_notification {
  position: fixed;
  top: -999px;
  background-color: rgba(65, 184, 131);
  min-width: 250px;
  z-index: 22;
  left: 50%;
  transform: translateX(-50%);
  transition: 0.7s;
  padding: 0 20px;
}

#screen-add-course .box_notification.active {
  top: 40px;
}

#screen-add-course .box_notification .message_notification {
  /* margin: 0; */
  font-size: 18px;
  color: #fff;
}

#screen-add-course .box_notification .box_btn_notification {
  margin: 10px 0;
}

#screen-add-course .box_notification .box_btn_notification .btn {
  margin: 0 5px;
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
}

#screen-add-course .box_notification .box_btn_notification .btn:disabled {
  opacity: 0.5;
}

#screen-add-course .box_notification .box_btn_notification .btn.btn-confirm {
  background-color: yellow;
}

#screen-add-course .box_notification .box_btn_notification .btn.btn-cancel {
  background-color: red;
  color: #fff;
}
</style>
