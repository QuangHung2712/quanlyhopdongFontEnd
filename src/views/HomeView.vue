<template>
  <v-container class="bg-gray">
    <v-row>
      <v-col cols="6">
        <h1>Contracts {{ department }}</h1>
      </v-col>
      <v-col offset="1" cols="2" class="d-flex justify-center align-center">
        <v-btn class="rounded-xl" color="primary" @click="BtnContractSample">Contract Sample</v-btn>
      </v-col>
      <v-col offset="1" cols="2" class="d-flex justify-center align-center">
        <v-btn class="rounded-xl" color="primary" @click="BtnAddContract">Add new Contract</v-btn>
      </v-col>
    </v-row>
    <v-container class="bg-white rounded-xl">
      <v-row class="h60px">
        <v-col cols="3">
          <v-text-field
              v-model="search"
              :loading="loading"
              append-inner-icon="mdi-magnify"
              label="Search Empolyee"
              variant="solo"
              hide-details
            >
          </v-text-field>
        </v-col>
        <v-col offset="1" cols="2">
          <v-select
            v-model="selectedPosition"
            clearable
            label="Position"
            :items="positions"
            item-value="name"
            item-title="name"
            variant="outlined"
            class="rounded-lg"
            >
          </v-select>

        </v-col>
        <v-col offset="1" cols="2">
          <v-select
            v-model="selectedContractType"
            clearable
            label="Contract Type"
            :items="contractType"
            item-value="title"
            item-title="title"
            variant="outlined"
            class="rounded-lg">
          </v-select>
        </v-col>
        <v-col offset="1" cols="2">
          <v-select
            clearable
            label="Status"
            :items="['California', 'Colorado', 'Florida', 'Georgia', 'Texas', 'Wyoming']"
            variant="outlined"
            class="rounded-lg">
          </v-select>
        </v-col>
      </v-row>
      <v-data-table
        :headers="headers"
        :items="filteredContracts"
        class="custom-data-table"
      >
        <template v-slot:[`item.actions`]="{ item }">
          <v-icon small @click="viewDetails(item)">mdi-eye</v-icon>
        </template>
      </v-data-table>
      <v-dialog v-model="dialogDetails" max-width="1000px">
      <v-card>
        <v-card-title class="text-h4 d-flex justify-space-between">
          {{ titleDiaLog }}
          <div v-show="showBTNDetailsContract">
            <v-btn class="actions" @click="ExportContract">Xuất file word</v-btn>
            <v-btn class="actions" @click="BtnEditContract">Sửa</v-btn>
            <v-btn class="actions" @click="btnDeleteContract">Xóa</v-btn>
            <v-btn class="actions">Kết thúc hợp đồng</v-btn>
            <v-btn @click="dialogDetails= false" class="actions">Cancel</v-btn>
          </div>
        </v-card-title>
        <v-card-text>
          <v-container>
    <p></p>
    <v-form>
      <v-row>
        <v-col cols="4">
          <v-text-field label="ID Hợp Đồng" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-text-field label="Phòng Ban" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-text-field label="Tên Nhân Viên" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="4">
          <v-text-field label="ID Nhân Viên" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-text-field label="Loại Hợp Đồng" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
        <v-col cols="4">
          <v-text-field label="Ngày Lập" type="date" outlined :readonly="isdisabled"></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="4">
          <v-text-field label="Ngày Hết Hạn" type="date" outlined :readonly="isdisabled"></v-text-field>
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
        </v-card-text>
        <div  v-show="showBTNAddContract">
          <v-card-actions class="d-flex justify-space-between">
          <v-btn class="actions">Lưu</v-btn>
          <v-btn class="actions" @click="CancelEditContract">Close</v-btn>
        </v-card-actions>
        </div>
      </v-card>
    </v-dialog>
    </v-container>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HomeView',

  data(){
    return{
      department: 'Administrative department',
      showBTNDetailsContract:false,
      showBTNAddContract:false,
      search:'',
      positions:[],
      titleDiaLog: '',
      selectedPosition: null,
      selectedContractType: null,
      selectedIdContract:0,
      dialogDetails: false,
      Contracts:[
      ],
      headers:[
      {title:'Tên nhân viên',value:'employeeName'},
      {title:'Chức vụ',value:'positionName'},
      {title:'Loại hợp đồng',value:'employeeContractType'},
      {title:'Phòng ban',value:'departmentName'},
      {title:'Ngoài lập hợp đồng',value:'fromDate'},
      {title:'Ngày hết hạn',value:'toDate'},
      {title:'Actions',value:'actions',sortable: false},
      ],
      contractType:[
        {title:"CTV PartTime",value:0},
        {title:"CTV FullTime",value:1},
        {title:"Thử việc",value:2},
        {title:"Thời hạn cố định",value:3},
        {title:"Dài hạn",value:4}
      ]
    }
  },
  created(){
    this.getPositions();
    this.getContract();
  },
  computed:{
    filteredContracts() {
      return this.Contracts.filter((contract) => {
        const matchesSearch = contract.employeeName.toLowerCase().includes(this.search.toLowerCase());
        const matchesPosition = this.selectedPosition ? contract.positionName === this.selectedPosition : true;
        const matchesContractType = this.selectedContractType !== null && this.selectedContractType !== undefined ? contract.employeeContractType === this.selectedContractType : true;

        return matchesSearch && matchesPosition && matchesContractType;
      });
    },

  },
  methods:{
    getContract() {
      axios.get('https://localhost:7106/api/Contract/GetContract')
        .then(response => {
          this.Contracts = response.data.result.map(contract => {
            const type = this.contractType.find(type => type.value === contract.employeeContractType);
            if (type) {
              contract.employeeContractType = type.title;
            }
            contract.fromDate = this.formatDate(contract.fromDate)
            contract.toDate = this.formatDate(contract.toDate)
            return contract;
          });
        })
        .catch(error => {
          console.error('Error fetching contract:', error);
        });
  },
    viewDetails(item) {
      this.titleDiaLog = 'Chi tiết hợp đồng';
      this.selectedIdContract = item.id
      this.showBTNDetailsContract=true
      this.showBTNAddContract=false
      this.dialogDetails = true;
    },
    BtnAddContract(){
      this.titleDiaLog = 'Add Contract'
      this.showBTNAddContract=true;
      this.showBTNDetailsContract=false;
      this.dialogDetails = true;
      this.selectedIdContract = 0
    },
    BtnEditContract(){
      this.titleDiaLog= 'Edit Contract'
      this.showBTNAddContract = true
      this.showBTNDetailsContract = false;
    },
    CancelEditContract(){
      if(this.selectedIdContract===0){
        this.dialogDetails = false
        this.selectedIdContract = 0
      }
      else{
        this.showBTNAddContract = false
        this.showBTNDetailsContract = true;
        this.titleDiaLog='Chi tiết hợp đồng';
      }
    },
    UpdateContract(value){
      this.Contracts.push(value);
    },
    getPositions(){
      axios.get('https://localhost:7106/api/Contract/FillDropDowPosition')
        .then(response => {
          this.positions = response.data.result;
        })
        .catch(error => {
          console.error('Error fetching positions:', error);
        });
    },
    async ExportContract(){
       var contract = this.Contracts.find(item => item.id === this.selectedIdContract.id)
       console.log(contract)
      try {
        const response = await fetch('https://localhost:7106/WeatherForecast/ExportContract', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(contract),
        });
        if (response.ok) {
          const blob = await response.blob();
          const url = window.URL.createObjectURL(blob);
          const link = document.createElement('a');
          link.href = url;
          link.download = 'contract.docx';
          link.click();
        } else {
          console.error('Error creating contract');
        }
      } catch (error) {
        console.error('Error:', error);
      }
    },
    formatDate(date){
      if (!date) {
        return
      }
      return new Date(date).toLocaleDateString('en-GB');
    },
    async btnDeleteContract(){
      if(confirm('Bạn có chắc chắn muốn xóa hợp đồng này không ' + this.selectedIdContract)) {
          try {
              const response = await axios.patch(`https://localhost:7106/api/Contract/DeleteContract/${this.selectedIdContract}`);
              if (response.status === 200) {
                  alert('Xóa thành công');
              } else {
                  alert('Xóa không thành công');
              }
          } catch (error) {
              console.error('Lỗi khi xóa hợp đồng:', error);
              alert('Xóa không thành công');
          }
      }
    }
  }
};
</script>
<style scoped>
.bg-gray{
  background-color: #f6f6f6;
}
.p0{
  padding: 0px;
}
.bg-white{
  background-color: white;
}
.h60px{
  height: 80px;
}
.custom-data-table .v-data-table-headers {
  background-color: #f6f6f6;
}
.v-card{
  background-color: #f6f6f6;
}
.actions{
  background-color: #1867c0;
  border-radius: 25px;
  color: white;
  margin-right: 10px;
}
.v-data-table{
  margin-top: 20px;
  border: 1px solid black;
}
</style>
