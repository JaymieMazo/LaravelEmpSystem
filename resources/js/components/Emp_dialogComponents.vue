<template>
<v-card class="rounded-xl">
        <v-card-title  
         style="font-family:cursive ; font-size:30px"
              class="text-center justify-center py-6 teal lighten-4"
           >{{this.tab_head}}
        </v-card-title >
        <v-card-text>
         <v-tabs   v-model="tab" color="basil" background-color="transparent" >
              <v-tab  v-for="(item, key) in itemTabs" :key="key" @change="tab_change(key)" 
              style="font-family:cambria ; font-size:15px">
             <b>   {{item}} </b>  
              </v-tab>


              <v-tabs-items  v-model="tab" >
                  <v-tab-item>
                        <v-row>
                              <v-col cols=5>
                                        <v-text-field
                                          label="Enter Employee Name"
                                          v-model="employee_name"
                                          >
                                        </v-text-field>
                              </v-col>

                              <v-col cols=2>
                                          <v-text-field
                                            label="Enter Age"
                                            v-model="age"
                                            >
                                          </v-text-field>
                              </v-col>

                              <v-col cols=2>
                                        <v-text-field
                                          label="Enter Birthday"
                                          v-model="birthday"
                                          >
                                        </v-text-field>
                            </v-col>

                            <v-col  cols=2>
                                  <v-select
                                          v-model="gender"
                                            :items="genderList" 
                                          item-text="name"
                                          item-value="id"
                                          label="Select Gender">
                                    </v-select>
                            </v-col>

                        
                        </v-row>

                        <v-row>
                              <v-col cols=3>
                                        <v-select  v-model="civilstatus"
                                        label="Enter Civil Status" 
                                        :items="civilStat"
                                        >
                                        </v-select>
                                      </v-col>



                                <v-col cols=3>
                                    <v-text-field label="Nationality" v-model="nationality"></v-text-field>
                                </v-col>
                                <v-col cols=3>
                                      <v-text-field label="Religion" v-model="Religion">
                                      </v-text-field>
                                </v-col>

                                <v-col cols=2>
                                      <v-text-field label="No. of Children" v-model="Children"></v-text-field>
                                </v-col>
                        </v-row>

                        </v-tab-item>

                          <v-tab-item>
                                    <v-row >
                                      <v-col cols=3>
                                            <v-text-field label="Enter Cellphone No." v-model="cpno" ></v-text-field>
                                      </v-col> 

                                        <v-col cols=3>
                                            <v-text-field  label="Enter Telephone No."
                                            v-model="telno" ></v-text-field>
                                      </v-col>

                                      
                                        <v-col cols=4>
                                            <v-text-field label="Enter email No " v-model="email"></v-text-field>
                                      </v-col>
                                    </v-row>


                                      <v-row >
                                      <v-col cols=3>
                                            <v-text-field label="Enter  present  Address" 
                                              v-model="address"></v-text-field>
                                      </v-col> 

                                        <v-col cols=3>
                                            <v-text-field label="Enter Contact Person"
                                            v-model="contact"></v-text-field>
                                      </v-col>

                                      
                                        <v-col cols=4>
                                            <v-text-field label="Enter  Contact Person CP No.   " v-model="con_cpno"></v-text-field>
                                      </v-col>
                              </v-row>
                  </v-tab-item>

                    <v-tab-item>
                          <v-row>
                                <v-col cols="3">
                                      <v-select
                                                v-model="company_code"
                                                    :items="companies"
                                                    item-text="company_name"
                                                    item-value="company_code"
                                                    label="select Company ">
                                      </v-select>
                                </v-col>
                                <v-col cols="4">
                                    <v-select
                                        v-model="department_code"
                                        :items="filteredDept"
                                        item-text="department_name"
                                        item-value="department_code"
                                          label="select Department ">

                                    </v-select>
                                </v-col>
                                <v-col cols="4"> 
                                    <v-select v-model="section_code"
                                              :items="filteredSection"
                                              item-text="section_name"
                                              item-value="section_code"
                                              label="select Section "
                                      >
                                    </v-select>
                                </v-col>
                                  </v-row>


                        <v-row> 
                                <v-col cols=3 >
                                        <v-select
                                            v-model="status"
                                            :items="statusList"
                                            item-text="name"
                                            item-value="id"
                                            label="Select Contract Status" >
                                      </v-select>
                              </v-col>
                            <v-col cols=3>
                                <v-text-field label="Enter Hired Date"></v-text-field>
                            </v-col>
                            <v-col cols=3>
                                <v-text-field label="Enter Retired Date"></v-text-field>
                            </v-col>
                                
                            </v-row>
                          </v-tab-item>
                          <v-btn @click="save()" color="primary">Save</v-btn>
               </v-tabs-items>   
                </v-tabs>
        </v-card-text>
        </v-card>
        </template>




<script>
        export default {
            data(){
                return{
                     genderList:[{id:1 , name:'Male'} , {id:0 , name:'Female'}],
                    statusList:[ {id:'C' , name:'Contractual'} , {id:'R' , name: 'Regular'}],
                    itemTabs:['Personal Information' , 'Contact Information' , 'Employment Information'], 
                         // personal Info
                    employee_code:null, 
                    employee_name:null, 
                    age:null,
                    birthday:null,
                     gender:null , 
                     civilstatus:null,
                     nationality:null,
                     religion:null,
                     children:null,
                    //  Contact Info
                    cpno:null,
                    tellno:null,
                    email:null,
                    address:null,
                    contact:null,
                    con_cpno:null, 
                    // employment Info
                     status:null , 
                     hired_date:null,
                     resigned_date:null,
                    company_code: null,
                    department_code:null, 
                    section_code_orig:null,
                    // others
                     tab:0,
                    tab_head:'Personal Information',
                     genderList:[{id:1 , name:'Male'} , {id:0 , name:'Female'}],
                    statusList:[ {id:'C' , name:'Contractual'} , {id:'R' , name: 'Regular'}],
                }
            },


            
        }
</script>