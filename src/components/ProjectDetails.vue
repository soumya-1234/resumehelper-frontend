<template>
    <div class="project-details">
        <div class="container py-5">
            <!-- Header -->
            <div class="section-header text-center mb-5">
                <h1 class="display-5 mb-3">Project Details</h1>
                <p class="lead text-muted">Showcase your projects and technical expertise</p>
            </div>

            <div class="form-wrapper">
                <div class="card border-0 shadow-sm">
                    <div class="card-header bg-primary text-white border-0 py-4">
                        <div class="d-flex align-items-center">
                            <div class="icon-circle me-3">
                                <i class="fas fa-project-diagram fa-2x"></i>
                            </div>
                            <h2 class="h4 mb-0">{{bool2 ? 'Add New Project' : 'Update Project'}}</h2>
                        </div>
                    </div>

                    <div class="card-body p-4">
                        <!-- Alert Messages -->
                        <div class="alerts mb-4">
                            <div class="alert alert-danger alert-dismissible fade show" v-if="bool" role="alert">
                                <i class="fas fa-exclamation-circle me-2"></i>
                                Empty fields are not allowed
                                <button type="button" class="btn-close" @click="bool = false"></button>
                            </div>
                            <div class="alert alert-success alert-dismissible fade show" v-if="bool1" role="alert">
                                <i class="fas fa-check-circle me-2"></i>
                                {{message}} Successfully
                                <button type="button" class="btn-close" @click="bool1 = false"></button>
                            </div>
                        </div>

                        <!-- Project Form -->
                        <form @submit.prevent="bool2 ? projdata() : projdataupdate()">
                            <!-- Project Name -->
                            <div class="form-group mb-4">
                                <label class="form-label">Project Name</label>
                                <div class="input-group">
                                    <span class="input-group-text">
                                        <i class="fas fa-bookmark"></i>
                                    </span>
                                    <input 
                                        type="text" 
                                        class="form-control" 
                                        v-model.trim="formdata.projname"
                                        placeholder="Enter project name"
                                    />
                                </div>
                            </div>

                            <!-- Technologies -->
                            <div class="form-group mb-4">
                                <label class="form-label">Technologies Used</label>
                                <div class="input-group">
                                    <span class="input-group-text">
                                        <i class="fas fa-code"></i>
                                    </span>
                                    <input 
                                        type="text" 
                                        class="form-control" 
                                        v-model.trim="formdata.techno"
                                        placeholder="e.g. React, Node.js, MongoDB"
                                    />
                                </div>
                                <small class="form-text text-muted">
                                    Separate technologies with commas
                                </small>
                            </div>

                            <!-- Project Details -->
                            <div class="form-group mb-4">
                                <label class="form-label">Project Details</label>
                                <div class="input-group">
                                    <span class="input-group-text">
                                        <i class="fas fa-align-left"></i>
                                    </span>
                                    <textarea 
                                        class="form-control" 
                                        rows="4"
                                        v-model.trim="formdata.projdetails"
                                        placeholder="Describe your project, its features, and your role"
                                    ></textarea>
                                </div>
                            </div>

                            <!-- Submit/Update Button -->
                            <div class="d-grid">
                                <button 
                                    type="submit"
                                    :class="['btn', 'btn-lg', bool2 ? 'btn-primary' : 'btn-warning']">
                                    <i :class="['fas', 'me-2', bool2 ? 'fa-plus-circle' : 'fa-edit']"></i>
                                    {{bool2 ? 'Add Project' : 'Update Project'}}
                                </button>
                            </div>
                        </form>

                        <!-- Back Button -->
                        <div class="text-center mt-4">
                            <router-link 
                                :to="{path: '/studenthome', query: {sid: ssid, sname: $route.query.sname}}"
                                class="btn btn-outline-primary">
                                <i class="fas fa-arrow-left me-2"></i>
                                Back to Dashboard
                            </router-link>
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
    name: "ProjectDetails",
    data() {
        return {
            formdata: {
                sid: "",
                projname: "",
                techno: "",
                projdetails: ""
            },
            bool: false,
            bool1: false,
            bool2: true,
            ssid: this.$route.query.sid,
            projid: "",
            message: "",
            data: []
        }
    },
    beforeMount() {
        axios.get('https://resumehelpbackend.onrender.com/students/projectdetget')
            .then((response) => {
                this.formatPosts(response.data)
                this.checksid()
            })
            .catch(error => {
                console.error('Error fetching project details:', error)
            })
    },
    props: {
        msg: String
    },
    methods: {
        async projdata() {
            this.formdata.sid = this.msg
            if (!this.validateForm()) return

            try {
                await axios.post("https://resumehelpbackend.onrender.com/students/projectdetcreate", this.formdata)
                this.handleSuccess('Submitted')
                this.resetForm()
            } catch (error) {
                console.error('Error creating project:', error)
                this.handleError()
            }
        },
        formatPosts(postData) {
            this.data = Object.keys(postData).map(key => ({
                ...postData[key],
                id: key
            }))
        },
        checksid() {
            const project = this.data.find(item => item.sid === this.ssid)
            if (project) {
                this.bool2 = false
                this.formdata = {
                    ...this.formdata,
                    projname: project.projname,
                    techno: project.techno,
                    projdetails: project.projdetails
                }
                this.projid = project._id
            }
        },
        async projdataupdate() {
            if (!this.validateForm()) return

            this.formdata.sid = this.ssid
            try {
                await axios.put(`https://resumehelpbackend.onrender.com/students/projectupdate/${this.projid}`, this.formdata)
                this.handleSuccess('Updated')
            } catch (error) {
                console.error('Error updating project:', error)
                this.handleError()
            }
        },
        validateForm() {
            if (!this.formdata.projname || !this.formdata.techno || !this.formdata.projdetails) {
                this.bool = true
                this.bool1 = false
                return false
            }
            return true
        },
        handleSuccess(message) {
            this.bool = false
            this.bool1 = true
            this.message = message
        },
        handleError() {
            this.bool = true
            this.bool1 = false
            this.message = 'An error occurred'
        },
        resetForm() {
            this.formdata = {
                ...this.formdata,
                projname: "",
                techno: "",
                projdetails: ""
            }
        }
    }
}
</script>

<style scoped>
.project-details {
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
    width: 40px;
    height: 40px;
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.form-label {
    font-weight: 500;
    color: var(--text-primary);
    margin-bottom: 0.5rem;
}

.input-group-text {
    background-color: rgba(37, 99, 235, 0.1);
    border: none;
    color: var(--primary-color);
}

.form-control {
    border: 1px solid rgba(0, 0, 0, 0.1);
    padding: 0.75rem;
    font-size: 0.9375rem;
    transition: all 0.3s ease;
}

.form-control:focus {
    border-color: var(--primary-color);
    box-shadow: 0 0 0 0.2rem rgba(37, 99, 235, 0.25);
}

.alert {
    border: none;
    border-radius: 0.5rem;
}

.alert-success {
    background-color: rgba(34, 197, 94, 0.1);
    color: #16a34a;
}

.alert-danger {
    background-color: rgba(239, 68, 68, 0.1);
    color: #dc2626;
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

.btn-close {
    font-size: 0.75rem;
}

@media (max-width: 768px) {
    .form-wrapper {
        margin: 0 1rem;
    }

    .card-header {
        padding: 1.5rem;
    }

    .icon-circle {
        width: 36px;
        height: 36px;
    }

    .form-control {
        font-size: 0.875rem;
    }
}
</style>