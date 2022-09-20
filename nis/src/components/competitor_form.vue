<template>
    <v-container>
        <v-slide-y-transition mode="out-in">
            <v-layout row wrap>
                <v-flex xs12 sm10 offset-sm1 md10 offest-md1 lg10 offset-lg1>
                    <!-- <v-toolbar flat color="grey lighten-2">
                        <v-toolbar-title>Competitors </v-toolbar-title>
                    </v-toolbar> -->
                    <loading :active.sync="isLoading" :is-full-page="fullPage" color="#7f0000" loader="bars"></loading>
                    <!-- <v-alert type="success" :value="alert">
                        I'm a success alert.
                    </v-alert> -->
                    <v-alert :value="alert" color="primary" dark shaped border="top" icon="mdi-home"
                        transition="slide-y-transition">
                        {{this.message}}
                    </v-alert>

                    <v-toolbar flat color="grey lighten-2">
                                    <v-toolbar-title>Competitors Details</v-toolbar-title>
                                </v-toolbar>
                    <v-card-text>
                        <v-container grid-list-md>
                            <v-layout row wrap>
                                <v-flex xs12 sm6 md4>
                                    <v-autocomplete v-model="selbranch" :items="branch" label="Branch"
                                        item-text="shortCode" item-value="TEXT">
                                    </v-autocomplete>
                                </v-flex>
                                <v-flex xs12 sm6 md4>
                                    <v-select :items="comp_type" v-model="Selcomptype" label="Type"
                                        item-text="shortCode" item-value="text"></v-select>
                                </v-flex>
                                <v-flex xs12 sm6 md4>
                                    <!-- <v-text-field v-model="compName" label="Competitor Name">
                                    </v-text-field> -->
                                    <v-autocomplete v-if="competName==false" v-model="comp_Name" v-bind:items="compName"
                                        v-on:change="changeNewText" label="Competitor Name" item-text="shortCode"
                                        item-value="TEXT">
                                    </v-autocomplete>
                                    <v-text-field v-else="!(comp_Name ==null||comp_Name=='')" v-model="comp_Name"
                                        label="Competitor Name">
                                    </v-text-field>
                                </v-flex>


                                <v-flex xs12 sm6 md4>
                                    <v-textarea clearable clear-icon="cancel" label="Doctors Name"
                                        v-model='compDoctors'></v-textarea>
                                </v-flex>
                                <v-flex xs12 sm6 md4>
                                    <v-textarea clearable clear-icon="cancel" label="Services" v-model='compService'>
                                    </v-textarea>
                                </v-flex>
                                <v-flex xs12 sm6 md4>
                                    <v-textarea clearable clear-icon="cancel" label="Marketing Style"
                                        v-model='compMarketing'></v-textarea>
                                </v-flex>
                                <v-flex xs12 sm6 md3 >
                                    <v-text-field v-if="(geo==true)" v-model="comp_State" label="Competitor State">
                                    </v-text-field>
                                </v-flex>
                                <v-flex xs12 sm6 md3>
                                    <v-text-field v-if="(geo==true)" v-model="comp_City" label="Competitor City">
                                    </v-text-field>
                                </v-flex>
                                <v-flex xs12 sm6 md3>
                                    <v-text-field v-if="(geo==true)" v-model="comp_Area" label="Competitor Area">
                                    </v-text-field>
                                </v-flex>
                                <v-flex xs12 sm6 md3>
                                    <v-text-field v-if="(geo==true)" class="inputNumber" type='number' v-model="priority" clearable
                                        label="priority">
                                    </v-text-field>
                                </v-flex>
                               
                            
                            </v-layout>
                        </v-container>
                    </v-card-text>
                    <v-toolbar flat color="grey lighten-2">
                                    <v-toolbar-title>Competitor Revenue Details</v-toolbar-title>
                                </v-toolbar>
                    <v-card-text>
                        <v-container grid-list-md>
                            <v-layout row wrap>
                                <v-flex xs12 sm3>
                                    <v-menu absolute ref="menu" :close-on-content-click="false" v-model="menu"
                                        :nudge-right="40" :return-value.sync="date" lazy transition="scale-transition"
                                        offset-y full-width min-width="200px">
                                        <v-text-field slot="activator" v-model="date" placeholder="Target Month"
                                            prepend-inner-icon="event" readonly></v-text-field>
                                        <v-date-picker color="primary" v-model="date" no-title scrollable :min="minDate"
                                            :max="maxDate" type=month backgroundRevenue-color="red"
                                            style="box-shadow:none">
                                            <v-spacer></v-spacer>
                                            <v-btn flat color="primary" @click="menu = false" style="outline:none">
                                                Cancel</v-btn>
                                            <v-btn flat color="primary" @click="$refs.menu.save(date);"
                                                style="outline:none">Ok</v-btn>
                                        </v-date-picker>
                                    </v-menu>
                                </v-flex>
                                <v-flex xs12 sm3>
                                    <v-text-field class="inputNumber" type='number' v-model="opdNo" clearable
                                        label="OPD"></v-text-field>
                                </v-flex>
                                <v-flex xs12 sm3>
                                    <v-text-field class="inputNumber" type='number' v-model="catNo" clearable
                                        label="Cataract"></v-text-field>
                                </v-flex>
                                <v-flex xs12 sm9 md3>
                                    <v-text-field class="inputNumber" type='number' v-model="refractiveNo" clearable
                                        label="Refractive"></v-text-field>
                                </v-flex>
                                <v-flex xs12 sm3>
                                    <v-text-field class="inputNumber" type='number' v-model="vrNo" clearable label="VR">
                                    </v-text-field>
                                </v-flex>
                                <v-flex xs12 sm9 md3>
                                    <v-text-field class="inputNumber" type='number' v-model="vrInjNo" clearable
                                        label="VR Inj"></v-text-field>
                                </v-flex>
                                <v-flex xs12 sm9 md3>
                                    <v-text-field class="inputNumber" type='number' v-model="revenueNo" clearable
                                        label="Total Revenue"></v-text-field>
                                </v-flex>

                                <v-flex xs12>
                                    <center>
                                        <v-btn color="info" v-if=(Draftstatus) @click="saveDraft"
                                            :disabled="!submitDraftvalid">Save Draft
                                        </v-btn>
                                        <v-btn color="info" v-if=!(Draftstatus) @click="updateDraft"
                                            :disabled="!submitDraftvalid">Update Draft
                                        </v-btn>
                                        <v-btn @click="clear">clear</v-btn>
                                        <v-btn @click="back()">back</v-btn>
                                        <v-btn color="success" @click="submit" :disabled="!submitDraftvalid">Submit
                                        </v-btn>

                                    </center>

                                </v-flex>
                            </v-layout>
                        </v-container>
                    </v-card-text>

                    <v-toolbar v-if="show" flat color="grey lighten-2">
                                    <v-toolbar-title>Drafted data</v-toolbar-title>
                                </v-toolbar>
                    <v-card-text>
                        <v-container grid-list-md>
                            <v-layout row wrap>
                                <table style=" border: 1px solid;" v-if="show">
                                    <thead>
                                        <tr style=" border: 1px solid;">
                                            <th class="text-xs-center" scope="col">EDIT</th>
                                            <v-spacer></v-spacer>
                                            <th class="text-xs-center" scope="col">BRANCH</th>
                                            <v-spacer></v-spacer>
                                            <th class="text-xs-center" scope="col">COMPETITOR TYPE</th>
                                            <v-spacer></v-spacer>
                                            <th class="text-xs-center" scope="col">COMPETITOR NAME</th>
                                            <v-spacer></v-spacer>
                                            <th class="text-xs-center" scope="col">TARGET MONTH</th>
                                            <v-spacer></v-spacer>
                                            <th class="text-xs-center" scope="col">DOCTORS NAME</th>
                                            <v-spacer></v-spacer>
                                            <th class="text-xs-center" scope="col">SERVICES</th>
                                            <v-spacer></v-spacer>
                                            <th class="text-xs-center" scope="col">MARKETING ACTIVITY</th>
                                            <v-spacer></v-spacer>
                                            <th class="text-xs-center" scope="col">DRAFTED AT</th>
                                            <v-spacer></v-spacer>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr style=" border: 1px solid;" v-for="item in draftdetail" :key="item.company_name">
                                            <td>
                                                <v-btn slot="activator" small fab color="success"
                                                    @click="rowEdit(item)">
                                                    <v-icon>check</v-icon>
                                                </v-btn>
                                            </td>
                                            <v-spacer></v-spacer>
                                            <td class="text-xs-center">{{item.branch}}</td>
                                            <v-spacer></v-spacer>
                                            <td class="text-xs-center">{{item.comp_type}}</td>
                                            <v-spacer></v-spacer>
                                            <td class="text-xs-center">{{item.company_name}}</td>
                                            <v-spacer></v-spacer>
                                            <td class="text-xs-center">{{item.target_month}}</td>
                                            <v-spacer></v-spacer>
                                            <td class="text-xs-center">{{item.doctor_name}}</td>
                                            <v-spacer></v-spacer>
                                            <td class="text-xs-center">{{item.services}}</td>
                                            <v-spacer></v-spacer>
                                            <td class="text-xs-center">{{item.marketing_activity}}</td>
                                            <v-spacer></v-spacer>
                                            <td class="text-xs-center">{{item.inserted_at}}</td>


                                        </tr>

                                    </tbody>
                                </table>
                            </v-layout>

                        </v-container>
                    </v-card-text>

                </v-flex>
            </v-layout>
        </v-slide-y-transition>
    </v-container>
</template>

<script>
import moment from "moment";
import { serverBus } from "../main";
var curday = function (sp) {
    var today = new Date();
    var dd = today.getDate();
    var mm = today.getMonth() + 1; //As January is 0.
    var yyyy = today.getFullYear();

    if (dd < 10) dd = '0' + dd;
    if (mm < 10) mm = '0' + mm;
    return (yyyy + sp + mm + sp + dd);
};

export default {
    data: () => ({
        submitDraftvalid: true,
        selbranch: null,
        Selcomptype: null,
        branch: null,
        comp_type: [
            {
                shortCode: 'Group',
                text: 'group'
            },
            {
                shortCode: 'Individual',
                text: 'individual'
            },
        ],
        compName: [
            {
                shortCode: 'New',
                text: 'New Competitor'
            },
        ],
        competName: false,
        comp_Name: "",
        compDoctors: "",
        compService: "",
        compMarketing: "",
        comp_State: "",
        comp_City: "",
        comp_Area: "",
        opdNo: 0,
        catNo: 0,
        refractiveNo: 0,
        vrNo: 0,
        vrInjNo: 0,
        revenueNo: 0,
        minDate: '2022-09-01',
        menu: false,
        maxDate: curday('-'),
        date: null,
        isLoading: false,
        fullPage: true,
        show: false,
        draftdetail: null,
        //alert: true,
        alert: false,
        message: null,
        Draftstatus: true,
        cd_id: '',
        priority: 0,
        geo:false
    }),
    mounted() {
        this.loadbranch();
        this.getToday();
        this.loadCompetitor();
        this.draftrecord();
        // setTimeout(() => {
        //     this.alert = false
        // }, 5000)
        if (alert) {
            this.hide_alert();
        }

    },
    methods: {
        getToday() {
            this.today = moment().format("YYYY-MM-DD");
        },
        loadbranch() {
            let userid = JSON.parse(sessionStorage.getItem("normal_user"));
            this.isLoading = true;
            this.loading = true;
            this.axios
                .get(`http://localhost:7777/api-chbranch/${userid.userName}`).then(response => {
                    this.branch = response.data;
                    this.isLoading = false;
                })
        },
        back() {
            serverBus.$emit('changeComponent', 'competitors')
        },
        clear() {
            this.compDoctors = ''
            this.compService = '';
            this.compMarketing = '';
            this.opdNo = 0;
            this.catNo = 0;
            this.refractiveNo = 0;
            this.vrNo = 0;
            this.vrInjNo = 0;
            this.revenueNo = 0;
            this.compName = '';
            this.date = null;
            this.competName = false
            this.comp_Name = '';
            this.comp_State = "";
            this.comp_City = "";
            this.comp_Area = "";
            this.priority = 0
            this.loadCompetitor(),
            this.geo=false
            this.Draftstatus=true
        },
        saveDraft() {
            this.submitDraftvalid = false;
            if ((this.selbranch === null) || (this.selbranch === '')) {
                alert("Branch is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.Selcomptype == null) || (this.Selcomptype == '')) {
                alert("Competitor type is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.compName == null) || (this.compName == '')) {
                alert("Competitor Name is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.date == null) || (this.date == '')) {
                alert("Month is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.compDoctors == null) || (this.compDoctors === '')) {
                alert("Doctor name is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.compService == null) || (this.compService === '')) {
                alert("Service is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.compMarketing == null) || (this.compMarketing === '')) {
                alert("Marketing activity is required");
                this.submitDraftvalid = true;
                return false;
            }

            let userid = JSON.parse(sessionStorage.getItem("normal_user"));
            this.isLoading = true;
            const requestOptions = {
                headers: {
                    "Content-Type": "application/json",
                    "token": "730d84a872b5987b66d7b3b91cd8b8ca7c3c4fdcfd00022fd4d32f1f142f072fa505235a06821746edcbe430fcd9d21ce78025c5ebe647bebd6cef207a7cae2b"
                },
            };
            this.$http.post(`http://localhost:7777/api-competitors`, {
                branch: this.selbranch,
                doctorName: this.compDoctors,
                createdBy: userid.name,
                marketing: this.compMarketing,
                month: this.date,
                companyName: this.comp_Name,
                compType: this.Selcomptype,
                serviceAvail: this.compService,
                state: this.comp_State,
                area: this.comp_Area,
                city: this.comp_City,
                priority: this.priority,
                saveType: 0,
                "services": {
                    opd: this.opdNo,
                    cataract: this.catNo,
                    refractive: this.refractiveNo,
                    vr: this.vrNo,
                    vr_inj: this.vrInjNo,
                    total: this.revenueNo
                }

            }, requestOptions).then(response => {
                if (response.data.code == 200) {
                    // alert("Data Drafted successfully");
                    this.isLoading = false;
                    this.clear()
                    this.draftrecord()
                    this.submitDraftvalid = true;
                    this.alert = true
                    this.message = 'Data Drafted successfully'
                    this.alert()

                }
                else {
                    this.message = 'Error in saving data'
                    this.alert()
                    this.isLoading = false;
                }
            })

        },
        submit() {
            this.submitDraftvalid = false;
            if ((this.selbranch === null) || (this.selbranch === '')) {
                alert("Branch is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.Selcomptype == null) || (this.Selcomptype == '')) {
                alert("Competitor type is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.compName == null) || (this.compName == '')) {
                alert("Competitor Name is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.date == null) || (this.date == '')) {
                alert("Month is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.compDoctors == null) || (this.compDoctors === '')) {
                alert("Doctor name is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.compService == null) || (this.compService === '')) {
                alert("Service is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.compMarketing == null) || (this.compMarketing === '')) {
                alert("Marketing activity is required");
                this.submitDraftvalid = true;
                return false;
            }

            let userid = JSON.parse(sessionStorage.getItem("normal_user"));
            this.isLoading = true;
            const requestOptions = {
                headers: {
                    "Content-Type": "application/json",
                    "token": "730d84a872b5987b66d7b3b91cd8b8ca7c3c4fdcfd00022fd4d32f1f142f072fa505235a06821746edcbe430fcd9d21ce78025c5ebe647bebd6cef207a7cae2b"

                },
            };
            this.$http.post(`http://localhost:7777/api-competitors`, {
                branch: this.selbranch,
                doctorName: this.compDoctors,
                createdBy: userid.name,
                marketing: this.compMarketing,
                month: this.date,
                companyName: this.comp_Name,
                compType: this.Selcomptype,
                serviceAvail: this.compService,
                saveType: 1,
                state: this.comp_State,
                area: this.comp_Area,
                city: this.comp_City,
                priority: this.priority,
                "services": {
                    opd: this.opdNo,
                    cataract: this.catNo,
                    refractive: this.refractiveNo,
                    vr: this.vrNo,
                    vr_inj: this.vrInjNo,
                    total: this.revenueNo
                }

            }, requestOptions).then(response => {
                if (response.data.code == 200) {
                    this.isLoading = false;
                    this.clear()
                    this.draftrecord()
                    this.submitDraftvalid = true;
                    this.alert = true
                    this.message = 'Data Submitted successfully'
                    this.alert()

                }
                else {
                    this.message = 'Error in saving data'
                    this.alert()
                    this.isLoading = false;
                }
            })

        },
        loadCompetitor() {
            this.compName = [];
            var arr1 = [{
                shortCode: 'New',
                text: 'New Competitor'
            }];
            let userid = JSON.parse(sessionStorage.getItem("normal_user"));
            const requestOptions = {
                headers: {
                    "Content-Type": "application/json",
                    "token": "730d84a872b5987b66d7b3b91cd8b8ca7c3c4fdcfd00022fd4d32f1f142f072fa505235a06821746edcbe430fcd9d21ce78025c5ebe647bebd6cef207a7cae2b"
                },
            };
            this.axios.get(`http://localhost:7777/api-listcompetitors/${userid.name}`, requestOptions).then(response => {
                this.compName = arr1.concat(response.data.data);

            })

        },
        draftrecord() {
            let userid = JSON.parse(sessionStorage.getItem("normal_user"));

            const requestOptions = {
                headers: {
                    "Content-Type": "application/json",
                    "token": "730d84a872b5987b66d7b3b91cd8b8ca7c3c4fdcfd00022fd4d32f1f142f072fa505235a06821746edcbe430fcd9d21ce78025c5ebe647bebd6cef207a7cae2b"
                },
            }
            this.axios.get(`http://localhost:7777/api-draftcompetitors/${userid.name}`, requestOptions).then(response => {
                if (response.data.data.length > 0) {

                    this.draftdetail = response.data.data
                    this.show = true;
                }
            })
        },
        rowEdit(a) {
            this.Draftstatus = false
            this.selbranch = a.branch
            this.Selcomptype = a.comp_type
            this.comp_Name = a.company_name
            this.compDoctors = a.doctor_name
            this.compService = a.services
            this.compMarketing = a.marketing_activity
            this.date = a.target_month
            this.opdNo = a.opd
            this.catNo = a.cataract
            this.refractiveNo = a.refractive
            this.vrNo = a.vr
            this.vrInjNo = a.vr_inj
            this.revenueNo = a.revenue
            this.cd_id = a.cd_id
            this.comp_State = a.state
            this.comp_Area = a.area
            this.comp_City = a.city
            this.priority = a.priority
        },
        changeNewText(a) {
            if (a == 'New') {
                this.competName = true
                this.geo=true
            }
        },
        hide_alert() {
            window.setInterval(() => {
                this.alert = false;
            }, 3000)
        },
        updateDraft() {
            this.submitDraftvalid = false;
            if ((this.selbranch === null) || (this.selbranch === '')) {
                alert("Branch is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.Selcomptype == null) || (this.Selcomptype == '')) {
                alert("Competitor type is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.compName == null) || (this.compName == '')) {
                alert("Competitor Name is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.date == null) || (this.date == '')) {
                alert("Month is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.compDoctors == null) || (this.compDoctors === '')) {
                alert("Doctor name is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.compService == null) || (this.compService === '')) {
                alert("Service is required");
                this.submitDraftvalid = true;
                return false;
            }
            if ((this.compMarketing == null) || (this.compMarketing === '')) {
                alert("Marketing activity is required");
                this.submitDraftvalid = true;
                return false;
            }

            let userid = JSON.parse(sessionStorage.getItem("normal_user"));
            // this.isLoading = true;
            const requestOptions = {
                headers: {
                    "Content-Type": "application/json",
                    "token": "730d84a872b5987b66d7b3b91cd8b8ca7c3c4fdcfd00022fd4d32f1f142f072fa505235a06821746edcbe430fcd9d21ce78025c5ebe647bebd6cef207a7cae2b"
                },
            };
            this.axios.put(`http://localhost:7777/api-competitors/${this.cd_id}`, {
                branch: this.selbranch,
                doctorName: this.compDoctors,
                createdBy: userid.name,
                marketing: this.compMarketing,
                month: this.date,
                companyName: this.comp_Name,
                compType: this.Selcomptype,
                serviceAvail: this.compService,
                priority: this.priority,
                saveType: 0,
                "services": {
                    opd: this.opdNo,
                    cataract: this.catNo,
                    refractive: this.refractiveNo,
                    vr: this.vrNo,
                    vr_inj: this.vrInjNo,
                    total: this.revenueNo
                }
            }, requestOptions).then(response => {
                if (response.data.code == 200) {
                    this.isLoading = false;
                    this.clear()
                    this.draftrecord()
                    this.submitDraftvalid = true;
                    this.alert = true
                    this.message = 'Data Drafted successfully'
                    this.alert()
                }
                else {
                    this.message = 'Error in saving data'
                    this.alert()
                    this.isLoading = false;
                }
            })

        }


    },

}
</script>

<style>
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

/* Firefox */
input[type=number] {
    -moz-appearance: textfield;
}
</style>