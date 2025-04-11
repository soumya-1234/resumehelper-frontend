<template>
    <div class="job-update">
        <div class="container py-5">
            <!-- Header -->
            <div class="section-header text-center mb-5">
                <h1 class="display-5 mb-3">Update Job Posting</h1>
                <p class="lead text-muted">Modify your existing job opportunity</p>
            </div>

            <div class="form-wrapper">
                <div class="card border-0 shadow-sm">
                    <div class="card-header bg-primary text-white border-0 py-4">
                        <div class="d-flex align-items-center">
                            <div class="icon-circle me-3">
                                <i class="fas fa-edit fa-2x"></i>
                            </div>
                            <h2 class="h4 mb-0">Edit Job Details</h2>
                        </div>
                    </div>

                    <div class="card-body p-4">
                        <!-- Alerts -->
                        <div class="alerts mb-4">
                            <div class="alert alert-danger d-flex align-items-center" v-if="bool" role="alert">
                                <i class="fas fa-exclamation-circle me-2"></i>
                                <div>Empty fields are not allowed</div>
                            </div>
                            <div class="alert alert-success d-flex align-items-center" v-if="bool1" role="alert">
                                <i class="fas fa-check-circle me-2"></i>
                                <div>{{message}} Successfully</div>
                            </div>
                        </div>

                        <form @submit.prevent="jdetails">
                            <!-- Job Title -->
                            <div class="form-group mb-4">
                                <label class="form-label">
                                    <i class="fas fa-bookmark me-2 text-primary"></i>
                                    Job Title
                                </label>
                                <input 
                                    type="text" 
                                    class="form-control form-control-lg" 
                                    v-model.trim="formdata.jtitle"
                                    placeholder="e.g. Senior Software Engineer"
                                />
                            </div>

                            <!-- Skills -->
                            <div class="form-group mb-4">
                                <label class="form-label">
                                    <i class="fas fa-tools me-2 text-primary"></i>
                                    Required Skills
                                </label>
                                <input 
                                    type="text" 
                                    class="form-control form-control-lg" 
                                    v-model.trim="formdata.skills"
                                    placeholder="e.g. JavaScript, React, Node.js"
                                />
                                <small class="form-text text-muted">
                                    Separate multiple skills with commas
                                </small>
                            </div>

                            <!-- Job Description -->
                            <div class="form-group mb-4">
                                <label class="form-label">
                                    <i class="fas fa-align-left me-2 text-primary"></i>
                                    Job Description
                                </label>
                                <textarea 
                                    class="form-control form-control-lg" 
                                    v-model.trim="formdata.jdesc"
                                    rows="5"
                                    placeholder="Describe the role, responsibilities, and requirements..."
                                ></textarea>
                            </div>

                            <!-- Submit Button -->
                            <div class="d-flex justify-content-between align-items-center mt-5" v-if="bool2">
                                <router-link 
                                    :to="{path: '/recrhome', query: {rid: ssid}}"
                                    class="btn btn-outline-primary">
                                    <i class="fas fa-arrow-left me-2"></i>
                                    Back to Dashboard
                                </router-link>
                                <button 
                                    type="submit"
                                    class="btn btn-warning">
                                    <i class="fas fa-save me-2"></i>
                                    Update Job Posting
                                </button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: "RecrUpdate",
    data() {
        return {
            formdata: {
                rid: "",
                jtitle: "",
                skills: "",
                jdesc: ""
            },
            bool: false,
            bool1: false,
            bool2: true,
            data: [],
            ssid: this.$route.query.rid,
            bid: "",
            message: "",
            objid: ""
        }
    },
    async beforeMount() {
        try {
            const response = await axios.get('https://resumehelpbackend.onrender.com/recruiters/jobdetget')
            this.formatposts(response.data)
            this.checkdata()
        } catch (error) {
            console.error('Error fetching job details:', error)
        }
    },
    methods: {
        formatposts(jobdetdata) {
            this.data = Object.keys(jobdetdata).map(key => ({
                ...jobdetdata[key],
                id: key
            }))
        },
        checkdata() {
            const job = this.data.find(item => item.rid === this.ssid)
            if (job) {
                this.formdata.jtitle = job.jtitle
                this.formdata.skills = job.skills
                this.formdata.jdesc = job.jdesc
                this.objid = job._id
            }
        },
        async jdetails() {
            this.formdata.rid = this.ssid
            this.bool = false
            this.bool1 = false

            if (!this.formdata.jtitle || !this.formdata.skills || !this.formdata.jdesc) {
                this.bool = true
                return
            }

            try {
                await axios.put(`https://resumehelpbackend.onrender.com/recruiters/jobupdate/${this.objid}`, this.formdata)
                this.bool1 = true
                this.message = "Updated"
            } catch (error) {
                console.error('Error updating job:', error)
                this.bool = true
                this.message = "Error updating job"
            }
        }
    }
}
</script>

<style scoped>
.job-update {
    background-color: var(--bg-light);
    min-height: 100vh;
    font-family: 'Inter', sans-serif;
}

.form-wrapper {
    max-width: 800px;
    margin: 0 auto;
}

.card {
    border-radius: 1rem;
    overflow: hidden;
    transition: transform 0.3s ease;
}

.card:hover {
    transform: translateY(-5px);
}

.card-header {
    background: linear-gradient(45deg, var(--primary-color), var(--secondary-color));
}

.icon-circle {
    width: 48px;
    height: 48px;
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.form-control {
    border: 2px solid #e2e8f0;
    padding: 0.75rem 1rem;
    font-size: 1rem;
    border-radius: 0.5rem;
    transition: all 0.3s ease;
}

.form-control:focus {
    border-color: var(--primary-color);
    box-shadow: 0 0 0 0.2rem rgba(37, 99, 235, 0.1);
}

.form-label {
    font-weight: 500;
    color: var(--text-primary);
    margin-bottom: 0.5rem;
}

textarea.form-control {
    resize: vertical;
    min-height: 120px;
}

.btn {
    padding: 0.75rem 1.5rem;
    font-weight: 500;
    border-radius: 0.5rem;
    transition: all 0.3s ease;
}

.btn:hover {
    transform: translateY(-2px);
}

.btn-warning {
    background: linear-gradient(45deg, #f59e0b, #fbbf24);
    border: none;
    color: white;
}

.btn-outline-primary {
    border: 2px solid var(--primary-color);
    color: var(--primary-color);
}

.btn-outline-primary:hover {
    background: linear-gradient(45deg, var(--primary-color), var(--secondary-color));
    color: white;
    border: 2px solid transparent;
}

.alert {
    border: none;
    border-radius: 0.5rem;
}

.alert-danger {
    background-color: rgba(239, 68, 68, 0.1);
    color: #dc2626;
}

.alert-success {
    background-color: rgba(34, 197, 94, 0.1);
    color: #16a34a;
}

@media (max-width: 768px) {
    .form-wrapper {
        margin: 0 1rem;
    }

    .card-header {
        padding: 1.5rem;
    }

    .icon-circle {
        width: 40px;
        height: 40px;
    }

    .btn {
        width: 100%;
        margin-bottom: 1rem;
    }

    .d-flex.justify-content-between {
        flex-direction: column-reverse;
    }
}
</style>