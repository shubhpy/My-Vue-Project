<template>
  <div class="main-content p-0">
    <div class="card h-fullscreen pt-5 mb-0 pl-3">
      <div class="card">
        <div class="card-body">
          <ul class="nav nav-tabs">
            <li class="nav-item col-sm-2 float-left p-0">
              <a class="nav-link active" data-toggle="tab" href="#In">Vendor In</a>
            </li>
            <li class="nav-item col-sm-2 float-left p-0">
              <a class="nav-link" data-toggle="tab" href="#Out">Collect Out</a>
            </li>
          </ul>
          <div class="tab-content">
            <div class="tab-pane fade active show" id="In">
              <br />
              <div class="col-sm-4 brdr">
                <label class="col-sm-12 float-left mb-0 pl-0">Add / Search Vendor :</label><br />
                <Autocomplete class="col-sm-12 p-0" :suggestions="vendors" v-model="selectedVendor"></Autocomplete><br />
                <label for="typeahead-focus col-sm-12 float-left" style="display:block">Vendor : {{selectedVendor}}</label>
                <div class="col-sm-6 pl-0 pt-4">
                  <label class="demonstration d-block">Date</label>
                  <el-date-picker class="col-sm-12 p-0" v-model="enteredDate" type="date" placeholder="Pick a Date" format="dd/MM/yyyy" :picker-options="pickerOptions1"></el-date-picker>
                </div>
                <div class="col-sm-6 pl-0 pr-0 pt-4">
                  <label class="d-block">Bill No:</label>
                  <input type="text" class="form-control" style="height:40px;" v-model="enteredBillNo" />
                </div>
                <div class="col-sm-8 pl-0 pr-0 pt-5">
                  <label>Select User:</label>
                  <Autocomplete :suggestions="users" v-model="selectedUser"></Autocomplete>
                  <label for="typeahead-focus col-sm-12 float-left text-primary">Selected user is : {{selectedUser}}</label>
                </div>
              </div>
              <div class="col-sm-8 card vendorItems mb-0">
                <div class="card-body pl-0 pr-3 pt-1">
                  <table id="vendor-items" class="table table-striped table-bordered dataTable display" cellspacing="0" cellpadding="0">
                    <thead>
                      <tr style="background:#f4f4f4;">
                        <th class="frst">&nbsp;</th>
                        <th class="scnd">
                          <label class="d-block">Select Item Name</label>
                          <Autocomplete :suggestions="items" v-model="selectedItem"></Autocomplete>              
                        </th>
                        <th class="thrd">
                          <label class="d-block">No. of Boxes<span class="text-primary"> (if applicable)</span></label>
                          <input type="text" class="form-control col-sm-4" placeholder="Boxes" v-model="noBoxes">
                        </th>
                        <th class="frth">
                          <label class="d-block">Qty. in each box</label>
                          <input type="text" class="form-control col-sm-4" placeholder="Quantity" v-model="quantity">
                        </th>
                        <th class="ffth">
                          <label class="d-block">Add Item(s)</label>
                          <a class="col-sm-6 btn btn-primary btn-outline text-dark p-2" @click="addItem">+</a>
                        </th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr style="background:#fff;position:relative;">
                        <div class="poAbs">
                          <div class="col-sm-12 p-0">
                            <div class="col-sm-2"><ins>Sr no.</ins></div>
                            <div class="col-sm-3"><ins>Item Name</ins></div>
                            <div class="col-sm-2"><ins>Boxes</ins></div>
                            <div class="col-sm-2"><ins>Quantity</ins></div>
                            <div class="col-sm-2"><ins>Actions</ins></div>
                          </div>
                        </div>
                      </tr>
                      <tr style="position:relative;">
                        <div class="poAbs mt-5">
                          <div class="col-sm-12 p-0" v-for="(ai,index) in addedItems">
                            <div class="col-sm-2 pt-1 pb-1">{{index+1}}.</div>
                            <div class="col-sm-3 pt-1 pb-1">{{ai.name}}</div>
                            <div class="col-sm-2 pt-1 pb-1">{{ai.boxes}}</div>
                            <div class="col-sm-2 pt-1 pb-1">{{ai.quantity}}</div>
                            <div class="col-sm-2 pt-1 pb-1"> <a class="btn btn-w-md btn-danger nav-action d-inline text-white"><span class="ti-close" @click="deleteItem(index)">Delete</span></a> </div>
                          </div>
                        </div>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
              <div class="fab fab-fixed">
                <a class="btn btn-primary btn-outline mr-5 text-dark"  @click="postNewItemsEntry">Submit</a>
              </div>
            </div>
            <div class="tab-pane fade" id="Out">
              <br />
              <div class="col-sm-4 brdr">
                <label class="col-sm-12 float-left mb-0 pl-0">Collector Name:</label><br />
                <Autocomplete class="col-sm-12 p-0" :suggestions="receivers" v-model="selectedReceiver"></Autocomplete><br />
                <label for="typeahead-focus col-sm-12 float-left" style="display:block">Selected Collector : {{selectedReceiver}}</label>
                <div class="col-sm-8 pl-0 pr-0 pt-3">
                  <label>Dept/Board:</label>
                  <Autocomplete :suggestions="boards" v-model="selectedBoard"></Autocomplete>
                  <label for="typeahead-focus col-sm-12 float-left text-primary">Location selected : {{selectedBoard}}</label>
                </div>
                <div class="col-sm-6 pl-0 pt-4">
                  <label class="demonstration d-block">Date</label>
                  <el-date-picker class="col-sm-12 p-0" v-model="enteredDate_Collect" type="date" placeholder="Pick a Date" format="dd/MM/yyyy" :picker-options="pickerOptions1"></el-date-picker>
                </div>
              </div>
              <div class="col-sm-8 card vendorItems mb-0">
                <div class="card-body pl-0 pr-3 pt-1">
                  <table id="vendor-items" class="table table-striped table-bordered dataTable display" cellspacing="0" cellpadding="0">
                    <thead>
                      <tr style="background:#f4f4f4;">
                        <th class="frst">&nbsp;</th>
                        <th class="scnd">
                          <label class="d-block">Select Item Name</label>
                          <Autocomplete :suggestions="items" v-model="selectedItem_Collect"></Autocomplete>              
                        </th>
                        <th class="thrd">
                          <label class="d-block">Qty. in each box</label>
                          <input type="text" class="form-control col-sm-4" placeholder="Quantity" v-model="quantity_Collect">
                        </th>
                        <th class="ffth">
                          <label class="d-block">Add Item(s)</label>
                          <a class="col-sm-6 btn btn-primary btn-outline text-dark p-2" @click="addItem_Collect">+</a>
                        </th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr style="background:#fff;position:relative;">
                        <div class="poAbs">
                          <div class="col-sm-12 p-0">
                            <div class="col-sm-2"><ins>Sr no.</ins></div>
                            <div class="col-sm-3"><ins>Item Name</ins></div>
                            <div class="col-sm-2"><ins>Quantity</ins></div>
                            <div class="col-sm-2"><ins>Actions</ins></div>
                          </div>
                        </div>
                      </tr>
                      <tr style="position:relative;">
                        <div class="poAbs mt-5">
                          <div class="col-sm-12 p-0" v-for="(ai,index) in addedItems_Collect">
                            <div class="col-sm-2 pt-1 pb-1">{{index+1}}.</div>
                            <div class="col-sm-3 pt-1 pb-1">{{ai.name}}</div>
                            <div class="col-sm-2 pt-1 pb-1">{{ai.quantity}}</div>
                            <div class="col-sm-2 pt-1 pb-1"> <a class="btn btn-w-md btn-danger nav-action d-inline text-white"><span class="ti-close" @click="deleteItem_Collect(index)">Delete</span></a> </div>
                          </div>
                        </div>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
              <div class="fab fab-fixed">
                <a class="btn btn-primary btn-outline mr-5 text-dark" @click="postNewItemsEntry_Collect">Submit</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import router from '../router'
  import Autocomplete from './Autocomplete'  
  
  export default {
  name: 'ItemEntry',
  components: {
      Autocomplete
    },
  data() {
    return {
      vendors: [
        {name:"Aggarwal",quantity:""},
        {name:"Horizon Computers",quantity:""},
        {name:"Cafe Coffee",quantity:""},
      ],
      items: [
          {name:"Stapler",quantity:""},
          {name:"Stapler pin",quantity:""},
          {name:"Milk",quantity:""},
          {name:"Green Tea",quantity:""}
        ],
      users: [
          {name:"Sanjeev",quantity:""},
          {name:"Kumar",quantity:""},
          {name:"Halke Ram",quantity:""},
          {name:"Ram Chand",quantity:""}
        ],
      boards: [
        {name:"NABH",quantity:""},
        {name:"ZED",quantity:""},
        {name:"IAEA Office",quantity:""},
        {name:"NABET",quantity:""}
      ],
      receivers :[
          {name:"Sanjeev",quantity:""},
          {name:"Kumar",quantity:""},
          {name:"Halke Ram",quantity:""},
          {name:"Ram Chand",quantity:""}
        ],

      enteredDate : "",
      enteredBillNo : "",
      selectedVendor : "",
      selectedUser : "",
      selectedItem : "",
      noBoxes : '0',
      quantity : '0',
      addedItems : [],

      selectedReceiver : "",
      selectedBoard : "",
      enteredDate_Collect : "",
      selectedItem_Collect : "",
      quantity_Collect : '0',
      addedItems_Collect : [],

      pickerOptions1: {
          // disabledDate(time) {
          //   return time.getTime() > Date.now();
          // },
          shortcuts: [{
            text: 'Today',
            onClick(picker) {
              picker.$emit('pick', new Date());
            }
          }, {
            text: 'Yesterday',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() - 3600 * 1000 * 24);
              picker.$emit('pick', date);
            }
          }, {
            text: 'Tomorrow',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() + 3600 * 1000 * 24);
              picker.$emit('pick', date);
            }
          },{
            text: 'A week ago',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit('pick', date);
            }
          }]
        },
    }
  },
  methods : {
    deleteItem: function(index,e){
      this.addedItems.splice(index, 1)
    },
    addItem: function(e) {
      e.preventDefault();
      var i
      var found = false
      if (this.selectedItem && this.noBoxes && parseInt(this.quantity)){
        for (i = 0; i < this.addedItems.length; i++) { 
            if (this.selectedItem == this.addedItems[i].name){
              found = true
              this.addedItems[i].boxes = parseInt(this.addedItems[i].boxes) + parseInt(this.noBoxes)
              this.addedItems[i].quantity = parseInt(this.addedItems[i].quantity) + parseInt(this.quantity)
            }
        }
        if (!found){
          this.addedItems.push({name:this.selectedItem,boxes:this.noBoxes,quantity:this.quantity})
        }
      }else{
        console.log("Details not entered")
      }
    },
    postNewItemsEntry: function() {
      if (this.selectedVendor!="" && this.selectedUser!="" && this.enteredBillNo!="" && this.enteredDate!="" && this.addedItems.length){
        console.log("Here");      
        var datatosend = {
          vendor : this.selectedVendor,
          user : this.selectedUser,
          bill : this.enteredBillNo,
          date : this.enteredDate,
          items: this.addedItems
        };
        this.$http.post( this.$hostname + 'input',JSON.stringify(datatosend))
        .then(function (data) {
          console.log(data.body);
          if (data.body.status){
            alert("Entry saved")
            this.selectedVendor="" 
            this.selectedUser=""
            this.enteredBillNo=""
            this.enteredDate=""
            this.addedItems=[]
          }else{
            alert("Some error occured")          
          }
        }.bind(this),function(data){
          console.log(data.body);
          alert("Some error occured")
        })
      }else{
        alert("Fill all the details as Vendor, User, Date ,Billno and atleast one item in items")
      }
    },
    postNewItemsEntry_Collect:function(){
      if (this.selectedReceiver!="" && this.selectedBoard!="" && this.enteredDate_Collect!="" && this.addedItems_Collect.length){
        console.log("Here");      
        var datatosend = {
          collector : this.selectedReceiver,
          board : this.selectedBoard,
          date : this.enteredDate_Collect,
          items: this.addedItems_Collect
        };
        this.$http.post( this.$hostname + 'output',JSON.stringify(datatosend))
        .then(function (data) {
          console.log(data.body);
          if (data.body.status){
            alert("Entry saved")
            this.selectedReceiver=""
            this.selectedBoard=""
            this.enteredDate_Collect=""
            this.addedItems_Collect=[]
          }else{
            alert("Some error occured")          
          }
        }.bind(this),function(data){
          console.log(data.body);
          alert("Some error occured")
        })
      }else{
        alert("Fill all the details as Item Collector, Board, Date and atleast one item in items")
      }
    },
    addItem_Collect: function(e) {
      e.preventDefault();
      var i
      var found = false
      if (this.selectedItem_Collect && parseInt(this.quantity_Collect)){
        for (i = 0; i < this.addedItems_Collect.length; i++) { 
            if (this.selectedItem_Collect == this.addedItems_Collect[i].name){
              found = true
              this.addedItems_Collect[i].quantity = parseInt(this.addedItems_Collect[i].quantity) + parseInt(this.quantity_Collect)
            }
        }
        if (!found){
          this.addedItems_Collect.push({name:this.selectedItem_Collect,quantity:this.quantity_Collect})
        }
      }
    },
    deleteItem_Collect: function(index,e){
      this.addedItems_Collect.splice(index, 1)
    }
  },
  created () {
    fetch(this.$hostname + 'getnames')
      .then(response => response.json())
      .then(json => {
        console.log(json)
        if (json.success){
          this.vendors = json.vendors
          this.items = json.item_names
          this.receivers = json.users
          // this.users = json.users
          this.users = json.users          
        }else{
          alert("API is working")
        }
    })
  }}
</script>

<style>
  @import url("//unpkg.com/element-ui@2.3.3/lib/theme-chalk/index.css");
</style>
