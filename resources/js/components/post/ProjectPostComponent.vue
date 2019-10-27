<template>
      <div class="">
        <b-alert :show="dismissCountDown" dismissible variant="danger" @dismissed="dismissCountDown=0" @dismiss-count-down="countDownChanged">
      {{ errorMsg }}
      <b-progress
        variant="danger"
        :max="dismissSecs"
        :value="dismissCountDown"
        height="4px"
      ></b-progress>
        </b-alert>
        <div class="form-group">
          <label for="projectName">Project Name</label>
          <input v-model="projectName" id="projectName" type="text" class="form-control">
        </div>


        <div class="form-group">
          <label for="projectPriority">Project Priority</label>
          <input v-model="projectPriority" id="projectPriority" type="number" class="form-control">
        </div>

        <!-- <div class="form-group">
          <label for="projectStartDate">Contract End Date</label>
          <input v-model="projectStartDate" id="projectStartDate" class="form-control">
        </div> -->

        <div class="container">
        <div class="row">
              <div class="col-sm form-group">
                <label for="projectStartDate">Project Start Date</label>
                <input v-model="projectStartDate" class="form-control" type="date" id="start" name="projectStartDate">
            </div>

            <div class="col-sm form-group">
                <label for="projectFinalDeadline">Project Final Deadline</label>
                <input v-model="projectFinalDeadline" type="date" id="projectFinalDeadline" class="form-control">
        </div>
                       <!-- <div class="col-sm">
            One of three columns
            </div>
            <div class="col-sm">
            One of three columns
            </div> -->
        </div>
        </div>
        <div class="form-group">
            <label for="projectStatus">Project Status</label>
            <!-- <input v-model="projectStatus" id="projectStatus" class="form-control"> -->
            <select v-model="projectStatus" name="carlist" class="form-control" form="carform">
                <option v-for="statusaya in statusList" :key="statusaya.id" class="form-group">
                    {{ statusaya.status_name }}
                </option>
            </select>
        </div>

        <div class="form-group">
          <label for="projectDescription">Project Description</label>
          <input v-model="projectDescription" id="projectDescription" class="form-control">
        </div>

        <div class="form-group">
            <label for="projectContract">Project Contracts</label>
            <!-- <input v-model="projectContract" id="projectContract" class="form-control"> -->
            <select v-model="projectContract" name="carlist" class="form-control" form="carform">
                <option v-for="contract in contractList" :value="contract.id" :key="contract.id" class="form-group">
                    {{ contract.contract_name }}
                </option>
            </select>
        </div>

        <button @click="addContract()"
          :class="{disabled: (!projectName || !projectFinalDeadline || !projectStartDate || !projectStatus || !projectDescription || !projectContract)}"
          class="btn btn-block btn-success">Submit</button>
      </div>

</template>

<script>

  export default {
    data() {
      return {
        projectName: "",
        projectPriority:"",
      projectFinalDeadline: "",
      projectStartDate: "",
      projectStatus: "",
      projectDescription: "",
      projectContract: "",
      errorMsg: "",
      dismissSecs: 10,
      dismissCountDown: 0,
      contractList:[],
      statusList:[]

      }
    },
    created() {
      //this.listenForChanges();
    },
    mounted() {
        fetch("/getStatus")
        .then(response => response.json())
        .then(data => (this.statusList = data));
        fetch("/getContracts")
        .then(response => response.json())
        .then(data => (this.contractList = data));
    },
    methods: {
        countDownChanged(dismissCountDown) {
            this.dismissCountDown = dismissCountDown
        },
        showAlert() {
            this.dismissCountDown = this.dismissSecs
        },
        addContract() {
           // alert("hELLO DARKNESS my old friend")
            this.$loading.value = true;
            //alert("hi");
            // check if entries are not empty
            if(!this.projectName ||!this.projectPriority ||  !this.projectFinalDeadline || !this.projectStartDate || !this.projectStatus || !this.projectDescription)
            return;
            //console.log(projectFinalDeadline < projectStartDate ? 'valid' : 'invalid');

            if(this.projectFinalDeadline < this.projectStartDate) {
                this.showAlert();
                this.errorMsg = "Final date cannot be less than end date"
                this.showDismissibleAlert ? true : false
                return;
            }
            else{
              //  console.log("OUH")
            }
            //alert("asd");
            // make API to save Contract
            axios.post('/addProject', {
            projectName: this.projectName,projectPriority: this.projectPriortiy, projectFinalDeadline: this.projectFinalDeadline , projectStartDate: this.projectStartDate, projectStatus: this.projectStatus, projectDescription: this.projectDescription, projectContract:this.projectContract
            }).then( response => {
            if(response.data) {
                //location.reload();
                this.projectName = this.projectFinalDeadline = this.projectStartDate = this.projectStatus = this.projectDescription = "";
            }
            else{
            }
            })
        },
    },
    }
</script>
