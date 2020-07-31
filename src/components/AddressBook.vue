<template>
  <div>
    <el-menu class="el-menu-demo" mode="horizontal">
      <el-menu-item index="1">
        Address Book
      </el-menu-item>
    </el-menu>
    <el-row :gutter="24" class="box-container">
      <el-col :span="22" :offset="1">
        <template>
          <el-table
            :data="contactData"
            :default-sort = "{prop: 'id', order: 'ascending'}"
            @row-dblclick="RowDblclick"
            @row-click="RowClick"
            style="width: 100%"
            border>
            <el-table-column
              width="40">
              <template slot="header" slot-scope="scope">
                <el-checkbox v-model="isAllSelected" @change="selectAll()">
                </el-checkbox>
              </template>
              <template slot-scope="scope">
                <el-checkbox v-model="scope.row.isSelected" @change="selectOne()">
                </el-checkbox>
              </template>
            </el-table-column>
            <el-table-column
              prop="id"
              label="ID"
              sortable>
            </el-table-column>
            <el-table-column
              prop="name"
              label="Name"
              sortable>
              <template
                slot-scope="scope">
                <el-input
                  v-show="isDoubleClick==scope.row.id"
                  v-model="scope.row.name"
                  placeholder="Please enter"
                  type="text"
                  size="mini"
                  clearable>
                </el-input>
                <span v-show="isDoubleClick!=scope.row.id">{{scope.row.name}}</span>
              </template>
            </el-table-column>
            <el-table-column
              prop="location"
              label="Location"
              sortable>
              <template
                slot-scope="scope">
                <el-input
                  v-show="isDoubleClick==scope.row.id"
                  v-model="scope.row.location"
                  placeholder="Please enter"
                  type="text"
                  size="mini"
                  clearable>
                </el-input>
                <span v-show="isDoubleClick!=scope.row.id">{{scope.row.location}}</span>
              </template>
            </el-table-column>
            <el-table-column
              prop="office"
              label="Office"
              sortable>
              <template
                slot-scope="scope">
                <el-input
                  v-show="isDoubleClick==scope.row.id"
                  v-model="scope.row.office"
                  placeholder="Please enter"
                  type="text"
                  size="mini"
                  clearable>
                </el-input>
                <span v-show="isDoubleClick!=scope.row.id">{{scope.row.office}}</span>
              </template>
            </el-table-column>
            <el-table-column
              label="Phone">
              <el-table-column
                prop="officePhone"
                label="Office"
                sortable>
                <template slot-scope="scope">
                  <el-input
                    v-show="isDoubleClick==scope.row.id"
                    v-model="scope.row.officePhone"
                    placeholder="Please enter"
                    type="text"
                    size="mini">
                  </el-input>
                  <span v-show="isDoubleClick!=scope.row.id">{{scope.row.officePhone}}</span>
                </template>
              </el-table-column>
              <el-table-column
                prop="cellPhone"
                label="Cell"
                sortable>
                <template slot-scope="scope">
                  <el-input
                    v-show="isDoubleClick==scope.row.id"
                    v-model="scope.row.cellPhone"
                    placeholder="Please enter"
                    type="text"
                    size="mini">
                  </el-input>
                  <span v-show="isDoubleClick!=scope.row.id">{{scope.row.cellPhone}}</span>
                </template>
              </el-table-column>
            </el-table-column>
          </el-table>
        </template>
      </el-col>
    </el-row>
    <el-row :gutter="24"  class="box-container">
      <el-col :span="15" :offset="1">
        <el-button @click="deleteData()" type="danger">Delete</el-button>
      </el-col>
      <el-col :span="3" :offset="2">
        <el-button @click="addData()">Add</el-button>
      </el-col>
      <el-col :span="3">
        <el-button @click="updateData()">Update</el-button>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: 'AddressBook',
  data () {
    return {
      contactData: [
        {
          isSelected: false,
          id: '11',
          name: 'xx',
          location: 'xx',
          office: 'Shanghai',
          officePhone: '100-200-300',
          cellPhone: '101-200-300'
        },
        {
          isSelected: false,
          id: '1',
          name: 'xx',
          location: 'xx',
          office: 'Anhui',
          officePhone: '100-200-300',
          cellPhone: '100-200-300'
        },
        {
          isSelected: false,
          id: '302',
          name: 'xx',
          location: 'xx',
          office: 'Xizang',
          officePhone: '900-200-300',
          cellPhone: '200-200-300'
        },
        {
          isSelected: false,
          id: '304',
          name: 'xx',
          location: 'xx',
          office: 'Zhejiang',
          officePhone: '600-200-300',
          cellPhone: '200-200-300'
        },
        {
          isSelected: false,
          id: '501',
          name: 'xx',
          location: 'xx',
          office: 'Shenyang',
          officePhone: '100-200-300',
          cellPhone: '500-200-300'
        }
      ],
      isAllSelected: false,
      isDoubleClick: null,
      updatedItemID: []
    }
  },
  created () {
    this.contactData = JSON.parse(localStorage.getItem('contact_data')) || this.contactData
    this.selectOne()
  },
  watch: {
    contactData: {
      handler () {
        localStorage.setItem('contact_data', JSON.stringify(this.contactData))
      },
      deep: true
    }
  },
  methods: {
    // 设置全选和反选 根据当前isAllSelected的状态设置下面isSelected的状态
    selectAll () {
      this.contactData.forEach(item => (item.isSelected = this.isAllSelected))
    },
    // 设置单选 根据对于item的isSelected控制上面isAllSelected的状态
    selectOne () {
      this.isAllSelected = this.contactData.every(item => item.isSelected)
    },
    // 删除选中的item
    deleteData () {
      this.contactData = this.contactData.filter(item => (item.isSelected === false))
      this.isAllSelected = false
    },
    // 增加个新的item
    addData () {
      let maxIdValue = Math.max.apply(Math, this.contactData.map(o => o.id))
      let data = {
        isSelected: false,
        id: maxIdValue + 1,
        name: '',
        location: '',
        office: '',
        officePhone: '',
        cellPhone: ''
      }
      this.contactData.unshift(data)
    },
    // 更新所有内容
    updateData () {
      const updatedItems = this.contactData.filter(item => (this.updatedItemID.indexOf(item.id) !== -1))
      this.$message({
        showClose: true,
        message: JSON.stringify(updatedItems),
        type: 'success'
      })
    },
    // 行的双击事件,进行编辑。 注意:双击事件会触发两次行的单击事件
    RowDblclick: function (row, column, event) {
      this.isDoubleClick = row.id
      this.updatedItemID.push(row.id)
    },
    // 行的单击事件, 取消编辑.
    RowClick: function (row, column, event) {
      if (this.isDoubleClick !== row.id) {
        this.isDoubleClick = null
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .box-container {
    margin-top: 10px;
  }
</style>
