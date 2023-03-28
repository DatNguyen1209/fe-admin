<script setup>
import { ref, watch } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const router = useRouter();

const userName = ref("");
const password = ref("");
const login = async () => {
  try {
    const res = await axios.post(`http://localhost:8081/api/v1/auth/signin`, {
      userName: userName.value,
      password: password.value,
    });
    localStorage.storedData = this.input;
    router.push("/");
  } catch (error) {
    console.log(error);
  }
};
watch(login, () => {
  if (isLocalStorage) {
    localStorage.setItem("storedData", this.input);
  }
});
// watch: {
//   input: function () {
//     if (isLocalStorage() /* function to detect if localstorage is supported*/) {
//       localStorage.setItem('storedData', this.input)
//     }
//   }
// }
</script>
<template>
  <div class="bg-img">
    <div class="d-flex align-center justify-center" style="height: 100vh">
      <v-sheet min-width="300" class="mx-auto">
        <h1 class="title">Đăng nhập hệ thống</h1>
        <v-form fast-fail style="width: 100%" @submit.prevent="login">
          <v-text-field
            variant="outlined"
            v-model="userName"
            label="User Name"
          ></v-text-field>

          <v-text-field
            variant="outlined"
            v-model="password"
            label="Password"
            type="Password"
          ></v-text-field>
          <v-btn type="submit" color="primary" block class="mt-2"
            >Sign in</v-btn
          >
        </v-form>
      </v-sheet>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.bg-img {
  background-size: 100%;
  // background: url(../assets/images/background/bg-1.jpg);
  // background-image: url(https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ0lUI6mEOgtY0wmBrmkcNmpEusyCs0fxb6Jg&usqp=CAU);
  background-repeat: no-repeat;
  background-position: right top;
  background-image: linear-gradient(
    to right,
    rgba(88, 37, 123, 0),
    rgba(88, 37, 123, 1)
  );
}
.title {
  text-transform: uppercase;
  color: #ee8a6a;
  text-align: center;
  margin-bottom: 20px;
}
</style>
