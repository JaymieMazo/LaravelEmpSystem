<template>
<v-card>
        <v-row  class="pa-2" >
        
                <v-col >
                        <v-select 
                                  v-model="company_code"
                                  :items="companies"
                                  item-text="company_name"
                                  item-value="company_code"
                                  label="Select Company">
                          </v-select>
            
                  </v-col>

                    <v-col >
                        <v-select
                          v-model="department_code"
                                 :items="filteredDept"
                                  item-text="department_name"
                                  item-value="department_code"
                                  label="Select Department" >
                          </v-select>
                  </v-col>



                <v-col>
                        <v-text-field
                          label="Enter Section Name"
                          v-model="section_name_input"
                         >
                        </v-text-field>
                  </v-col>

                <v-col >
                      <v-btn 
                      type="submit"
                      color="primary lighten-1" 
                        @click="save()"  
                        dense>
                              {{this.action}} 
                        </v-btn>

                        <v-btn 
                        color="primary lighten-1" 
                        @click="Cancel() " 
                        dense
                        v-show="this.IsCancelShow"> Cancel 
                        </v-btn>
                </v-col>
        </v-row>



      <v-simple-table >
      
        <template v-slot:default>

              <thead >
                <tr >
                    <th class="text-left">No </th>
                  <th class="text-left">Company </th>
                  <th class="text-left">Department</th>  
                  <th class="text-left">Section</th>  
    
                  <th class="text-left">Created Date</th>
                  <th class="text-left">Deleted Date</th>
                  <th class="text-left">Updated Date</th>
                  <th class="text-left">Updated By</th>

                  <th>Action</th>
                </tr>
              </thead>

                <tbody>
                      <tr
                        v-for="(item, index)  in sec"  
                        :key="index"
                      >
                            <td>{{ index + 1 }}</td>
                            <td>{{ item.company_name }}</td>
                            <td>{{ item.department_name }}</td>
                             <td>{{ item.section_name }}</td>
                            <td>{{ item.created_at }}</td>
                             <td>{{ item.deleted_at }}</td> 
                            <td>{{ item.updated_at }}</td> 
                            <td>{{ item.employee_name }}</td>
                          <td>
                                    <v-btn icon>

                                        <v-icon  

                                              color="primary lighten-2"
                                              @click="edit(
                                                item.company_code,
                                              item.department_code, 
                                              item.section_code,  
                                               item.section_name, 
                                               item.created_at, 
                                               item.updated_at, 
                                                )"
                                              >mdi-book-edit
                                        </v-icon>

                                    </v-btn>

                                  <v-btn icon>
                                          <v-icon
                                      
                                          color="red lighten-2"
                                            @click="Delete(
                                              item.company_code,
                                              item.department_code,
                                              item.section_code)"
                                          >
                                            mdi-trash-can
                                          </v-icon>
                                  </v-btn>
                          </td>
                          
                      </tr>
                  </tbody>

      </template>
      </v-simple-table>   
</v-card>
</template>


<script>

export default {


				data(){
				        return{
				            companies:[],
                    departments:[],
                    sections:[],
				            company_code: null,
                    department_code:null, 
				            section_name_input:null,
				            action:'ADD', 
                    company_code_orig: null, 
                    department_code_orig: null, 
                    section_code:null, 
                    IsCancelShow:false, 
                    created_at:null,
                    updated_at:null,
				        } 
				}, 

				created(){
				        axios.get('api/section' ).then(res=>{
				              this.companies= res.data.Company
                      this.departments=res.data.Department
                      this.sections=res.data.Section
				        })
				},


      computed:{
        filteredDept(){
          return this.departments.filter(rec=>{
                return rec.company_code == this.company_code 
          })
        },

        sec(){

          if(this.company_code == null){
              return  this.sections
          }

          else if(this.company_code != null  && this.department_code != null){
            return this.sections.filter(rec=> {
              return rec.company_code == this.company_code && rec.department_code == this.department_code
            })
          }  else if(this.company_code != null  && this.department_code == null){
              return this.sections.filter(rec=> {
              return rec.company_code == this.company_code
            })
          }

        }
      }, 



			  methods:{
			      save(){
					        axios.post('api/section' , 
					        {action: this.action ,
                   company_code:  this.company_code ,
                    department_code: this.department_code , 
					          section_name: this.section_name_input  ,
                     company_code_orig:this.company_code_orig , 
                      department_code_orig:this.department_code_orig,
                      section_code:this.section_code ,
                      created_at:this.created_at,
                      updated_at:this.updated_at
					            }).then(res=> {
					                  if(res.data=='blank'){
					                      alert('Company and Department and Section Name are required ')
					                  }else if(res.data == 'exist'){
					                      alert('Section already exist')
					                  }else{
                              // alert('refresh')
					                      this.sections=res.data.Section
					                  }
                      })
                }, 
      
      

			      edit(com_code , dep_code, sec_code,  sec_name, created_at , updated_at){
						        this.action='UPDATE'
                    this.IsCancelShow=true
       
                    this.company_code=com_code
                    this.department_code=dep_code
                    this.section_name_input=sec_name
                    this.company_code_orig=com_code
                    this.department_code_orig=dep_code
                    this.section_code=sec_code
                    this.created_at=created_at
                    this.updated_at=updated_at
			      },
            
			      Cancel(){
                this.company_code=''
                this.department_code=''
			          this.section_name_input =''
			          this.IsCancelShow=false
			          this.action='ADD'
			      }, 
			
			       Delete(companyCode, deptcode , secCode ){
                        if(confirm('Are you sure you want to delete? ')){
                                  this.action="DELETE"
                                  axios.post('api/section' ,
                                            {action: this.action, 
                                            company_code:companyCode, 
                                            department_code : deptcode,
                                            section_code : secCode
                                }).then(res=>{
                                      this.sections=res.data.Section
                                      })
                        
                      }  
			            
			
            }, 
            

  				}, 
  }
</script>


