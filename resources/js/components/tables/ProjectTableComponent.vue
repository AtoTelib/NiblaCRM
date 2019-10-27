<template>
     <div class="col" id="main-contract">
        <b-alert
        variant="success"
        dismissible
        fade
        :show="updated"
        @dismissed="updated=false"
        >
      projects Updated!
        </b-alert>

          <div>
             <!-- <b-spinner label="Loading..."></b-spinner> -->
        </div>

        <div >
            <div id="contract-header">
                <h4> projects </h4>
            </div>

            <div id="contract-container">
                <!-- {{ projects }} -->


                <div id="categories-table" class="table-responsive">
                    <table class="table">
                        <tr>
                            <th>ID</th>
                            <th>Icon</th>
                            <th>Name</th>
                            <th>Priority</th>
                            <th>Final Deadline</th>
                            <th>Next deadline</th>
                            <th>Current milestone</th>
                            <th>Completation Level</th>
                            <th>Status</th>
                            <th>Description</th>
                            <th>Belong to</th>
                            <th>Under The following projects</th>
                            <th>Created at</th>
                            <th>Updated at</th>
                            <th>Change Contract</th>
                            <th>Edit</th>
                            <th>Remove</th>
                        </tr>
                        <tr v-for="project in projects" :key="project.id">
                            <td>{{ project.id }}</td>
                            <td>{{ project.project_icon }}</td>
                            <td>{{ project.project_name }}</td>
                            <td>{{ project.project_priority }}</td>
                            <td>{{ project.project_final_deadline }}</td>
                            <td>{{ project.project_start_date  }}</td>
                            <td>{{ project.project_current_milestone }}</td>
                            <td>{{ project.project_cmp_level }}</td>
                            <td>{{ project.project_status }}</td>
                            <td>{{ project.project_description }}</td>
                            
                            <td> 
                                <ul>
                                    <li v-for="agent in project.agent" :key="agent.id">
                                        {{ agent.agent_name }}
                                    </li>
                                </ul> 
                            </td>

                            <td> 
                                <ul>
                                    <li v-for="contract in project.contract" :key="contract.id">
                                        {{ contract.contract_name }}
                                    </li>
                                </ul> 
                            </td>

                            <td>{{project.created_at}}</td>
                            <td>{{project.updated_at}}</td>

                            <td>meaw</td>

                            <td ><project-edit-modal-component :key=project.id :id=project.id :icon=project.project_icon :name=project.project_name :priority=project.project_priority :startDate=project.project_start_date :endDate=project.project_final_deadline :cmpLevel=project.project_cmp_level :currentMilestone=project.project_cmp_level :status=project.project_status :description=project.project_description></project-edit-modal-component></td>

                            <td><project-remove-modal-component :id=project.id ></project-remove-modal-component></td>

                        </tr>
                    </table>


                </div>

            </div>
        </div>
    </div>

</template>


<script>
  export default {
    data() {
      return {
        projects: [],
        updated:false,
        showDismissibleAlert: false

      }
    },
    created() {
        this.listenForChanges();
    },
    mounted() {
       this.fetchData();

    },
    methods: {
    fetchData(){
         fetch("/getProjects")
        .then(response => response.json())
        .then(data => (this.projects = data));
    },

    listenForChanges() {
        Echo.channel('projects')
          .listen('ProjectPublished', Contract => {
            this.fetchData();
            this.updated=true;
            this.showDismissibleAlert ? 'visible' : 'hidden'
          })
        Echo.channel('projects')
          .listen('ProjectPublished', Contract => {
            this.fetchData();
            this.updated=true;
            this.showDismissibleAlert ? 'visible' : 'hidden'

          })
        Echo.channel('projects')
          .listen('ProjectPublished', Contract => {
            this.fetchData();
            this.updated=true;
            this.showDismissibleAlert ? 'visible' : 'hidden'

          })
      }

  },

}

</script>
