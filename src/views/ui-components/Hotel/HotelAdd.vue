<script setup>
import { ref, defineProps } from "vue";
import axios from "axios";

const props = defineProps({
  isOpen: Boolean,
  data: Object,
});

const emit = defineEmits(["add", "close"]);

const hotelName = ref("");
const hotelType = ref("");
const phone = ref("");
const address = ref("");
const fromPrice = ref("");
const rated = ref();
const description = ref("");
const images = ref("");
const handleAdd = async () => {
  try {
    const dataAdd = {
      hotelName: hotelName.value,
      hotelType: hotelType.value,
      phone: phone.value,
      fromPrice: fromPrice.value,
      rated: rated.value,
      address: address.value,
      description: description.value,
    };
    const res = await axios.post(
      "http://localhost:8081/api/v1/hotel/createhotel",
      dataAdd
    );
    emit("add");
  } catch (e) {
    console.log(e);
  }
};
const handleClose = () => {
  emit("close");
};
</script>
<template>
  <v-row justify="center">
    <v-dialog v-model="isOpen" width="auto" persistent>
      <v-card>
        <v-card-title>
          <span class="text-h5 text-uppercase">Thêm mới</span>
        </v-card-title>
        <v-card-text
          style="max-height: 400px min-width: 800px; overflow-x: auto"
        >
          <v-container>
            <v-row>
              <v-col cols="6">
                <v-text-field
                  v-model="hotelName"
                  required
                  label="Tên khách sạn"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  label="Loại khách sạn"
                  v-model="hotelType"
                  required="không được để trống!!"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  label="Giá từ"
                  v-model="fromPrice"
                  required="không được để trống!!"
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  v-model="phone"
                  label="Điện thoại"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  v-model="rated"
                  label="Đánh giá"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-file-input
                  v-model="images"
                  multiple
                  label="File input"
                ></v-file-input>
              </v-col>

              <v-col cols="12">
                <v-text-field
                  v-model="description"
                  label="Mô tả"
                  hint="example of persistent helper text"
                  persistent-hint
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue-darken-1" variant="text" @click="handleClose">
            Đóng
          </v-btn>
          <v-btn color="blue-darken-1" variant="text" @click="handleAdd">
            Cập nhật
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
<style>
.text-uppercase {
  text-transform: uppercase !important;
}
.v-dialog .v-overlay__content {
  max-height: 600px;
  overflow-y: scroll !important;
  max-width: 700px;
}
.v-overlay-scroll-blocked {
  padding-inline-end: 0;
}
</style>
