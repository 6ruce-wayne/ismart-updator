<template>
    <div class="col-md-12">
        <div class="card">
            <div class="card-header pb-0">
                <div class="d-flex align-items-center">
                    <p class="mb-0">{{ card.title }}</p>
                    <button class="btn btn-primary btn-sm ms-auto" @click="submit">
                        บันทึก
                    </button>
                </div>
            </div>
            <div class="card-body">
                <p class="text-uppercase text-sm">Basic Information</p>
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label for="companyControl">{{
                                card.company
                            }}</label>
                            <select
                                class="form-control"
                                id="companyControl"
                                v-model="form.company"
                            >
                                <option value="">
                                    {{ card.company }}
                                </option>
                                <option value="40">CHO</option>
                                <option value="41">TMT</option>
                            </select>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label class="form-control-label">{{
                                card.customer_code
                            }}</label>
                            <input
                                class="form-control"
                                type="text"
                                v-model="form.customercode"
                                onfocus="focused(this)"
                                onfocusout="defocused(this)"
                                placeholder="รหัสลูกค้า SAP"
                                @keyup="getcustomer"
                                aria-describedby="emailHelp"
                            />
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label>{{ card.province }}</label>
                            <select
                                class="form-control"
                                v-model="form.province"
                            >
                                <option value="">
                                    {{ card.province }}
                                </option>
                                <option
                                    v-for="item in provinces"
                                    :key="item"
                                    :value="item.ProvinceCode"
                                >
                                    {{ item.ProvinceDescription }}
                                </option>
                            </select>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label>{{ card.customer_type }}</label>
                            <select
                                class="form-control"
                                v-model="form.customertype"
                            >
                                <option value="">
                                    {{ card.customer_type }}
                                </option>
                                <option
                                    v-for="item in customertypes"
                                    :key="item"
                                    :value="item.CustomerGroupCode"
                                >
                                    {{ item.CustomerGroupDescription }}
                                </option>
                            </select>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label>{{ card.customer_group }}</label>
                            <select
                                class="form-control"
                                v-model="form.customergroup"
                            >
                                <option value="">
                                    {{ card.customer_group }}
                                </option>
                                <option
                                    v-for="item in customergroups"
                                    :key="item"
                                    :value="item.Description"
                                >
                                    {{ item.Description }}
                                </option>
                            </select>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label>{{ card.salesman }}</label>
                            <select
                                class="form-control"
                                v-model="form.salesman"
                                @change="this.setSalesOrg"
                            >
                                <option value="">
                                    {{ card.salesman }}
                                </option>
                                <option
                                    v-for="item in salesmans"
                                    :key="item"
                                    :value="item.SalesmanCode"
                                >
                                    {{ item.SalesmanName }}
                                </option>
                            </select>
                        </div>
                    </div>
                </div>
                <hr class="horizontal dark" />
                <p class="text-uppercase text-sm">Customer Information</p>
                <div class="row">
                     <div class="col-md-6">
                        <div class="form-group">
                            <label
                                for="example-text-input"
                                class="form-control-label"
                                >Customer name</label
                            >
                            <input
                                class="form-control"
                                type="text"                               
                                v-model="form.customer.name"
                                :readonly="true"
                                placeholder="ชื่อลูกค้า"
                            />
                        </div>                       
                    </div>
                     <div class="col-md-6">
                        <div class="form-group">
                            <label
                                for="example-text-input"
                                class="form-control-label"
                                >Contact Person</label
                            >
                            <input
                                class="form-control"
                                type="text"                               
                                v-model="form.customer.contact"                               
                                placeholder="ชื่อผู้ติดต่อ"
                            />
                        </div>
                        </div>
                    <div class="col-md-12">
                        <div class="form-group">
                            <label
                                for="example-text-input"
                                class="form-control-label"
                                >Address</label
                            >
                            <input
                                class="form-control"
                                type="text"
                                v-model="form.customer.address"
                                onfocus="focused(this)"
                                :readonly="true"
                                onfocusout="defocused(this)"
                            />
                        </div>
                    </div>
                     <div class="col-md-4">
                        <div class="form-group">
                            <label
                                for="example-text-input"
                                class="form-control-label"
                                >Tax No.</label
                            >
                            <input
                                class="form-control"
                                type="text"
                                v-model="form.customer.tax"
                                :readonly="true"
                                onfocus="focused(this)"
                                onfocusout="defocused(this)"
                            />
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="form-group">
                            <label>{{ card.creditterm }}</label>
                            <select
                                class="form-control"
                                v-model="form.customer.creditterm"   
                                @change="setTerm"                         
                            >
                                <option value="">
                                    {{ card.creditterm }}
                                </option>
                                <option
                                    v-for="item in creditterm"
                                    :key="item"
                                    :value="item.code"                                    
                                >
                                    {{ item.code }} วัน
                                </option>
                            </select>
                        </div>
                    </div>                   
                    <div class="col-md-4">
                        <div class="form-group">
                            <label
                                for="example-text-input"
                                class="form-control-label"
                                >Country</label
                            >
                            <input
                                class="form-control"
                                type="text"
                                value="United States"
                                onfocus="focused(this)"
                                onfocusout="defocused(this)"
                            />
                        </div>
                    </div>
                    <div class="col-md-4">
                        <div class="form-group">
                            <label
                                for="example-text-input"
                                class="form-control-label"
                                >Postal code</label
                            >
                            <input
                                class="form-control"
                                type="text"
                                value="437300"
                                onfocus="focused(this)"
                                onfocusout="defocused(this)"
                            />
                        </div>
                    </div>
                </div>
                <hr class="horizontal dark" />
                <p class="text-uppercase text-sm">About me</p>
                <div class="row">
                    <div class="col-md-12">
                        <div class="form-group">
                            <label
                                for="example-text-input"
                                class="form-control-label"
                                >About me</label
                            >
                            <input
                                class="form-control"
                                type="text"
                                value="A beautiful Dashboard for Bootstrap 5. It is Free and Open Source."
                                onfocus="focused(this)"
                                onfocusout="defocused(this)"
                            />
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import cusotemrgroup from "/../public/json/customergroup.json";
import parthner from "../../public/json/businesspartner.json";
import accountdetail from "../../public/json/accountdetail.json";
export default {
    mounted() {
        this.getProvince();
        this.getCustomerType();
        this.getSalesman();       
    },
    data() {
        return {
            error:[],
            creditterm:[
                {code:"COD",term:"0"},
                {code:"C-30",term:"30"},
                {code:"C-45",term:"45"},
                {code:"C-60",term:"60"},
                {code:"C-90",term:"90"},
            ],
            card: {
                title: "Customer Form",
                company: "เลือกบริษัท",
                customer_code: "Customer Code",
                province: "เลือกจังหวัดหรือประเทศ",
                customer_type: "เลือกประเภทลูกค้า",
                customer_group: "เลือกกลุ่มธรุกิจลูกค้า",
                salesman: "เลือกพนักงานขายที่ดูแล",
                creditterm: "เลือกเงื่อนไขการชำระ"
            },
            provinces: [],
            customertypes: [],
            salesmans: [],
            customergroups: cusotemrgroup,                
            form: {
                company: "",
                customercode: "",
                customertype: "",
                customergroup: "",
                province: "",
                salesman: "",
                salesorganization: '',  
                customer:{
                    contact:'',
                    term:'',
                    creditterm:'',
                    name:'',     
                    address:'',
                    tax:'',
                },          
            },
        };
    },
    methods: {
        getProvince() {
            var x = this;
            var url = "/api/province";
            axios
                .get(url)
                .then((response) => {
                    x.provinces = response.data;
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        getCustomerType() {
            var x = this;
            var url = "/api/customertype";
            axios
                .get(url)
                .then((response) => {
                    x.customertypes = response.data;
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        getSalesman() {
            var x = this;
            var url = "/api/salesman";
            axios
                .get(url)
                .then((response) => {
                    x.salesmans = response.data;
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        setSalesOrg(){
          var x = this
          x.form.salesorganization = x.salesmans.find(({SalesmanCode})=> SalesmanCode === x.form.salesman).SalesOrganization                   
        },
        setTerm(){
            var x =this
            x.form.customer.term = x.creditterm.find(({code}) => code === x.form.customer.creditterm).term
        },
        submit(event){            
            var x = this
            x.checkForm(event)
            console.log(x.form)

        },
        getcustomer(event){
            var x = this
            var form = x.form.customer
            var code = x.form.customercode
            var name = parthner.find(d => d.Business_Partner_ID === code) 
            var deatil =accountdetail.find(d=>d.Account === code)   
            try{
                form.name = name.OfficialName
                form.address = deatil.Address
                form.tax =  deatil.Tax
                console.log(deatil.Address)       
            }catch(error){
                console.log('by pass') 
            }
           
        },
        checkForm:function(e){
            if(this.form.customercode){
                return true;
            }
            this.error=[];
            if(!this.form.customercode){
                this.error.push('กรุณาระบุรหัสลูกค้า')
            }

            e.preventDefault();
        }
        
    },   
};
</script>

<style></style>
