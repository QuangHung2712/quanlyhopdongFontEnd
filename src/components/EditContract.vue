<template>
  <v-container>
    <p>{{ Idcontract.ReadOrEdit }} {{  }}</p>
    <v-form>
      <v-row>
        <v-col cols="4">
          <v-text-field label="ID Hợp Đồng" outlined :readonly="isdisabled" v-model="contractData.id"></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-text-field label="Phòng Ban" outlined :readonly="isdisabled" v-model="contractData.Departments"></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-text-field label="Tên Nhân Viên" outlined :readonly="isdisabled" v-model="contractData.name"></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="4">
          <v-text-field label="ID Nhân Viên" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-text-field label="Loại Hợp Đồng" outlined :readonly="isdisabled" v-model="contractData.contractType"></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-text-field label="Ngày Lập" type="date" outlined :readonly="isdisabled" v-model="contractData.FromDate"></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="4">
          <v-text-field label="Ngày Hết Hạn" type="date" outlined :readonly="isdisabled" v-model="contractData.ToDate"></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-text-field label="Lương Cơ Bản" type="number" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-text-field label="Lương Hiệu Suất" type="number" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="4">
          <v-text-field label="Phụ Cấp" type="number" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-text-field label="Subject" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-text-field label="Vị Trí" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="4">
          <v-text-field label="Ngày Kết Thúc" type="date" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
        <v-col cols="8">
          <v-textarea label="Ghi Chú" outlined :readonly="isdisabled"></v-textarea>
        </v-col>
      </v-row>
    </v-form>
  </v-container>
</template>

<script>
export default {
  props: {
    Idcontract: {
      type: Object,
      required: true,
      default: ()=>({
        id:0,
        ReadOrEdit: false,
      })
    },
  },
  data() {
    return {
      Contracts: [
        { id: 1, name: 'Phạm Quang Hưng', position: 'Nhân viên', contractType: 'Dài hạn', Departments: 'Hành chính', FromDate: '2024-08-12', ToDate: '2024-08-12' },
        { id: 2, name: 'Phạm Quang Hưng', position: 'Trưởng phòng', contractType: 'Hạn định', Departments: 'Hành chính', FromDate: '2024-08-12', ToDate: '2024-08-12' },
        { id: 3, name: 'Phạm Quang Hưng', position: 'Nhân viên', contractType: 'Thử việc', Departments: 'Hành chính', FromDate: '2024-08-12', ToDate: '2024-08-12' },
        { id: 4, name: 'Phạm Quang Hưng', position: 'Nhân viên', contractType: 'CTV Part Time', Departments: 'Hành chính', FromDate: '2024-08-12', ToDate: '2024-08-12' },
        { id: 5, name: 'Phạm Quang Hưng', position: 'Nhân viên', contractType: 'CTV Full Time', Departments: 'Hành chính', FromDate: '2024-08-12', ToDate: '2024-08-12' },
        { id: 6, name: 'Phạm Quang Hưng', position: 'Nhân viên', contractType: 'Dài hạn', Departments: 'Hành chính', FromDate: '2024-08-12', ToDate: '2024-08-12' },
        { id: 7, name: 'Phạm Quang Hưng', position: 'Nhân viên', contractType: 'Dài hạn', Departments: 'Hành chính', FromDate: '2024-08-12', ToDate: '2024-08-12' },
        { id: 8, name: 'Phạm Thị Bích Hà', position: 'Nhân viên', contractType: 'Dài hạn', Departments: 'Hành chính', FromDate: '2024-08-12', ToDate: '2024-08-12' },
        { id: 9, name: 'Phạm Minh Trang', position: 'Nhân viên', contractType: 'Dài hạn', Departments: 'Hành chính', FromDate: '2024-08-12', ToDate: '2024-08-12' },
      ],
    };
  },
  computed: {
    contractData() {
      return this.Contracts.find(item => item.id === this.Idcontract.id) || {};
    },
    isdisabled() {
      return this.Idcontract.id !== 0;
    },
  },
};
</script>

<style scoped>
.Container {
  background-color: white;
  height: 700px;
}
</style>
