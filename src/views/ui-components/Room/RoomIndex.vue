<script setup>
import { onMounted, ref, watch } from "vue";
import axios from "axios";
import moment from "moment";
import RoomAdd from "./RoomAdd.vue";
const desserts = ref([]);
const page = ref(1);
const isOpenAdd = ref(false);
const totalPage = ref(0);
const totalRecords = ref(0);

onMounted(() => {
  getData();
});
watch(page, () => {
  getData();
});
const getData = async () => {
  try {
    const res = await axios.get(
      `http://localhost:8081/api/v1/hotel/getAll?page=${page.value}&size=5`
    );
    desserts.value = res.data.data;
    totalPage.value = res.data.totalPages;
    totalRecords.value = res.data.totalRecords;
  } catch (e) {
    console.error(e);
  }
};
const openDialogAdd = (data) => {
  console.log(data);
  isOpenAdd.value = true;
};
const handleAdd = () => {
  console.log("run");
  isOpenAdd.value = false;
  getData();
};
const handleClose = () => {
  isOpenAdd.value = false;
};
</script>

<template>
  <!-- ----------------------------------------------------------------------------- -->
  <!-- Dencity -->
  <!-- ----------------------------------------------------------------------------- -->
  <div>
    <div class="mt-4">
      <h2 class="heading-tb mb-3">Thông tin phòng</h2>
      <div>
        <v-btn @click="openDialogAdd(item)" class="mb-4" color="secondary"
          >Thêm mới</v-btn
        >
      </div>
      <v-table>
        <thead>
          <tr>
            <th
              class="text-subtitle-1 font-weight-bold text-center font-weight-black"
            >
              STT
            </th>
            <th
              class="text-subtitle-1 font-weight-bold text-center font-weight-black"
            >
              Tên khách sạn
            </th>
            <th
              class="text-subtitle-1 font-weight-bold text-center font-weight-black"
            >
              Loại khách sạn
            </th>
            <th
              class="text-subtitle-1 font-weight-bold text-center font-weight-black"
            >
              Điện thoại
            </th>
            <th
              class="text-subtitle-1 font-weight-bold text-center font-weight-black"
            >
              Giá từ
            </th>
            <th
              class="text-subtitle-1 font-weight-bold text-center font-weight-black"
            >
              Mô tả
            </th>
            <th
              class="text-subtitle-1 font-weight-bold text-center font-weight-black"
            >
              Hình ảnh
            </th>
            <th
              class="text-subtitle-1 font-weight-bold text-center font-weight-black"
            >
              Ngày tạo
            </th>
            <th
              class="text-subtitle-1 font-weight-bold text-center font-weight-black"
            >
              Cập nhật
            </th>
            <th
              class="text-subtitle-1 font-weight-bold text-center font-weight-black"
            >
              Chức năng
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in desserts" :key="item.id">
            <td class="text-center">{{ item.id }}</td>
            <td class="text-center">{{ item.hotelName }}</td>
            <td class="text-center">{{ item.hotelType }}</td>
            <td class="text-center">{{ item.phone }}</td>
            <td class="text-center">{{ item.fromPrice }} VNĐ</td>
            <td class="text-center">{{ item.description }}</td>
            <td class="text-center">
              <img
                style="height: 100px; width: 80px; border-radius: 4px"
                :src="`http://localhost:8081/uploads/${item.images[0].url}`"
                alt=""
              />
            </td>
            <td class="text-center">
              {{
                moment(item.createdDate).format("dddd, MMMM Do YYYY, h:mm:ss a")
              }}
            </td>
            <td class="text-center">
              {{
                moment(item.modifiedDate).format(
                  "dddd, MMMM Do YYYY, h:mm:ss a"
                )
              }}
            </td>
            <td style="min-width: 160px; text-align: center">
              <v-btn
                icon="mdi-pencil"
                color="warning"
                class="mr-1"
                size="small"
              ></v-btn>
              <v-btn
                icon="mdi-delete"
                @click="openDialogDelete(item)"
                color="errorr"
                size="small"
              ></v-btn>
            </td>
          </tr>
        </tbody>
      </v-table>
    </div>
  </div>
  <div class="text-center">
    <v-pagination
      v-model="page"
      :length="totalPage"
      color="success"
      rounded="circle"
    ></v-pagination>
  </div>
  <HotelAdd
    :isOpen="isOpenAdd"
    @close="handleClose"
    @add="handleAdd"
    :data="dataAdd"
  />
</template>
<style>
.heading-tb {
  text-align: center;
  text-transform: uppercase;
}
</style>
