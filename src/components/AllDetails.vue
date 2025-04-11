<template>
    <div class="all-details">
        <div class="container py-4">
            <!-- Basic Details -->
            <div class="card border-0 shadow-sm mb-4 hover-card">
                <div class="card-header bg-primary text-white border-0 py-3">
                    <div class="d-flex align-items-center">
                        <i class="fas fa-user-circle fa-2x me-3"></i>
                        <h2 class="mb-0">Basic Details</h2>
                    </div>
                </div>
                <div class="card-body p-4">
                    <div class="row g-4">
                        <div class="col-md-6">
                            <div class="detail-item">
                                <label class="text-muted mb-1">Name</label>
                                <h3 class="detail-value">{{$route.query.sname}}</h3>
                            </div>
                        </div>
                        <div class="col-md-6">
                            <div class="detail-item">
                                <label class="text-muted mb-1">Website</label>
                                <h3 class="detail-value">{{website}}</h3>
                            </div>
                        </div>
                        <div class="col-md-6">
                            <div class="detail-item">
                                <label class="text-muted mb-1">Address</label>
                                <h3 class="detail-value">{{address}}</h3>
                            </div>
                        </div>
                        <div class="col-md-6">
                            <div class="detail-item">
                                <label class="text-muted mb-1">Mobile Number</label>
                                <h3 class="detail-value">{{mobile}}</h3>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Academic Details -->
            <div class="card border-0 shadow-sm mb-4 hover-card">
                <div class="card-header bg-primary text-white border-0 py-3">
                    <div class="d-flex align-items-center">
                        <i class="fas fa-graduation-cap fa-2x me-3"></i>
                        <h2 class="mb-0">Academic Details</h2>
                    </div>
                </div>
                <div class="card-body p-4">
                    <div class="row g-4">
                        <div class="col-md-4">
                            <div class="detail-item">
                                <label class="text-muted mb-1">10th Percentage</label>
                                <h3 class="detail-value">{{percentage}}%</h3>
                            </div>
                        </div>
                        <div class="col-md-4">
                            <div class="detail-item">
                                <label class="text-muted mb-1">12th Percentage</label>
                                <h3 class="detail-value">{{percentage1}}%</h3>
                            </div>
                        </div>
                        <div class="col-md-4">
                            <div class="detail-item">
                                <label class="text-muted mb-1">Graduation Percentage</label>
                                <h3 class="detail-value">{{percentage2}}%</h3>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Skills Details -->
            <div class="card border-0 shadow-sm mb-4 hover-card">
                <div class="card-header bg-primary text-white border-0 py-3">
                    <div class="d-flex align-items-center">
                        <i class="fas fa-tools fa-2x me-3"></i>
                        <h2 class="mb-0">Skills</h2>
                    </div>
                </div>
                <div class="card-body p-4">
                    <div class="skills-wrapper">
                        <div v-for="(skill, index) in skillsList" :key="index" class="skill-tag">
                            {{skill}}
                        </div>
                    </div>
                </div>
            </div>

            <!-- Project Details -->
            <div class="card border-0 shadow-sm mb-4 hover-card">
                <div class="card-header bg-primary text-white border-0 py-3">
                    <div class="d-flex align-items-center">
                        <i class="fas fa-project-diagram fa-2x me-3"></i>
                        <h2 class="mb-0">Project Details</h2>
                    </div>
                </div>
                <div class="card-body p-4">
                    <div class="project-details">
                        <div class="detail-item mb-4">
                            <label class="text-muted mb-1">Project Name</label>
                            <h3 class="detail-value">{{projname}}</h3>
                        </div>
                        <div class="detail-item mb-4">
                            <label class="text-muted mb-1">Technologies Used</label>
                            <div class="tech-tags">
                                <span v-for="(tech, index) in techList" :key="index" class="tech-tag">
                                    {{tech}}
                                </span>
                            </div>
                        </div>
                        <div class="detail-item">
                            <label class="text-muted mb-1">Description</label>
                            <p class="detail-value mb-0">{{projdetails}}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: "AllDetails",
    data() {
        return {
            bdata: [],
            adata: [],
            sdata: [],
            pdata: [],
            ssid: this.$route.query.sid,
            website: "",
            mobile: "",
            address: "",
            percentage: null,
            percentage1: null,
            percentage2: null,
            skill: "",
            projname: "",
            techno: "",
            projdetails: ""
        }
    },
    computed: {
        skillsList() {
            return this.skill ? this.skill.split(',').map(s => s.trim()) : []
        },
        techList() {
            return this.techno ? this.techno.split(',').map(t => t.trim()) : []
        }
    },
    beforeMount(){
        axios.get('https://resumehelpbackend.onrender.com/students/basicdetget')
        .then((response)=>{
            this.formatPosts(response.data)
            this.checksid()
        })
        axios.get('https://resumehelpbackend.onrender.com/students/academicdetget')
        .then((response)=>{
            this.formatPosts1(response.data)
            this.checksid1()
        })
        axios.get('https://resumehelpbackend.onrender.com/students/skilldetget')
        .then((response)=>{
            this.formatPosts2(response.data)
            this.checksid2()
        })
        axios.get('https://resumehelpbackend.onrender.com/students/projectdetget')
        .then((response)=>{
            this.formatPosts3(response.data)
            this.checksid3()
        })
    },
    methods:{
        formatPosts(postData){
            for (let key in postData){
                this.bdata.push({...postData[key],id:key});
            }
        },
        formatPosts1(postData){
            for (let key in postData){
                this.adata.push({...postData[key],id:key});
            }
        },
        formatPosts2(postData){
            for (let key in postData){
                this.sdata.push({...postData[key],id:key});
            }
        },
        formatPosts3(postData){
            for (let key in postData){
                this.pdata.push({...postData[key],id:key});
            }
        },
        checksid(){
            for(var i=0;i<this.bdata.length;i++){
                if(this.bdata[i].sid==this.ssid){
                    this.website=this.bdata[i].website
                    this.mobile=this.bdata[i].mobile
                    this.address=this.bdata[i].address
                }
            }
        },
        checksid1(){
            for(var i=0;i<this.adata.length;i++){
                if(this.adata[i].sid==this.ssid){
                    this.percentage=this.adata[i].percentage
                    this.percentage1=this.adata[i].percentage1
                    this.percentage2=this.adata[i].percentage2
                }
            }
        },
        checksid2(){
            for(var i=0;i<this.sdata.length;i++){
                if(this.sdata[i].sid==this.ssid){
                    this.skill=this.sdata[i].skill
                }
            }
        },
        checksid3(){
            for(var i=0;i<this.pdata.length;i++){
                if(this.pdata[i].sid==this.ssid){
                    this.projname=this.pdata[i].projname
                    this.techno=this.pdata[i].techno
                    this.projdetails=this.pdata[i].projdetails
                }
            }
        }
    }
}
</script>

<style scoped>
.all-details {
    padding-top: 2rem;
    background-color: var(--bg-light);
    min-height: 100vh;
}

.hover-card {
    transition: transform 0.3s ease;
    border-radius: 1rem;
    overflow: hidden;
}

.hover-card:hover {
    transform: translateY(-5px);
}

.card-header {
    border-bottom: none;
}

.detail-item {
    margin-bottom: 1rem;
}

.detail-item label {
    font-size: 0.875rem;
    font-weight: 500;
    color: var(--text-secondary);
    text-transform: uppercase;
    letter-spacing: 0.5px;
}

.detail-value {
    font-size: 1.125rem;
    font-weight: 500;
    color: var(--text-primary);
    margin: 0;
}

.skills-wrapper {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
}

.skill-tag {
    background-color: rgba(37, 99, 235, 0.1);
    color: var(--primary-color);
    padding: 0.5rem 1rem;
    border-radius: 2rem;
    font-weight: 500;
    font-size: 0.875rem;
}

.tech-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
}

.tech-tag {
    background-color: rgba(37, 99, 235, 0.1);
    color: var(--primary-color);
    padding: 0.5rem 1rem;
    border-radius: 2rem;
    font-weight: 500;
    font-size: 0.875rem;
}

@media (max-width: 768px) {
    .all-details {
        padding-top: 1rem;
    }

    .detail-value {
        font-size: 1rem;
    }

    .card-header h2 {
        font-size: 1.25rem;
    }
}
</style>